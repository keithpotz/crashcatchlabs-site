# CrashCatch Analyze

**Windows crash dump analysis for indie developers and studios.**

CrashCatch Analyze is a native Windows desktop app that turns `.dmp` files into actionable crash reports with symbolication, AI-powered explanations, Unreal Engine awareness, and one-click PDF export. No cloud required. Free to use.

> **v1.0.0** — Windows only. [Download the latest release →](../../releases/latest)

---

![Main application view](mainpage.png)

![Crash dump analysis](dumpexample.png)

![CrashCatch Intel engine](intel.png)

![Unreal Engine crash analysis](Unreal%20Engine%20Crash.png)

![Compare Dumps](compare.png)

---

## Features

### Core Analysis
- Drag-and-drop `.dmp` file loading
- Full stack trace with symbolicated frames
- Exception code decoding (ACCESS_VIOLATION, STACK_OVERFLOW, etc.)
- Module list with load addresses
- Thread enumeration
- Raw JSON output for scripting / CI pipelines

### Explain Mode
- One-click AI explanation of the crash in plain English
- Powered by Claude — explains the root cause, not just the exception code

### Engineer Mode
- Deep technical breakdown for senior engineers
- Ranked hypotheses with supporting evidence from the crash report
- Exact WinDbg commands using real thread IDs and register values
- Codebase grep patterns derived from actual stack frames
- Reproduction conditions, fix checklist, and blast radius analysis
- Fully deterministic — no API key required, no network, instant results

### CrashCatch Intel
- Pattern detection engine built on top of the analysis
- Flags common crash classes: null pointer dereferences, stack corruption, heap misuse, thread races
- Confidence scores and remediation notes
- Thread deadlock detection — identifies threads blocked in known wait functions
- Register near-null highlighting — flags suspicious low-value registers alongside zero detection

### Unreal Engine Support
- Detects UE projects automatically
- UE context tab: engine version, callsite classification, Blueprint vs. native frame tagging
- Understands common UE crash patterns (GC, async loading, physics)
- Open a full UE crash folder — auto-finds `.dmp` + `CrashContext.runtime-xml` and merges both into one report

### Workflow
- **Batch analysis** — analyse a whole folder of `.dmp` files at once; summary table across all dumps
- **Crash comparison** — diff two dumps side by side with per-frame match indicators
- **Crash signature deduplication** — tracks unique crash signatures across your session, badges duplicates
- **Folder watcher** — monitor a crash output folder; new dumps are auto-analysed as they appear
- **Session history** — recent files remembered across launches
- **Frame annotations** — add persistent notes to any stack frame
- **Per-dump notes** — attach notes to individual dump files; included in exports
- **Missing PDB report** — one-click list of every module with unresolved symbols

### Export & Sharing
- **PDF export** — complete report with all fields: stack trace (with frame annotations), registers, exception details, Intel findings, AI explanation, Engineer Mode output, UE context, per-dump notes, and app icon in the header
- **Copy as Markdown** — paste crash reports straight into GitHub, Notion, or Jira
- **GitHub issue button** — create a pre-filled issue directly from a crash report
- **Settings export / import** — back up and restore symbol paths and preferences as JSON

### UI & Polish
- **Ctrl+F search** — full-text search across the report with match highlighting
- **Drag-to-reorder symbol paths** — reorder search paths by dragging in settings
- **System tray** — minimises to tray on close; left-click to show/hide
- **Contextual help** — inline `?` tooltips throughout the UI
- **Keyboard shortcuts** — full keyboard navigation (see Help menu)
- **Auto-updater** — checks for updates at launch and installs automatically

### Security
- **Malicious dump protection** — magic byte validation and 500 MB size cap before analysis
- **UNC path blocking** — symbol paths cannot point to network shares, preventing NTLM credential leaks
- **Content Security Policy** — strict CSP enabled throughout the app
- **Settings export is key-safe** — API keys are never written to exported settings files

---

## Download

Go to [Releases](../../releases) and download the latest `.exe` installer.

**Requirements:**
- Windows 10 / 11 (x64)
- ~50 MB disk space
- Internet connection required for Explain Mode (AI calls) — Engineer Mode works fully offline

> **SmartScreen warning:** The installer is not yet code-signed. Windows will show a "Windows protected your PC" prompt. Click **More info → Run anyway** to proceed.

---

## Open Source SDK

The analysis engine that powers CrashCatch Analyze is open source:

**[github.com/keithpotz/CrashCatch](https://github.com/keithpotz/CrashCatch)** — Apache 2.0

Use it to integrate crash analysis directly into your own tools, CI pipelines, or game engines.

---

## Feedback

Found a bug or have a feature request? [Open an issue](../../issues) — all feedback welcome.

---

## Tech Stack

- **Frontend:** TypeScript, HTML/CSS (Tauri WebView2)
- **Backend:** Rust (Tauri 2)
- **Analysis engine:** C++ (DbgHelp, DIA SDK)
- **AI:** Anthropic Claude API

---

© 2026 CrashCatch Labs
