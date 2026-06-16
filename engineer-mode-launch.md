# Engineer Mode Launch — Marketing Content
**v0.3.0-beta · April 2026**

---

## Twitter / X Posts

Post these across a week. Do not post more than one per day.

---

**Post 1 — Launch announcement (lead with the problem)**

Drop a crash dump into CrashCatch and you get a stack trace, exception code, and crash classification in seconds.

v0.3.0-beta adds Engineer Mode: press Ctrl+0 and you get competing hypotheses, exact WinDbg commands, source search patterns, a fix checklist, and a blast radius.

No AI. No API key. Fully offline. Instant.

https://github.com/keithpotz/Crash-Catch-Analyzer-Release

#cpp #gamedev #unrealengine #debugging

---

**Post 2 — Before/after contrast**

Before Engineer Mode:
- Open crash dump
- See exception code: 0xC0000005
- Open WinDbg docs
- Remember which commands to run
- Figure out which file to look at
- 45 minutes later: find the null pointer

After Engineer Mode:
- Open crash dump
- Press Ctrl+0
- Get: 3 hypotheses ranked by confidence, 5 WinDbg commands with real thread IDs from the dump, 4 source search patterns, a fix checklist
- 2 minutes later: find the null pointer

Beta is free. Windows x64.

---

**Post 3 — The Rust engine angle (for HN / tech audience)**

We prototyped Engineer Mode as a Claude API call.

We shipped it as a deterministic Rust engine instead.

Every WinDbg command uses real values from the dump: actual thread IDs, actual register values, actual addresses. A language model would approximate these. The Rust engine reads them directly.

The output is also reproducible. Same dump, same output, every time. No hallucinated commands, no fabricated addresses.

https://crashcatchlabs.com/blog-engineer-mode.html

---

**Post 4 — Unreal Engine specific**

Unreal Engine crash dumps are painful.

The game thread crashes. You have a .dmp file. You open WinDbg. You stare at the stack. AEnemy::Tick faulted somewhere. The this-pointer was probably null. Now what?

CrashCatch Engineer Mode gives you:
- Hypothesis: Null Pointer Dereference [91% confidence]
- Command: dt AEnemy @rcx
- Search pattern: TWeakObjectPtr.*AEnemy
- Fix: add IsValid() before Tick, use TWeakObjectPtr

Offline. Free during beta. Works on UE4 and UE5 dumps.

#unrealengine #gamedev #indiedev

---

**Post 5 — Thread (multi-part, post as a thread)**

We just shipped Engineer Mode in CrashCatch v0.3.0-beta. Here is what it actually does, and why we built it the way we did. Thread:

1/ Most crash tools answer "what crashed." Engineer Mode answers "how do I fix it." Those are different questions and they require different output.

2/ The output has 6 parts:
- Hypotheses (ranked by confidence, with evidence)
- WinDbg plan (real commands, real values from the dump)
- Search patterns (grep patterns for your code, system modules filtered)
- Reproduction conditions (timing flags, memory pressure flags)
- Fix checklist (UE-aware for Unreal projects)
- Blast radius (low / medium / high / critical with scope + note)

3/ The WinDbg commands look like this:

~~[0x1a4c]s; k
dt AEnemy @rcx
!heap -p -a 0x0

Thread ID 0x1a4c came from the dump. Register RCX was identified because it held a near-null value at the fault. Type AEnemy was extracted from the crashing frame AEnemy::Tick. These are not templates. They are generated from this specific dump.

4/ We built it as a deterministic Rust engine. No API key, no network, instant. Same dump in = same output every time. Commands are always correct because they read real values from the dump, not from inference.

5/ It covers 10 crash classes: null pointer, use-after-free, heap corruption, stack overflow, deadlock, abort/assert, pure virtual call, security cookie overrun, out of memory, generic access violation. UE ensure() and check() failures are detected on top of any class.

6/ v0.3.0-beta is free to download and use through June 1. Windows x64. No license key needed.

https://github.com/keithpotz/Crash-Catch-Analyzer-Release

---

## Dev.to Blog Post

**Title:** From Crash Dump to Debug Plan: How We Built Engineer Mode Without AI

**Tags:** cpp, debugging, windows, gamedev

---

When your C++ application crashes on a user's machine, you get a `.dmp` file. Getting useful information out of it is already hard. But there's a second problem that most crash tools ignore entirely: even after you know *what* crashed, figuring out *what to do about it* still requires significant experience and time.

That's the problem Engineer Mode solves.

### What most crash tools give you

A good crash analyzer gives you:
- A symbolicated stack trace
- The exception code and target address
- Register state at the time of the fault
- A crash classification (null pointer, heap corruption, etc.)

That's the starting point for debugging. After you have that information, a senior engineer will:

