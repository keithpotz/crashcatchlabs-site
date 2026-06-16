# Engineer Mode — Implementation Notes
**Date:** March 30 – April 1, 2026

---

## What Was Built

Engineer Mode is a new analysis tab (`⚙ Engineer`, `Ctrl+0`) that generates a complete debug session plan from a crash dump. It is distinct from Explain Mode in both purpose and implementation.

| | Explain Mode | Engineer Mode |
|---|---|---|
| **Purpose** | Plain-English "what happened" | Technical "how do I fix it" |
| **Output** | Root cause, evidence, suggested fix | Hypotheses, WinDbg plan, search hints, reproduction, fix checklist, blast radius |
| **Implementation** | Claude Haiku API (requires API key) | Deterministic Rust engine (no API, no key, offline) |
| **Speed** | Network round trip | Instant |
| **IP** | External AI | Proprietary engine |

---

## Design Decision: No AI

Engineer Mode was initially prototyped as a Claude Sonnet API call. It was rebuilt as a pure Rust deterministic engine because:

- Every output it generates (WinDbg commands, search hints, fix steps) can be derived directly from the crash dump data — no AI reasoning needed
- Claude is not a moat — anyone can prompt an LLM with a crash dump
- A rule-based Rust engine is proprietary IP that cannot be replicated
- Works fully offline, no API key required, no per-use cost, instant results

---

## Files Changed

### New
- `src-tauri/src/engineer.rs` — full deterministic analysis engine (~600 lines)

### Modified
- `src-tauri/src/lib.rs` — added `mod engineer`, `engineer_crash` Tauri command (synchronous, takes `crash_json: String`)
- `index.html` — added `⚙ Engineer` tab button (amber), `⚙ Engineer Mode` View menu entry (`Ctrl+0`)
- `src/main.ts` — `EngineerReport` interfaces, state vars, `renderEngineer()`, `generateEngineerReport()`, click handlers, keyboard shortcut, PDF export section
- `src/styles.css` — full Engineer Mode component styles (hypothesis cards, WinDbg step rows, search hint rows, fix checklist, blast radius badge)

---

## Output Schema

```typescript
interface EngineerReport {
  hypotheses: {
    title: string;
    confidence: number;        // 0–100
    supporting: string[];
    contradicting: string[];
    how_to_confirm: string;    // specific WinDbg command or action
  }[];
  windbg_plan: {
    step: number;
    command: string;           // exact WinDbg syntax
    purpose: string;
  }[];
  search_hints: {
    pattern: string;           // grep-compatible pattern
    reason: string;
  }[];
  reproduction: {
    conditions: string[];
    is_timing_dependent: boolean;
    is_memory_pressure_related: boolean;
  };
  fix_checklist: string[];
  blast_radius: {
    severity: 'low' | 'medium' | 'high' | 'critical';
    scope: string;
    note: string;
  };
}
```

---

## Crash Classes Covered

The engine detects and produces tailored output for 10 crash classes:

| Class | Exception Code | Detection Method |
|---|---|---|
| **Null Pointer Dereference** | `0xC0000005` | Target address == 0x0, null registers |
| **Stack Overflow** | `0xC00000FD` | Exception code + repeated frame heuristic |
| **Use-After-Free** | `0xC0000005` | Deallocation function in crashing stack |
| **Heap Corruption** | `0xC0000374` | Exception code |
| **Deadlock** | any | All non-crashing threads in wait functions |
| **Abort / Assert** | `0x40000015` | Exception code |
| **Pure Virtual Call** | any | `__purecall` in crashing stack |
| **Security Cookie / Stack Overrun** | `0xC0000409` | Exception code |
| **Out of Memory** | `0xC0000017` | Exception code |
| **Access Violation (generic)** | `0xC0000005` | Fallback for non-null AV |

**Unreal Engine supplement** — layered on top of any class:
- `ensure()` failure: detects `FDebug::EnsureFailed` in stack
- `check()` failure: detects `FDebug::CheckVerifyFailed` in stack

---

## What Makes the Output Proprietary

### WinDbg commands use real dump data
Commands include actual OS thread IDs (`~~[0x1a4c]s`), actual null registers (`dt Module!Type @rcx`), and actual target addresses (`!heap -p -a 0x...`). Not generic advice — specific to the dump being analysed.

### Hypotheses ranked by evidence
Multiple competing theories ranked by confidence (0–100), each with supporting and contradicting evidence pulled from registers, exception codes, stack frames, and thread state.

### Type inference from stack frames
Extracts type names from C++ qualified function names (`AEnemy::Tick` → `AEnemy`) to generate specific `dt` commands and search patterns.

### User-code filtering
System modules (`ntdll`, `kernel32`, `ucrtbase`, etc.) are filtered out — search hints and analysis focus on the user's actual code.

### UE-aware fix checklist
Null deref fixes suggest `TWeakObjectPtr` + `IsValid()` for UE projects, `std::shared_ptr` + null check for non-UE. Fix advice is context-specific.

---

## Tauri Command

```rust
// lib.rs
#[tauri::command]
fn engineer_crash(crash_json: String) -> Result<String, String> {
    let report = engineer::analyze(&crash_json)?;
    serde_json::to_string(&report).map_err(|e| e.to_string())
}
```

- **Synchronous** (no `async`) — deterministic, no I/O
- Input: full `CrashReport` JSON string (same format as `CrashCatchCore` output)
- Output: `EngineerReport` JSON string

---

## Frontend Invocation

```typescript
const raw = await invoke<string>('engineer_crash', {
  crashJson: JSON.stringify(currentReport),
});
```

No API key. No network. Button always enabled.

---

## PDF Export

Engineer Mode output is included in the PDF report when a report has been generated:
- Hypotheses rendered as cards with confidence %
- WinDbg plan as a numbered table (command + purpose)
- Search hints table
- Reproduction conditions list with timing/memory flags
- Fix checklist as ordered list
- Blast radius with severity badge (color-coded: critical = red, high = orange, medium = yellow, low = green)

---

## Keyboard & Navigation

- **Tab button**: `⚙ Engineer` (amber color, after `◈ Intel` in tab bar)
- **Keyboard shortcut**: `Ctrl+0`
- **View menu**: `⚙ Engineer Mode` → `Ctrl+0`
- **Data action**: `tab-engineer`
