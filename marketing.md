# CrashCatch Marketing Plan
**Goal:** Drive beta downloads before the free period ends 2026-06-01
**Product:** CrashCatch Analyze v0.2.1-beta — Windows-first crash intelligence for C++ and Unreal Engine

---

## 1. Target Audience

### Primary
- **Indie and AA game studios** shipping on Steam or Epic using Unreal Engine
- **Solo Unreal Engine developers** — no devops team, no budget for $200/mo SaaS
- **Native C++ Windows app developers** — DCC tools, engines, plugins, systems software

### Secondary
- **Enterprise and defense teams** with data compliance requirements (offline analysis is the key hook)
- **Junior C++ developers** who get a crash dump and have no idea what to do with it
- **Senior engineers** who want to speed up their debugging workflow

### Key Insight
The audience lives on Reddit, Discord, Hacker News, and YouTube. They do not respond to traditional ads. They respond to proof — show the before (raw WinDbg output) and after (CrashCatch explained output).

---

## 2. Core Messaging

### Headline
**"Drop in a crash dump. Get an answer."**

### Supporting Points
- No cloud upload. Your crash data stays on your machine.
- One-click AI explanation of any C++ crash
- Built for Unreal Engine — not an afterthought
- Free during beta. One-time price at launch. Not $26/month.
- Zero-dependency header-only SDK. One line of code.

### Objection Handling
| Objection | Response |
|---|---|
| "I already use WinDbg" | CrashCatch uses DbgHelp under the hood — same symbols, better output in seconds |
| "I use Sentry" | Sentry is web-first. It does not understand UE crash folders, GC crashes, or Blueprint frames |
| "I don't ship C++" | Then CrashCatch is not for you — and that's fine |
| "It's unsigned" | Expected for beta. SmartScreen prompt takes 2 clicks to bypass |

---

## 3. Channels and Tactics

---

### Reddit

**Target subreddits:**
- r/unrealengine (800k members — highest priority)
- r/cpp (250k members)
- r/gamedev (1.2M members)
- r/indiegaming
- r/gameenginedevs
- r/WindowsDev

**Post types that work:**
1. **Before/after post** — screenshot of raw WinDbg output next to CrashCatch Explain Mode output for the same crash. No pitch, just show it. Caption: "I got tired of reading hex, so I built this."
2. **Problem/solution post** — "How do you all deal with crash dumps from shipped builds?" Post in r/unrealengine, answer with CrashCatch after a few replies establish the problem
3. **Release announcement** — "I built a crash analyzer for Unreal Engine devs — beta is free, here's what it does." Keep it honest and direct.
4. **Tutorial post** — "How to actually read a Windows minidump file" — educational, links to the blog post, mentions CrashCatch naturally at the end

**Rules:**
- Never post the same content to multiple subs in the same week
- Always lead with value, not the product
- Respond to every comment for the first 48 hours
- Post on Tuesday or Wednesday mornings US Eastern time

---

### Hacker News

**Show HN post:**
```
Show HN: CrashCatch — a Windows crash dump analyzer for C++ and Unreal Engine developers
```

**Body should include:**
- What it does in 2 sentences
- Why you built it (honest story — WinDbg is painful, SaaS tools ignore C++)
- What makes it different (offline, AI explanation, UE support)
- Link to the GitHub release
- Mention it is free during beta

**Timing:** Tuesday or Wednesday, 8am-10am US Eastern
**Expectation:** Even 50 upvotes can drive hundreds of downloads

**Ask HN alternative:**
Post "Ask HN: How do you handle crash dumps from shipped C++ applications?" — participate in the thread, mention CrashCatch only if directly relevant.

---

### YouTube

**Already have:** Introduction video (Dt-_nAqVHg4) embedded on the landing page.

**Suggested video series:**
1. **"Reading a crash dump in 60 seconds"** — short-form, show the drag-drop-analyze flow
2. **"WinDbg vs CrashCatch — same crash, different experience"** — side-by-side comparison video
3. **"How Explain Mode works"** — screen recording of an actual UE crash being analyzed and explained
4. **"Batch analyzing 50 crash dumps"** — shows the batch feature, appeals to QA teams and studios
5. **Tutorial: Setting up crash reporting in Unreal Engine with CrashCatch**