1. Form several competing theories about root cause
2. Decide which theory is most consistent with the evidence in the dump
3. Write the WinDbg commands needed to test each theory
4. Search their codebase for the relevant type names and patterns
5. Assess whether it's a quick fix or a deeper structural issue

Junior engineers either skip these steps or spend significant time figuring them out. Engineer Mode does all of them automatically.

### What Engineer Mode generates

**Hypotheses ranked by confidence**

For a `0xC0000005` access violation with `RAX = 0x0` and a fault in `AEnemy::Tick`, Engineer Mode generates:

```
[HIGH 91%] Null Pointer Dereference
  ✓ RAX = 0x0 (null pointer in accumulator)
  ✓ Target address: 0x0
  ✓ Fault in user module AEnemy::Tick
  ✗ No deallocation found in crashing stack
  Confirm: dt AEnemy @rcx

[LOW 9%] Use-After-Free
  ✓ Read access violation
  ✗ Target address 0x0 — not heap-range
  ✗ No deallocation frame in crashing stack
```

Each hypothesis includes supporting evidence, contradicting evidence, and a specific command to confirm or rule it out.

**Exact WinDbg commands with real dump values**

```
1  !analyze -v
   Full automated analysis

2  ~~[0x1a4c]s; k
   Switch to crashing thread, print stack

3  dt AEnemy @rcx
   Inspect AEnemy object at RCX register

4  !heap -p -a 0x0
   Check heap metadata at fault address
```

Thread ID `0x1a4c` came from the dump. `AEnemy` was extracted from the crashing frame. `@rcx` was selected because RCX was the near-null register at fault. These are not templates filled in with placeholder text — they are generated from this specific dump.

**Source search patterns**

```
AEnemy
AEnemy\s*\*\s*\w+\s*=
IsValid\(.*AEnemy
GetEnemy|FindEnemy|SpawnEnemy
```

Grep-compatible patterns generated from the type names and function names in the crashing stack. System modules are filtered out automatically.

**Reproduction conditions, fix checklist, blast radius**

The fix checklist is context-aware. In an Unreal Engine project, a null pointer fix gets suggestions for `TWeakObjectPtr` and `IsValid()`. In a plain C++ project, it suggests `std::weak_ptr` and explicit null guards. The blast radius rates severity as low / medium / high / critical based on which thread crashed, which module was involved, and the exception class.

### Why we didn't use AI

The first prototype was an API call to Claude Sonnet. We shipped it as a deterministic Rust engine instead.

Here's why.

Every piece of output Engineer Mode generates can be derived directly from the crash data. The WinDbg commands need real thread IDs, real register values, real addresses — and those are all in the dump. You don't need a language model to read a struct field. You need a parser.

More importantly: the output needs to be trustworthy at the command level. An AI will sometimes generate a plausible-looking WinDbg command with a fabricated value. A Rust engine that reads the actual thread ID from the dump and formats it into the command string is always correct. When you paste `~~[0x1a4c]s` into WinDbg, you know that thread ID is real because the code that generated it read it directly from the dump binary.

There's also the practical side: offline, instant, no cost per use, no API key, reproducible output. Same dump in, same output every time. That matters for a debugging tool.

The Intel Engine (our heuristic crash classifier, also in Rust) works the same way. CrashCatch uses AI exactly where it adds value — Explain Mode, which translates crash data into plain-English explanations for engineers who aren't familiar with Windows internals — and avoids AI where deterministic code is more reliable and faster.

### Crash classes covered

Engineer Mode produces tailored output for 10 crash classes:

| Class | Detection |
|---|---|
| Null Pointer Dereference | `0xC0000005` + null target address |
| Use-After-Free | `0xC0000005` + deallocation in crashing stack |
| Heap Corruption | `0xC0000374` |
| Stack Overflow | `0xC00000FD` + repeated frame heuristic |
| Deadlock | All threads in wait functions |
| Abort / Assert | `0x40000015` |
| Pure Virtual Call | `__purecall` in crashing stack |
| Security Cookie Overrun | `0xC0000409` |
| Out of Memory | `0xC0000017` |
| Generic Access Violation | Fallback for non-null `0xC0000005` |

Unreal Engine `ensure()` and `check()` failures are detected from the stack and layered on top of any base class.

### How to use it

Open a crash dump in CrashCatch and press `Ctrl+0`, or click the Engineer tab. The report generates immediately. Engineer Mode output is also included in PDF exports and the Copy as Markdown output.

CrashCatch v0.3.0-beta is free to download and use through June 1, 2026. Windows x64, no license key required.

GitHub: https://github.com/keithpotz/Crash-Catch-Analyzer-Release

---

## YouTube Script

**Title:** Engineer Mode: From Crash Dump to Debug Plan in Seconds

**Target length:** 4-5 minutes