**Distribution:**
- Post to YouTube with keyword-rich titles and descriptions
- Share clips on Twitter/X and LinkedIn
- Embed relevant videos in the corresponding blog posts and docs pages

---

### Twitter / X

**Account strategy:**
- Post short before/after crash analysis clips (15-30 seconds screen recordings)
- Share each new blog post with a 2-3 sentence hook
- Reply to developers complaining about crash dumps, WinDbg pain, or Unreal crashes
- Tweet the raw vs explained output as a screenshot — high shareability

**Example tweets:**
- "This is what an Unreal Engine crash dump looks like in WinDbg. This is what it looks like in CrashCatch. [screenshot]"
- "Built batch crash analysis into CrashCatch — drop a whole folder of .dmp files and get a summary table in seconds. Beta is free."
- "Why does C++ crash reporting still cost $200/month in 2026? Built something better. [link]"

**Hashtags:** #gamedev #unrealengine #cpp #indiedev #gamedevelopment

---

### Discord Communities

**Target servers:**
- **Unreal Slackers** (largest UE community — 80k+ members) — post in tools/resources channel
- **Game Dev League**
- **C++ Discord**
- **r/gamedev Discord**
- **Epic Games Developer Community**
- **Indie Game Dev**

**Approach:**
- Lurk for a week before posting
- Help people with crash-related questions first, mention CrashCatch when it is genuinely relevant
- Share release announcements in announcements or tools channels only
- Offer to analyze a crash live in chat to demonstrate the tool

---

### Unreal Engine Forums and Community

- Post in the **Unreal Engine forums** tools and resources section
- Submit to the **Unreal Engine Marketplace** developer tools section if applicable
- Participate in the **UE Developer Community** on Discord
- Comment on crash-related threads in the official forums with helpful answers, mention CrashCatch

---

### Product Hunt

**Launch a Product Hunt listing:**
- Schedule for a Tuesday or Wednesday
- Prepare hunter outreach 1 week in advance
- Write a clear tagline: "Drop a crash dump. Get a plain-English root cause. In seconds."
- Ask existing users and contacts to upvote on launch day
- Respond to every comment
- Product Hunt traffic converts well for developer tools

**Prep checklist:**
- [ ] 240x240 logo
- [ ] Gallery screenshots (before/after, batch analysis, UE crash folder)
- [ ] 60-second demo video
- [ ] Tagline under 60 characters
- [ ] Hunter lined up

---

### Developer Newsletters

Submit to newsletters that reach C++ and game development audiences:

| Newsletter | Audience | Submission |
|---|---|---|
| **TLDR** | General dev — 1M+ subscribers | tldr.tech/submit |
| **Gamedev.js Weekly** | Game developers | Email the editor |
| **C++ Weekly** | C++ developers | Jason Turner — Twitter/email |
| **Game Developer Digest** | Game devs | Submit via website |
| **Console & PC Gaming Newsletter** | Broader game industry | |

---

### GitHub

- Make the **README of the CrashCatch repo** a strong landing page — screenshots, before/after, quick install
- Pin the release repo as a featured repo
- Post in GitHub **Discussions** if the repo has them
- Add CrashCatch to **awesome-cpp**, **awesome-gamedev**, and similar curated lists — these get a lot of passive traffic
- Submit to **awesome-unreal**

---

### SEO and Blog Content

Already have 4 blog posts. Planned posts that target high-intent search queries:

| Title | Target Keyword | Priority |
|---|---|---|
| "How to debug an Unreal Engine crash on a player's machine" | unreal engine crash debug | High |
| "What is a Windows minidump and how do you read one?" | windows minidump | High |
| "C++ crash reporting tools compared: Sentry, Backtrace, and CrashCatch" | c++ crash reporting | High |
| "How to set up crash reporting in Unreal Engine 5" | unreal engine 5 crash reporting | High |
| "Stack overflow vs access violation: what's the difference?" | stack overflow access violation | Medium |
| "How to generate a minidump from a Windows application" | generate minidump windows | Medium |
| "Understanding heap corruption crashes in C++" | heap corruption c++ | Medium |
| "Unreal Engine assertion failed: what it means and how to fix it" | unreal assertion failed | Medium |

---

### Indie Game Dev Platforms

- **GameDev.net** — post in the tools section and participate in the community
- **itch.io** — list CrashCatch as a developer tool (it has a dev tools section)
- **IndieDB** — create a project page
- **Steam** — not yet applicable but keep in mind for commercial launch

---

### Email List (Mailchimp)

- Send a launch email to existing subscribers announcing v0.2.1-beta
- Keep it short: what is new, why it matters, download link
- Follow up 2 weeks before the free period ends (2026-06-01) with a reminder

**Email sequence:**
1. Welcome email — sent immediately on signup: what CrashCatch is, link to download beta
2. Feature highlight — 1 week after signup: show one feature in depth (e.g., batch analysis)
3. Social proof — 2 weeks after: GitHub stars, community feedback, use cases
4. Pricing reminder — sent 2026-05-15: beta ends June 1, here is what it will cost

---

### LinkedIn

- Post about the technical problem CrashCatch solves — frame it as an engineering insight, not a product pitch
- Share blog posts with a technical hook
- Connect with leads at game studios, tools companies, and defense/enterprise software teams
- Post the comparison table (CrashCatch vs Sentry vs WinDbg vs Backtrace) as a carousel

---

### Cold Outreach

Identify developers who have:
- Tweeted about WinDbg frustration
- Posted on Reddit about crash dump problems
- Asked questions about Unreal Engine crashes on Stack Overflow or forums

Reach out with a personal, direct message. Offer a free look at the tool. Ask for honest feedback. Do not mass-message.

---

## 4. Content Calendar (Now to June 1)

| Week | Action |
|---|---|
| Week 1 (now) | Post on r/unrealengine, set up Product Hunt listing, publish new blog post |
| Week 2 | Hacker News Show HN post, Twitter clip of Explain Mode |
| Week 3 | Discord outreach (Unreal Slackers), newsletter submissions |
| Week 4 | Product Hunt launch day |
| Week 5 | New blog post, YouTube tutorial video |
| Week 6 | Reddit r/cpp post, GitHub awesome list submissions |
| Week 7 | Email list feature highlight, LinkedIn post |
| Week 8 | New blog post, Twitter before/after clip |
| Week 9 (May 15) | Email reminder — beta ends June 1 |
| Week 10 | Final push — all channels, "last week of free beta" messaging |

---

## 5. Metrics to Track

| Metric | Target | Where to Check |
|---|---|---|
| GitHub release downloads | 500 before June 1 | GitHub release page |
| Unique site visitors | 5,000/month | Cloudflare Analytics |
| Email subscribers | 200 | Mailchimp |
| GitHub repo stars | 100 | GitHub |
| Reddit post upvotes | 100+ on best post | Reddit |
| Blog organic traffic | Top 3 posts ranking | Google Search Console |

---

## 6. Budget

CrashCatch is a bootstrapped product. The entire plan above is zero-cost organic marketing. If budget becomes available, prioritize in this order:

1. **Reddit promoted posts** — target r/unrealengine and r/cpp. $50-200 can reach tens of thousands of the exact right people.
2. **Google Ads** — target "unreal engine crash", "c++ crash reporting", "windows minidump tool". High intent, manageable CPC.
3. **Sponsored newsletter slot** in TLDR or a game dev newsletter. $200-500 for a single slot.

---

## 7. Key Rules

- Always lead with the problem, not the product
- Show do not tell — screenshots and videos convert better than any copy
- Never use fake urgency — the June 1 deadline is real, use it honestly
- Respond to every comment and message in the first 48 hours of any post
- No em-dashes in any marketing copy
- Do not cross-post the same content to multiple communities in the same week