**Format:** screen recording with voiceover. No face cam needed.

---

### [HOOK — 0:00 to 0:20]

*[Screen: a raw crash dump open in WinDbg, hex output visible]*

You get a crash dump from a user. You open it. You see an access violation, a faulting stack frame, some register values. You know what crashed. But what do you actually do next?

If you've been writing C++ long enough, you have a process in your head. Form some theories, write a few WinDbg commands, search the codebase for the relevant types. It works, but it takes time and experience. That's what Engineer Mode automates.

---

### [SETUP — 0:20 to 0:50]

*[Screen: CrashCatch Analyze open with a crash dump loaded, Intel Engine tab visible]*

This is CrashCatch. I've already loaded a crash dump. The Intel Engine has classified it as a null pointer dereference with high confidence. The stack shows the crash happened in `AEnemy::Tick` in an Unreal Engine project. `RAX` is zero.

So I know what happened. Now I want to know how to fix it.

*[Click Engineer tab or press Ctrl+0]*

I press Ctrl+0 to open Engineer Mode.

---

### [DEMO — 0:50 to 3:00]

*[Screen: Engineer Mode panel loads instantly]*

Instant. No network request, no API key. The report is already here.

**Hypotheses section:**

The top hypothesis is Null Pointer Dereference at 91% confidence. Below it you can see exactly why: RAX was null at the time of the fault, the target address was zero, and the crash was in user code. There's also a use-after-free hypothesis at 9% confidence — lower because there's no deallocation function in the crashing stack and the target address doesn't look like a heap pointer. Each hypothesis has a "how to confirm" field with the specific command to run.

**WinDbg plan:**

Five steps. Step 2 is `~~[0x1a4c]s; k` — switch to thread 0x1a4c and print the stack. That thread ID came from the dump. Step 3 is `dt AEnemy @rcx` — inspect the AEnemy object at the address in RCX. The type name came from the crashing frame. Step 4 is `!heap -p -a 0x0`. These aren't generic commands. They're built from this specific dump.

**Search patterns:**

Four patterns. The first is just `AEnemy` — find every reference to this type. The second is a regex for AEnemy pointer declarations, so you can check which ones have null guards and which ones don't. The third looks for existing `IsValid` checks on the type, which is the correct Unreal Engine pattern for null safety. These go straight into your terminal.

**Fix checklist:**

Because this is an Unreal Engine project, the checklist is UE-aware. It suggests wrapping the AEnemy pointer in `TWeakObjectPtr`, checking with `IsValid()` before Tick runs, and looking for any spawn or get functions that might return null without being checked. Not generic null pointer advice — specific to how UE manages object lifetimes.

**Blast radius:**

Critical. Game thread crash. This terminates the process for every user on this build. The note says to ship a fix before the next release.

---

### [HOW IT WORKS — 3:00 to 3:45]

*[Screen: back to Engineer Mode, scroll through the output slowly]*

The first version of this was an AI call. We rebuilt it as a deterministic Rust engine.

Here's why: every value in this output is in the dump. The thread ID, the register value, the type name from the stack frame — all of it is structured data that a parser can read directly. You don't need a language model for that, and using one introduces the risk of fabricated values in your WinDbg commands.

With a Rust engine, if the command says thread `0x1a4c`, that thread exists in the dump. The output is also reproducible. Same dump, same output, every time. No randomness.

It's also fully offline. Instant. No cost per analysis. Engineer Mode always works, even on an air-gapped machine.

---

### [OUTRO — 3:45 to 4:15]

*[Screen: GitHub releases page]*

Engineer Mode ships in CrashCatch v0.3.0-beta. The beta is free to download and use through June 1, 2026. Windows x64. No license key required.

The link is in the description. If you're working with C++ crash dumps and you're not already using CrashCatch, give it a try. Drop a dump in, press Ctrl+0, and see what comes back.

If you have feedback on the Engineer Mode output — hypotheses that are wrong, commands that should be different, crash classes that aren't covered — open an issue on GitHub. That's exactly the kind of feedback that improves the engine.

Thanks for watching.

---

*[End screen: CrashCatch logo + GitHub link + subscribe prompt]*

---

**Description (YouTube):**

Engineer Mode in CrashCatch v0.3.0-beta generates a complete debug session plan from any Windows crash dump. Competing hypotheses ranked by confidence, exact WinDbg commands built from real dump values, grep patterns for your source code, a context-aware fix checklist, and a blast radius assessment. Offline, instant, no API key.

Download: https://github.com/keithpotz/Crash-Catch-Analyzer-Release
Blog post: https://crashcatchlabs.com/blog-engineer-mode.html

0:00 The problem
0:50 Loading a crash dump
1:10 Engineer Mode demo
3:00 Why it's a Rust engine, not AI
3:45 Download
