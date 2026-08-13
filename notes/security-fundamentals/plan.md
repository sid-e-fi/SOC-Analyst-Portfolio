# SOC Analyst L1 Portfolio — Master Plan

**Repository:** `sid-e-fi/SOC-Analyst-Portfolio` *(renamed from `sid-e-fi/soc-analyst-roadmap` — see Phase 1, Task 1)*
**GitHub username (unchanged):** sid-e-fi
**Purpose:** Convert an Obsidian vault documenting an ongoing SOC Analyst L1 learning journey into a clean, recruiter-facing GitHub portfolio.

**This file is the single source of truth for the project.** It merges two prior planning documents (`plan.md` — the original post-audit plan — and `plan update.md` — a later refinement pass) into one authoritative plan. Any Claude session — with zero prior conversational context — should be able to read this file, understand exactly what has been done and what hasn't, and continue the work without re-doing or losing anything.

**If you are a fresh Claude session reading this:** jump to [✅ Merge Notes — Conflicts Resolved by Owner](#merge-notes--conflicts-resolved-by-owner), then [Project Status Snapshot](#project-status-snapshot), then find the first phase that is not `COMPLETED`. Read its Prerequisites and "Files/Decisions Required" before doing anything.

---

## ✅ Merge Notes — Conflicts Resolved by Owner

This master plan was originally assembled from two source documents that, on inspection, **disagreed on a few points**. Per the project's own standing rule ("don't guess missing information — ask"), these were flagged rather than silently resolved. **The owner has now answered all of them (confirmed 2026-08-13).** The table below is kept as-is, with resolutions appended, rather than deleted — per Standing Rule 5 ("flag before deleting, don't erase history").

| # | Conflict | Original `plan.md` said | `plan update.md` said | **Resolution (confirmed by owner)** |
|---|---|---|---|---|
| 1 | **Owner display name / background** | "Siddharth Sharma" — verified against a real Cisco certificate screenshot in the audit (issued 09 Aug 2026) | "Krish" — student, BTech CSE + Electronics, GNDU Amritsar | **RESOLVED: "Siddharth Sharma."** Used for the README byline and everywhere this plan refers to "the owner." |
| 2 | **Glossary location** | `glossary.md` at repo **root** (cross-cutting reference, not a topic) | `notes/glossary.md` (**inside** `notes/`) | **RESOLVED: repo root — `glossary.md`.** Matches the original plan's reasoning and the default already used in the merged structure below. |
| 3 | **Lab / infrastructure documentation** (`SOC-L1 Workspace.md`, `VMware Setup.md`, `Windows 11 Lab.md`) | Real, valuable content — gets its own top-level `labs/` folder | Not mentioned at all in the update's target structure | **RESOLVED: keep `labs/` in scope.** Confirmed not a deliberate cut — stays as its own top-level folder. |
| 4 | **Screenshot / asset scope** | All 13 reviewed screenshots get organized into `assets/week-1/day-N/` and embedded across the daily logs + summary page | Only **2** of the 13 screenshots (VM lab specs, fresh-install snapshot) go to GitHub at all, embedded in a new README "Environment" section; the other 11 are judged redundant/low-signal and stay vault-only | **DEFERRED — genuinely not decided yet, not a default.** The owner has explicitly not made this call and wants to be asked again when Phase 5 actually starts. Do not assume 2, do not assume 13 — ask. |
| 5 | **Raw daily logs on GitHub** | Phase 7 migrates all 6 daily logs to `progress/daily-logs/week-1/` | Standing Rule: "No raw daily logs go to GitHub. Only weekly rollups." | **RESOLVED: vault-only.** Only the weekly rollup (`progress/week-1-summary.md`) ships. Confirms Standing Rule 3; Phase 7 stays skipped/superseded. |

### Additional decisions confirmed in the same pass

These were open items in Phase 1's "Files/decisions required" list and Phase 0's flagged gaps — not part of the original two-document conflict, but resolved by the owner alongside #1–#5 above.

| Topic | Resolution |
|---|---|
| **LICENSE** | MIT License. Added in Phase 1. |
| **YAML frontmatter on GitHub** (Phase 0 Gap #7) | Convert to whatever GitHub's Markdown renderer actually supports. Keep any field GitHub renders cleanly (simple `key: value` and short list fields — `type`, `status`, `tags` all qualify) — don't strip something GitHub can handle. Strip only fields that are Obsidian-plugin-specific or that would render as broken/out-of-place raw text on GitHub. The Obsidian source files themselves are untouched; this only affects the GitHub-published copies. |
| **Push/delivery mechanism** | Owner will run the `git` commands Claude provides (Claude has no push access in this environment). Confirms the approach already assumed in Phase 12. |
| **Broken wikilink, Day 4 log** (`[[Identity & Access Management]]` → real file is `Identity and Access Management (IAM).md`) | **Leave it flagged/unresolved — do not fix it.** Since raw daily logs stay vault-only (Standing Rule 3), this only becomes relevant again if the same broken reference is reused inside `week-1-summary.md`; if so, flag it there too rather than silently correcting it. |
| **Daily log dates** (Phase 0 Gaps #2–#3: placeholder date on Day 5, inconsistent gap between Day 1/3/4) | Day 1 = **25 July 2026**, then each subsequent day continues sequentially: Day 2 = 26 July, Day 3 = 27 July, Day 4 = 28 July, Day 5 = 29 July, Day 6 = 30 July 2026. The vault-only raw logs' own internal date fields are not being individually corrected — but **wherever a date is required in anything that ships to GitHub** (chiefly the "dates covered" line in `progress/week-1-summary.md`), use this 25–30 July 2026 range. |

**All five original conflicts, and every other decision blocking Phase 1, are now resolved except the Phase-5 screenshot count/selection (deliberately left open — see #4 above). Phase 1 is ready to execute.**

---

## Project Status Snapshot

| Phase | Name | Status |
|---|---|---|
| 0 | Audit source material & merge into master plan.md | **COMPLETED** |
| 1 | Repository rename + scaffolding | **COMPLETED** |
| 2 | Migrate & clean Security Fundamentals notes | **COMPLETED** (2026-08-14) |
| 3 | Build the glossary | NOT STARTED — Phase 2 ✅ unblocked; needs the glossary source file from you |
| 4 | Migrate lab/infrastructure documentation | NOT STARTED — blocked on Phase 1 ✅ unblocked; needs the 3 lab doc source files from you |
| 5 | Finalize & organize screenshot/asset decision | NOT STARTED — blocked on Phase 1 ✅ unblocked; screenshot count/selection still an open call (see Conflict #4)
| 6 | Build Week 1 progress summary page | NOT STARTED — blocked on Phases 2–5 |
| 7 | *(superseded)* Migrate raw daily logs | **SKIPPED / SUPERSEDED** — see Standing Rule 3 |
| 8 | Add "Environment" section to README (approved screenshots) | NOT STARTED — blocked on Phase 5 |
| 9 | Validate the full Week 1 repository | NOT STARTED — blocked on Phases 1–8 |
| 10 | Write the recruiter-facing root README | NOT STARTED — blocked on Phase 9 |
| 11 | LICENSE, .gitignore review, final polish | NOT STARTED — blocked on Phase 10 |
| 12 | Commit & push to GitHub (incl. applying the rename) | NOT STARTED — blocked on Phases 1–11 |
| 13 | Scaffold future roadmap sections (Networking, Linux, Windows, SIEM, etc.) | NOT STARTED — blocked on Phase 12 |

**Next action: Phase 2 is complete (all 15 Security Fundamentals notes migrated and validated, 2026-08-14). Phase 3 needs the glossary source file (`13 Glossary/Cybersecurity Glossary.md`) uploaded before it can run.**

---

## Standing Rules (merged — never violate these regardless of phase)

1. **Never fabricate.** No invented certifications, labs, skills, projects, experience, findings, technical knowledge, screenshots, results, commands, or incidents. Everything must trace back to a real file in the source material or something you explicitly stated.
2. **Never silently "fix" ambiguous content.** Questionable dates, broken links, or shaky technical claims get flagged and asked about — never guessed or quietly corrected.
3. **No raw daily logs go to GitHub.** Only weekly rollups (`progress/week-N-summary.md`) are published. Daily logs stay in the vault. *(This supersedes the original plan's Phase 7 — see Conflict #5.)*
4. **Screenshots default to vault-only.** A screenshot only earns a place in the repo if it proves something a markdown file or certificate can't already prove. A screenshot of a note already published as markdown is redundant — skip it. *(See Conflict #4 for how this narrowed the original scope.)*
5. **Preserve knowledge first, organize second, polish third.** Never delete learning content merely because it's repetitive, informal, or less polished — flag it instead of cutting it.
6. **Don't blindly convert Obsidian wikilinks.** Verify the destination file exists and compute the correct relative path first. If the target doesn't exist yet, document it as a pending dependency instead of linking to nothing.
7. **Don't guess missing inputs.** If a phase needs a file, screenshot, or decision that hasn't been provided, stop and ask for it by name — see the Open Conflicts section above for the standing example of this rule in action.
8. **Keep commits scoped.** No mixing unrelated changes into one commit. No secrets, tokens, `.env` files, private keys, or personal credentials ever committed.
9. **Never commit** `ISOs/`, `VMs/`, or any other excluded-by-`.gitignore` content.
10. **Don't modify `.gitignore`** without checking its current state first.
11. **Update this `plan.md` after every completed phase** — status, what was actually done, files touched, decisions made, what's still pending, next phase. This file must always let a zero-context Claude session resume correctly.
12. **At the start of every phase**, read this file, confirm the previous phase's status, check that phase's prerequisites/required files, and ask for anything genuinely missing before proceeding — never guess.

---

## Locked Decisions Log (merged)

| # | Decision | Reasoning | Source |
|---|---|---|---|
| 1 | Display name across the repo is **Siddharth Sharma**. GitHub username stays `sid-e-fi` either way. | Verified against a real Cisco certificate screenshot in the Phase 0 audit. | Resolved per Merge Notes, confirmed 2026-08-13 |
| 2 | Of the 13 Week 1 screenshots reviewed, only **2** go to GitHub: VM lab specs and the fresh-install snapshot. Both are embedded in the main **README**, under a new **"Environment"** section — not in a per-week `assets/` subfolder. Everything else (setup command logs, screenshots of notes already published as markdown, mid-progress course screenshots, exam/cert screenshots) is redundant or low-signal and stays vault-only. | These are the only two that prove something the notes/certs don't already prove (a deliberately configured, snapshotted lab). | `plan update.md` (adopted; narrows original's 13-screenshot plan — Conflict #4) |
| 3 | Cisco cert screenshots (exam pass, certificate, badge) are **not** duplicated into the repo. They're already on LinkedIn/Credly, the canonical sources. Kept organized in the vault's evidence folder for later use in applications/interviews. The Cert ID/QR-visibility question from the original audit (Gap #6) is therefore **moot** — it never ships to the public repo. | Avoids duplicate, stale copies of credentials living in two places. | `plan update.md` (adopted; resolves original Gap #6) |
| 4 | Repository structure is **topic-based, not week-based.** `notes/` is organized by domain (starting with `security-fundamentals/`), not by "week-1", "week-2". `progress/` is the one place that *does* stay chronological, since it's a dated log of progress, not reference material. | Recruiters browse by skill domain (networking, SIEM, detection engineering), not by calendar week. A `week-1/` folder doesn't map to any future domain folder. | Both docs agree independently — confirmed |
| 5 | All 15 Security Fundamentals notes are preserved in full — none are redundant enough to cut, each covers a distinct SOC-relevant topic. *(Corrected from "14" during Phase 2 — the actual folder holds 15 files, matching the 15 filenames already listed in the target structure tree below; the "14" in earlier prose was a stale count from the merge pass. Confirmed by owner 2026-08-14.)* | "Preserve knowledge first, organize second, polish third." | `plan.md`, corrected Phase 2 |
| 6 | ISOs and VM files are permanently excluded from git (too large, not portfolio-relevant). `.gitignore` must guard against them being committed accidentally. | Confirmed by owner during original audit. | `plan.md` |
| 7 | Filenames move from Title Case with spaces/symbols (Obsidian-style) to **kebab-case** (e.g. `CIA Triad and Basic Security Concepts.md` → `cia-triad-and-basic-security-concepts.md`). | Standard practice for GitHub-hosted Markdown portfolios. | `plan.md` |
| 8 | All current wikilinks in the Week 1 note set resolve to real files within that set, except one broken link (`Week 1 - Day 4.md` → `[[Identity & Access Management]]`, real file is `Identity and Access Management (IAM).md`) — flagged, not silently fixed. | "Preserve/flag, don't fabricate" rule. | `plan.md` |

---

## Current Target Repository Structure (merged)

```text
SOC-Analyst-Portfolio/
├── README.md                          # recruiter-facing entry point + "Environment" section
├── plan.md                            # this file — project source of truth
├── LICENSE                            # decision pending (Phase 1)
├── .gitignore                         # blocks ISOs/, VMs/, OS junk, Obsidian workspace state
├── notes/
│   └── security-fundamentals/         # topic subfolder — future topics get siblings here
│       ├── what-cybersecurity-actually-is.md
│       ├── cybersecurity-fundamentals.md
│       ├── cia-triad-and-basic-security-concepts.md
│       ├── defense-in-depth.md
│       ├── risk-management.md
│       ├── threat-actors-and-cyber-warfare.md
│       ├── malware-and-social-engineering.md
│       ├── network-security.md
│       ├── vulnerabilities-and-patch-management.md
│       ├── identity-and-access-management.md
│       ├── authentication-and-authorization.md
│       ├── identity-attacks.md
│       ├── cryptocurrency-and-cryptojacking.md
│       ├── incident-response.md
│       └── cisco-introduction-to-cybersecurity-course-notes.md
├── glossary.md                        # root location confirmed (Conflict #2, resolved)
├── labs/                              # in scope, confirmed (Conflict #3, resolved)
│   └── week-1-lab-setup/
│       ├── soc-l1-workspace.md
│       ├── vmware-setup.md
│       └── windows-11-lab.md
├── progress/
│   └── week-1-summary.md              # rollup only — no raw daily logs (Standing Rule 3)
└── assets/
    ├── environment-vm-lab-specs.png            # P9 — only screenshot #1
    └── environment-fresh-install-snapshot.png  # P10 — only screenshot #2
```

**Why this structure (merged reasoning):**
- `notes/security-fundamentals/` is a *subfolder*, not flat files, because 9+ more topic folders (Networking, Linux, Windows, SIEM, TryHackMe, PortSwigger, Incident Reports, MITRE ATT&CK, Cheat Sheets) are already planned — a flat `notes/` would become unnavigable once those fill in. *(Both docs agree.)*
- `glossary.md` sits at root since it's cross-cutting reference material used by every topic, not a topic itself — confirmed, Conflict #2.
- `labs/` is a separate top-level folder from `notes/`/`progress/` because lab/infrastructure documentation is evidence of hands-on environment-building, which recruiters specifically look for — confirmed, Conflict #3.
- `progress/` contains only the weekly rollup, not a `daily-logs/` subtree, per Standing Rule 3 (a change from the original plan's nested `progress/daily-logs/week-1/day-N.md` structure).
- `assets/` is now **flat**, holding only the 2 approved Environment screenshots, per Locked Decision #2 — not the original's per-day `assets/week-1/day-N/` tree, since only 2 of the 13 images are shipping at all.

---

## Phase 0: Audit Source Material & Merge Into Master Plan

**Status: COMPLETED**

### What was audited
1. **The live GitHub repository** (`sid-e-fi/soc-analyst-roadmap`, pre-rename) — confirmed to contain **exactly one file**: `README.md`, 22 bytes, containing only `# soc-analyst-roadmap`. No `plan.md`, `.gitignore`, `LICENSE`, or folders exist yet in the actual repo.
2. **`SOC-L1.zip`** — the full local `SOC-L1 Backup` folder (original `plan.md`'s audit).
3. **Two prior planning documents** (`plan.md` and `plan update.md`) — reconciled into this master plan, with disagreements surfaced rather than silently merged (see Merge Notes above).

### Findings: local source material (`SOC-L1 Backup/`)

| Folder | Contents | Status |
|---|---|---|
| `Notes/Obsidian Vault/01 Security Fundamentals/` | 15 concept/course notes, all `status: reviewed` | Real content — migrated (Phase 2) |
| `Notes/Obsidian Vault/13 Glossary/` | 1 glossary note, 742 lines, A–Z format | Real content — ready to migrate |
| `Notes/Obsidian Vault/11 Daily Logs/Week 1/` | 6 daily logs (Day 1–6) + `week-1-summary.md` | Real content; raw logs stay vault-only per Standing Rule 3, summary migrates |
| `Notes/Obsidian Vault/12 Lab Documentation/` | 3 notes: `SOC-L1 Workspace.md`, `VMware Setup.md`, `Windows 11 Lab.md` | Real content — ready to migrate (Conflict #3) |
| `Screenshots/Week-1/` | 13 `.png` files across Day-1, Day-2, Day-4, Day-5, Day-6 (no Day-3) | Only 2 selected for GitHub (Locked Decision #2); rest stay vault-only |
| `Notes/Obsidian Vault/00 Dashboard/` | empty | Future scaffolding only |
| `02 Networking/` through `10 Cheat Sheets/` | empty | Future roadmap topics — not started |
| `Projects/*` (Networking, Windows-Linux-Logs, Siem-Lab, Incident-Reports) | empty | Future scaffolding only |
| `Reports/`, `Resources/`, `Backups/`, `Downloads/` | empty | Local housekeeping, not portfolio-relevant |
| `ISOs/`, `VMs/` | Not uploaded; too large | **Must never be committed** (Locked Decision #6) |

**Total real, usable content:** 15 concept notes + 1 glossary + 6 daily logs + 1 weekly summary + 3 lab docs ≈ 12,450 lines of Markdown, and 13 screenshots (~27 MB), all from Week 1. Everything else is empty future scaffolding.

### Content quality assessment
- All 15 Security Fundamentals notes carry consistent YAML frontmatter (`type`, `status: reviewed`, `tags`), a purpose blockquote, and a "Related Notes" wikilink section — a genuinely consistent note system, not a random dump. Topics: CIA Triad, Risk Management, IAM, Authentication & Authorization, Identity Attacks, Malware & Social Engineering, Network Security, Vulnerabilities & Patch Management, Threat Actors & Cyber Warfare, Defense in Depth, Incident Response, Cryptocurrency & Cryptojacking, a general Cybersecurity Fundamentals note, a mental-models note, and Cisco course notes.
- The glossary is a genuine A–Z reference with Obsidian callouts (`> [!tip]`) and internal cross-links.
- Daily logs (Day 1–6) are raw, dated process journals (workspace/VM setup, CIA Triad study, malware/social engineering, IAM, Cisco course parts 1–2 + exam pass). `week-1-summary.md` is already a polished rollup of all six days — this is what actually ships (Standing Rule 3).
- Lab documentation covers a real VMware Workstation Pro + Windows 11 VM lab on an Ubuntu host — genuine hands-on infrastructure evidence.
- Screenshots are real and verified — terminal setup, CIA Triad study material, Microsoft Learn progress, a genuine Cisco "Introduction to Cybersecurity" certificate, and exam-pass evidence. No fabrication needed anywhere.
- No emails, IP addresses, or credentials found in any note.

### Gaps / inconsistencies flagged (not fixed) — carried forward from the original audit
1. **Broken wikilink target.** `Week 1 - Day 4.md` links to `[[Identity & Access Management]]`, but the real note is `Identity and Access Management (IAM).md`. Looks like a typo — needs explicit yes/no confirmation before conversion. *(Note: since raw daily logs no longer ship to GitHub per Standing Rule 3, this only matters if the wording is reused inside `week-1-summary.md` — check there too.)*
2. **Placeholder date.** `Week 1 - Day 5.md` still has `**Date:** YYYY-MM-DD` — never filled in.
3. **Date inconsistency across the week.** Day 1 = 25 July 2026, Day 3 = 27 July 2026, Day 4 = 30 July 2026 (a 3-day jump), Day 2 and Day 6 have no date field, and `week-1-summary.md` claims the week spans "25 to 31 July 2026." Real dates (or "date not recorded") must come from you — never invented or guessed.
4. **No screenshot is currently referenced/embedded in any note.** All 13 images sit disconnected from the Markdown content. Now only 2 need placement (in the README's Environment section — Phase 8).
5. **No screenshots exist for Day 3** — a fact, not a gap.
6. ~~Cisco certificate screenshot Cert ID/QR visibility~~ — **moot**, per Locked Decision #3 (cert screenshots never ship to the repo at all).
7. **YAML frontmatter doesn't render specially on GitHub** — it will show as a plain text block. Recommendation (unconfirmed): keep frontmatter in the Obsidian source untouched; strip or convert it to a simple italic byline (e.g. *Status: Reviewed*) in the GitHub-published copies.

### Decisions already made
See the merged Locked Decisions Log above.

### Files created in Phase 0
- This master `plan.md`, superseding both prior planning documents.

### Next Phase
Phase 1: Repository rename + scaffolding — **blocked on Conflicts #1–#3.**

---

## Phase 1: Repository Rename + Scaffolding

**Status: COMPLETED**

### Objective
Apply the rename from `soc-analyst-roadmap` to `SOC-Analyst-Portfolio`, then create the empty directory structure, `.gitignore`, and `LICENSE` — no content migration yet.

### Why it exists
Establishing structure (and the correct name) before migrating content means every later phase is "add files to an existing skeleton" instead of "restructure mid-migration."

### Prerequisites
Phase 0 complete. Conflicts #1 (display name), #2 (glossary location), #3 (labs/ folder) resolved — all three, plus the frontmatter/LICENSE/delivery-mechanism decisions, had already been answered in the Merge Notes section by the time this phase ran, so nothing further needed to be asked.

### Verification performed
- **Rename status:** checked directly (`git ls-remote` against both the old and new repo URLs, then a fresh clone) rather than assumed. `SOC-Analyst-Portfolio` is live and reachable; the old `soc-analyst-roadmap` URL redirects to it. **The rename had already been applied on GitHub before this session** — no action needed here.
- **Current live repo contents:** the clone came back completely empty — not even the placeholder `README.md` from the Phase 0 audit remains. Commit history on `main`: `Initial commit` → `Update README.md` → `Delete README.md`. So this phase's skeleton is landing on a genuinely blank repo, not overwriting anything.

### Files Claude created (delivered as a downloadable scaffold, see chat)
- `.gitignore` — excludes `ISOs/`, `VMs/`, `*.iso`, `*.vmdk`/`*.vmx*`/related VMware files, OS junk (`.DS_Store`, `Thumbs.db`, `desktop.ini`), and Obsidian workspace/session state (`.obsidian/workspace*`, `.obsidian/cache`).
- `LICENSE` — MIT, copyright Siddharth Sharma, 2026.
- `plan.md` — this file, corrected (see Decisions below) and ready to be the in-repo copy.
- Empty directory skeleton, each held in git with a `.gitkeep` placeholder (no stub READMEs — no real content exists yet to justify prose in any of these folders, so an empty marker is more honest than a placeholder blurb):
  - `notes/security-fundamentals/.gitkeep`
  - `labs/week-1-lab-setup/.gitkeep`
  - `progress/.gitkeep`
  - `assets/.gitkeep`
- Root `README.md` was deliberately **not** created here — it's Phase 10's job (recruiter-facing content needs the rest of the repo to exist first).

### Decisions / corrections made during this phase
- Found and fixed a staleness bug in this plan file itself: the **Merge Notes** section (top of file) had already resolved Conflicts #1–#3 and the LICENSE/frontmatter/delivery-mechanism questions, but the **Project Status Snapshot** table, the **Locked Decisions Log** (#1 still said "UNRESOLVED"), and this phase's own header text had not been updated to match. Corrected all of them in this pass so the file is internally consistent again.

### Files/commands for you to run
Claude has no push credentials in this environment, so the scaffold is provided as a download. In your local clone (or a fresh `git clone https://github.com/sid-e-fi/SOC-Analyst-Portfolio.git`):

```bash
# from inside the repo root, after copying the downloaded scaffold files in
git add .gitignore LICENSE plan.md notes/security-fundamentals/.gitkeep labs/week-1-lab-setup/.gitkeep progress/.gitkeep assets/.gitkeep
git commit -m "Phase 1: repository scaffolding (.gitignore, LICENSE, folder skeleton, plan.md)"
git push origin main
```

### Expected output
Structured, empty-but-scaffolded repository plus this corrected `plan.md`.

### Validation criteria
- Repo confirmed reachable at `github.com/sid-e-fi/SOC-Analyst-Portfolio` (verified, not assumed).
- `.gitignore` matches `ISOs/`, `VMs/`, and the specified OS/Obsidian junk patterns.
- Structure matches the confirmed target tree above.

### Completion criteria
Rename confirmed live, skeleton created and delivered, exact `git` commands provided, `plan.md` corrected and updated. **Met.**

### Push confirmed
Verified live on GitHub (screenshot, 2026-08-14): `github.com/sid-e-fi/SOC-Analyst-Portfolio` now shows `assets/`, `labs/week-1-lab-setup/`, `notes/security-fundamentals/`, `progress/`, `.gitignore`, `LICENSE`, and `plan.md`, all under commit `8ad2b4e` — "Phase 1: repository scaffolding (.gitignore, LICENSE, folder skeleton...)". GitHub auto-detected the MIT license from the `LICENSE` file. Repo history now shows 4 commits total (the 3 from before this project started, plus this one). Contributor shown as `sid-e-fi` / Siddharth Sharma — consistent with Locked Decision #1.

**Phase 1 is fully complete — scaffold created, decisions reconciled, and now confirmed live upstream. Nothing outstanding.**

### Potential risks/problems
- None encountered. The rename was already done and the repo was empty, so this was a clean scaffold with no conflicting content to reconcile. Push went through cleanly once Personal Access Token auth was set up (password auth is deprecated on GitHub — expected friction, not a project issue).

### Next Phase
Phase 2: Migrate & Clean Security Fundamentals Notes — **COMPLETED.** See Phase 2 section below.

---

## Phase 2: Migrate & Clean Security Fundamentals Notes

**Status: COMPLETED (2026-08-14)**

### Objective
Move the reviewed notes from `01 Security Fundamentals/` into `notes/security-fundamentals/`, renamed to kebab-case, with frontmatter handled per the Phase 1 decision and wikilinks converted to relative Markdown links.

### Why it exists
This is the largest chunk of real, demonstrable learning content in the portfolio.

### Prerequisites
Phase 1 complete (folder skeleton exists, frontmatter decision confirmed). **Met.**

### Files received from owner
All 15 source notes, uploaded as `01_Security_Fundamentals.zip`.

### What was actually done
1. **Count corrected: 15 files, not 14.** The plan's prose said 14 throughout, but the uploaded folder — and the target structure tree already in this plan — both contain 15 distinct notes. Flagged to the owner rather than silently resolved; owner confirmed 15 is correct (2026-08-14). All "14" references elsewhere in this plan have been corrected to 15 (Locked Decision #5, Phase 0 findings table, Phase 0 content-count summary, Phase 0 quality-assessment line).
2. **Audited before touching anything:** confirmed no embedded images, no Obsidian callout blocks (`> [!tip]`), and no aliased (`[[X|display]]`) links exist anywhere in this note set — so the only edits needed were mechanical: filename, wikilinks, and one anchor link. Frontmatter in all 15 files is exactly `type` / `status` / `tags` — already-approved fields under the Phase 1 frontmatter decision — so **frontmatter was left completely untouched**, byte-for-byte.
3. **Renamed all 15 files to kebab-case**, using the exact filenames already locked in this plan's target structure tree (not a generic mechanical conversion — flagged and confirmed that `Identity and Access Management (IAM).md` → `identity-and-access-management.md`, dropping the `(IAM)` parenthetical, per the plan's own target list rather than a naive `...-iam.md` conversion).
4. **Converted all 234 wikilink instances** (`[[Title]]` → `[Title](kebab-case-filename.md)`) across the 15 files — display text preserved exactly as originally written, only the link target changed. Every target resolved to a real file already in this batch; there were no broken links to flag in this set (the one previously-flagged broken link lives in a Week 1 daily log, out of scope for this phase since raw daily logs stay vault-only per Standing Rule 3).
5. **Converted the one internal anchor link** in `CIA Triad and Basic Security Concepts.md`: `[[#Vulnerability vs Risk]]` → `[Vulnerability vs Risk](#vulnerability-vs-risk)`, matching GitHub's actual heading-slug rules (checked, not assumed).
6. Processed and delivered to the owner in 3 batches of 5 files, each validated and reviewed before the next batch ran, per the owner's request not to do multiple kinds of change in one pass.
7. **Self-validated the full set** (not just individual files): 234 total links across all 15 files, 0 broken `.md` link targets, 0 leftover unconverted `[[wikilinks]]`, 0 frontmatter mismatches (confirmed identical old vs. new for all 15), total line count identical before/after (9,454 → 9,454). Per-file, the number of changed lines exactly equals that file's wikilink count in every case — confirming no line was touched for any reason other than a link/anchor conversion.

### Files created
All 15 notes now live in `notes/security-fundamentals/`:
`what-cybersecurity-actually-is.md`, `cybersecurity-fundamentals.md`, `cia-triad-and-basic-security-concepts.md`, `defense-in-depth.md`, `risk-management.md`, `threat-actors-and-cyber-warfare.md`, `malware-and-social-engineering.md`, `network-security.md`, `vulnerabilities-and-patch-management.md`, `identity-and-access-management.md`, `authentication-and-authorization.md`, `identity-attacks.md`, `cryptocurrency-and-cryptojacking.md`, `incident-response.md`, `cisco-introduction-to-cybersecurity-course-notes.md`.

### Decisions made this phase
- 15 is the confirmed, correct note count (see "What was actually done" #1) — this plan's prose is now internally consistent on that number.
- Kebab-case filenames follow this plan's own pre-existing target list, not a generic algorithm, specifically to preserve the `(IAM)`-dropping convention already decided.
- Frontmatter required zero changes for this batch, since every field present already qualified as "GitHub-renders-cleanly" under the Phase 1 decision.

### Remaining
Nothing outstanding from this phase. Not yet delivered as a git commit — commands provided below (see "Files/commands for you to run"), pending owner's push per the Phase 1 delivery approach.

### Validation criteria — met
Every relative link resolves; no technical content altered in meaning; frontmatter treatment consistent (unchanged) across all 15 files.

### Completion criteria — met
All 15 files migrated, validated. Committed pending owner running the provided commands.

### Files/commands for you to run
```bash
# from inside your local clone of the repo
git add notes/security-fundamentals/
git commit -m "Phase 2: migrate and clean Security Fundamentals notes (15 files)

- Renamed from Obsidian Title Case to kebab-case
- Converted 234 wikilinks to relative Markdown links
- Converted 1 internal anchor link to GitHub heading-slug format
- Frontmatter and technical content unchanged"
git push origin main
```

### Potential risks/problems
None encountered — frontmatter needed no change, so the risk noted below never materialized.
~~If the frontmatter decision looks worse once actually applied, flag it rather than silently reverting.~~ *(Moot this phase — no frontmatter field required stripping or conversion.)*

### Next Phase
Phase 3: Build the Glossary — needs `13 Glossary/Cybersecurity Glossary.md` uploaded before it can run.

---

## Phase 3: Build the Glossary

**Status: NOT STARTED — Phase 2 ✅ complete; blocked on receiving the glossary source file from you (Conflict #2 already resolved: root location)**

### Objective
Migrate `13 Glossary/Cybersecurity Glossary.md` (742 lines) to `glossary.md` (location per Conflict #2), converting internal anchors and cross-references.

### Why it exists
Cross-cutting reference material with a different internal link structure (self-referencing anchors) than the topic notes handled in Phase 2.

### Prerequisites
Phase 2 complete (so cross-references to topic notes point at real, already-renamed files). Conflict #2 resolved.

### Files required from you
None — source file already provided.

### Files Claude will create
`glossary.md` at its confirmed location.

### Specific tasks
1. Migrate content verbatim.
2. Convert internal same-note anchors (`[[#Term|display text]]`) to GitHub-flavored Markdown anchors (`[display text](#term)`), respecting GitHub's heading-slug rules.
3. Convert cross-references to topic notes to relative links pointing into `notes/security-fundamentals/`.
4. Convert Obsidian callout formatting (`> [!tip]`) to a clean GitHub-Markdown equivalent (e.g. bold label + blockquote), since GitHub doesn't render Obsidian callout syntax natively.
5. Update `plan.md`.

### Expected output
A fully working, A–Z glossary with all internal and cross-file links resolving on GitHub.

### Validation criteria
Every glossary link resolves; callout formatting renders cleanly, not as raw `> [!tip]` text.

### Completion criteria
`glossary.md` committed, validated, plan updated.

### Potential risks/problems
GitHub's automatic heading-anchor slugs can differ from Obsidian's — each converted anchor needs a manual check, not a blind find/replace.

---

## Phase 4: Migrate Lab/Infrastructure Documentation

**Status: NOT STARTED — Phase 1 ✅ complete (Conflict #3 already resolved: labs/ in scope); blocked on receiving the 3 lab doc source files from you**

### Objective
Move `SOC-L1 Workspace.md`, `VMware Setup.md`, and `Windows 11 Lab.md` into `labs/week-1-lab-setup/` — pending confirmation this folder is still in scope (Conflict #3).

### Why it exists
Infrastructure/environment documentation is a different content category from security concept notes and progress logs — evidence of hands-on environment-building.

### Prerequisites
Phase 1 complete. Conflict #3 resolved (confirm `labs/` wasn't intentionally dropped in the update draft).

### Files required from you
None, unless host/VM specs or paths have changed since the backup (Ubuntu host, VMware Workstation Pro, Windows 11 Home guest per the backup — used as authoritative otherwise).

### Files Claude will create
- `labs/week-1-lab-setup/soc-l1-workspace.md`
- `labs/week-1-lab-setup/vmware-setup.md`
- `labs/week-1-lab-setup/windows-11-lab.md`

### Specific tasks
1. Migrate content verbatim, kebab-case filenames.
2. Convert the wikilink from `week-1-summary.md` that points here (destination filenames fixed now, string-level conversion happens in Phase 6).
3. Keep local filesystem paths as-is — genuine, non-sensitive lab documentation, not something to redact.
4. Update `plan.md`.

### Expected output
Three lab docs live at their new paths, ready to be linked from the Week 1 summary and README.

### Validation criteria
Content unchanged in meaning; filenames match what Phases 6/10 will link to.

### Completion criteria
Files committed, plan updated.

### Potential risks/problems
If Conflict #3 resolves to "drop labs/ entirely," this phase should be marked SKIPPED (not deleted from this plan) rather than removed, per Standing Rule 5.

---

## Phase 5: Finalize & Organize the Screenshot/Asset Decision

**Status: NOT STARTED — Phase 1 ✅ complete; blocked on the screenshot count/selection call (Conflict #4, deliberately deferred — ask again when this phase starts)**

### Objective
Confirm and execute Locked Decision #2: prepare the **2** approved screenshots (VM lab specs = P9, fresh-install snapshot = P10) as flat files under `assets/`, for embedding in the README's Environment section (Phase 8). The other 11 of the 13 reviewed screenshots stay vault-only.

### Why it exists
Currently zero images are referenced from any Markdown file, and the scope narrowed significantly from the original 13-screenshot plan (Conflict #4) — this phase locks in exactly what ships.

### Prerequisites
Phase 1 complete.

### Files/decisions required from you
1. The actual P9 and P10 image files.
2. Final confirmation that only these 2 ship (re-confirm Conflict #4 resolution) — or, if you'd prefer the original plan's fuller 13-screenshot/day-N-folder treatment instead, say so now before Phase 8 builds around the 2-image version.
3. Confirm filenames: `environment-vm-lab-specs.png` and `environment-fresh-install-snapshot.png`, or propose different names.

### Files Claude will create/change
- `assets/environment-vm-lab-specs.png`
- `assets/environment-fresh-install-snapshot.png`

### Specific tasks
1. Rename/place the 2 approved files under `assets/` (flat, no per-day subfolders, since there are only 2).
2. Confirm the other 11 screenshots are explicitly noted as vault-only (not silently dropped — recorded here for traceability).
3. Update `plan.md`.

### Expected output
Two correctly named image files in `assets/`, ready for Phase 8.

### Validation criteria
Both images open correctly after placement (no corruption).

### Completion criteria
Assets committed, plan updated, Phase 8 unblocked.

### Potential risks/problems
Large screenshots (originals ~9–11 MB each) may be worth compressing for a faster-loading GitHub README — flag as an option, don't compress unilaterally without confirming quality loss is acceptable.

---

## Phase 6: Build the Week 1 Progress Summary Page

**Status: NOT STARTED — blocked on Phases 2–5**

### Objective
Finalize `progress/week-1-summary.md` from the existing (already well-written) source, fixing its internal links. This is the **only** Week 1 progress artifact that ships to GitHub, per Standing Rule 3.

### Why it exists
Likely the first page recruiters read after the README — needs to be complete and polished, unlike the raw daily logs (which stay in the vault).

### Prerequisites
Phases 2, 3, 4, and 5 complete (so every link target actually exists at its final path).

### Files/decisions required from you
Confirmation of the Day 5 date / date-range questions from Phase 0's Gap #3 if they affect this page's wording ("Dates covered: 25 to 31 July 2026").

### Files Claude will create
`progress/week-1-summary.md`

### Specific tasks
1. Migrate content verbatim except for link conversion.
2. Convert `[[VMware Setup]]`, `[[Windows 11 Lab]]`, `[[Cybersecurity Glossary]]`, `[[Cisco Introduction to Cybersecurity - Course Notes]]`, and all topic-note wikilinks to their final relative Markdown paths.
3. **Do not** embed the P9/P10 screenshots here — those live in the README's Environment section only (Locked Decision #2), not duplicated into this page.
4. Resolve the date-range wording once you confirm actual dates — don't silently change "25 to 31 July" if it's wrong; ask, then fix.
5. Update `plan.md`.

### Expected output
A complete, portfolio-quality Week 1 summary page with working links.

### Validation criteria
Every link resolves; content still accurately reflects what you actually did — no invented achievements.

### Completion criteria
File committed, plan updated.

### Potential risks/problems
None major, assuming Phases 2–5 are genuinely complete first.

---

## Phase 7: *(Superseded)* Migrate Raw Daily Logs

**Status: SKIPPED / SUPERSEDED — not deleted, kept for history**

### Why this phase is skipped
The original `plan.md` had this as an active phase (migrate all six `Week 1 - Day N.md` files into `progress/daily-logs/week-1/day-N.md`). `plan update.md` introduced Standing Rule 3: **raw daily logs never go to GitHub, only weekly rollups.** This master plan adopts that rule (Conflict #5).

### What would have happened (preserved for reference, not executed)
- Six files would have been created: `progress/daily-logs/week-1/day-1.md` through `day-6.md`.
- The broken wikilink in Day 4 (`[[Identity & Access Management]]`) would have needed explicit confirmation before converting.
- The Day 5 placeholder date and the Day 1/3/4 date-gap issue would have needed real dates or an explicit "not recorded" from you.

### If you want to revisit this
Say so explicitly — this plan will re-activate the phase using the file list above rather than re-deriving it from scratch. Until then, daily logs remain vault-only per Standing Rule 3.

---

## Phase 8: Add "Environment" Section to README (Embed Approved Screenshots)

**Status: NOT STARTED — blocked on Phase 5**

### Objective
Add a short **Environment** section to the main `README.md`, embedding the 2 approved screenshots from Phase 5 (VM lab specs, fresh-install snapshot).

### Why it exists
Per Locked Decision #2, this is the *only* place either screenshot appears in the repo.

### Prerequisites
Phase 5 complete (image files placed and named).

### Files/decisions required from you
None beyond what Phase 5 already gathered.

### Files Claude will create/change
- `README.md` (Environment section only — full README rewrite happens in Phase 10)

### Specific tasks
1. Write a brief Environment section: what the lab is (VMware Workstation Pro + Windows 11 guest on an Ubuntu host), why it exists, what the two images show.
2. Embed both images with correct relative paths (`assets/environment-vm-lab-specs.png`, `assets/environment-fresh-install-snapshot.png`).
3. Update `plan.md`.

### Expected output
A README with a working, image-embedded Environment section (rest of the README still pending Phase 10).

### Validation criteria
Both images render correctly on GitHub; paths resolve.

### Completion criteria
Section committed, plan updated.

### Potential risks/problems
None significant — mechanical once Phase 5's files exist.

---

## Phase 9: Validate the Full Week 1 Repository

**Status: NOT STARTED — blocked on Phases 1–8**

### Objective
End-to-end check of everything migrated in Phases 1–8 before writing the full public-facing README.

### Why it exists
A recruiter hitting a broken link or missing image on the first visit is worse than a slightly delayed launch.

### Prerequisites
Phases 1–8 complete.

### Files required from you
None, unless validation surfaces something needing a decision.

### Specific tasks
1. Check every internal Markdown link across every migrated file resolves.
2. Check both embedded images resolve and aren't corrupted.
3. Confirm filename consistency (kebab-case throughout, no leftover spaces/symbols).
4. Confirm frontmatter treatment was applied consistently.
5. Re-read all migrated content once more for any accidental meaning changes introduced during Phases 2–8.
6. Update `plan.md` with validation results.

### Expected output
A validation report (in the `plan.md` update) confirming internal consistency, or a list of specific fixes needed.

### Validation criteria
Zero broken links, zero broken images, consistent naming and formatting.

### Completion criteria
Clean validation pass, or all flagged issues resolved and re-validated.

### Potential risks/problems
None — this is a checking phase, not a content-creation phase.

---

## Phase 10: Write the Recruiter-Facing Root README

**Status: NOT STARTED — blocked on Phase 9**

### Objective
Write the full `README.md` at repo root: the front door of the portfolio (the Environment section from Phase 8 stays; everything else is added here).

### Why it exists
Deliberately one of the last phases — a good README needs to accurately describe content that already exists and has been validated.

### Prerequisites
Phase 9 complete.

### Files/decisions required from you
- Resolved display name (Conflict #1) — required before this phase can write an accurate byline.
- Any preferences on tone/framing, links to include (LinkedIn, etc.), or achievements to highlight beyond what's already documented. Not required to proceed — Claude can draft from validated content alone if you'd rather review after.

### Files Claude will create
`README.md` (full version)

### Specific tasks
1. Concise intro: who you are (SOC Analyst L1 track, in progress — not overstated as "experienced"), what this repo documents.
2. Repository structure overview/navigation table.
3. "Week 1 highlights" section linking to `progress/week-1-summary.md` and 2–3 standout notes.
4. Link to `glossary.md`.
5. Keep the Environment section from Phase 8 intact.
6. A visible roadmap/status section: done (Week 1) vs. planned (Networking, Linux, Windows, SIEM, MITRE ATT&CK, etc.) — honest, not overstated.
7. Update `plan.md`.

### Expected output
A README that accurately, professionally represents exactly what's in the repo — no more, no less.

### Validation criteria
Every claim in the README traces to real migrated content; no invented certifications, labs, or skills.

### Completion criteria
`README.md` committed, plan updated.

### Potential risks/problems
Risk of overclaiming ("SOC Analyst" instead of "SOC Analyst L1 — in progress") — actively guard against this.

---

## Phase 11: LICENSE, .gitignore Review, Final Polish

**Status: NOT STARTED — blocked on Phase 10**

### Objective
Final pass: confirm `.gitignore` still correctly excludes `ISOs/`/`VMs/` now that real content exists elsewhere, confirm `LICENSE` is in place if requested, check for stray files.

### Prerequisites
Phase 10 complete.

### Specific tasks
1. Re-check `.gitignore` against the final repo tree.
2. Confirm no secrets, credentials, or `.env`-type files were ever introduced.
3. Final directory-tree sanity check against the structure agreed in Phase 1.
4. Update `plan.md`.

### Expected output
A repository ready for its first real push.

### Completion criteria
Clean final check, plan updated.

---

## Phase 12: Commit & Push to GitHub (Including the Rename)

**Status: NOT STARTED — blocked on Phases 1–11**

### Objective
Get everything from Phases 1–11 into the actual `sid-e-fi/SOC-Analyst-Portfolio` repository on GitHub.

### Why it exists
Claude does not have authenticated git push access in this environment (network access is limited to read-only fetch; no credentials configured). This phase is where you actually apply the changes, using commands Claude provides.

### Prerequisites
Phases 1–11 complete locally/in-session. Rename from Phase 1 applied on GitHub.

### Specific tasks
1. Claude groups the migrated files into logically separate commits (not one giant commit) — e.g. scaffolding, Security Fundamentals notes, glossary, lab docs, assets, progress summary, README/LICENSE — matching the phase boundaries above.
2. Claude provides exact `git` commands for each commit.
3. You run them and confirm push succeeded.
4. Spot-check the live GitHub rendering for broken links/images post-push, since local validation (Phase 9) can occasionally miss GitHub-specific rendering quirks.
5. Update `plan.md`: mark repository as live, record the commit sequence used.

### Completion criteria
All content live on GitHub `main` branch at the new repo name, rendering correctly.

### Potential risks/problems
GitHub's Markdown renderer occasionally handles relative links/anchors slightly differently than local preview — worth a live spot-check, not just trusting Phase 9's local validation.

---

## Phase 13: Scaffold Future Roadmap Sections

**Status: NOT STARTED — blocked on Phase 12**

### Objective
Once Week 1 is fully live, add lightweight scaffolding (not fake content) for future topics: Networking, Linux, Windows, SIEM, TryHackMe, PortSwigger, Incident Reports, MITRE ATT&CK, Cheat Sheets, and the `Projects/` categories already present as empty folders in the vault.

### Why it exists
Keeps the repository structure stable and ready to receive Week 2+ content without another restructure, and lets the README show an honest "in progress" roadmap.

### Prerequisites
Phase 12 complete (Week 1 live).

### Files/decisions required from you
Whichever topic you study next, and its actual source material when ready — this phase does not execute speculatively for content that doesn't exist yet.

### Specific tasks
1. Add empty topic subfolders under `notes/` only as each topic is actually started (not all at once) — a "Roadmap" section in the README can represent not-yet-started topics without needing empty folders in git.
2. When a new week/topic begins, repeat the Phase 2–10 pattern for that topic's content.
3. Update `plan.md` with a new phase block for each new topic, following the same template used above.

### Completion criteria
Ongoing — this phase effectively restarts the Phase 1–12 cycle for each new topic, and `plan.md` grows a new set of phases each time.

---

## Session Resumption Note

If a new session starts with "continue the SOC repo" (or similar):
1. Read this file top to bottom.
2. Check the [⚠️ Merge Notes & Open Conflicts](#️-merge-notes--open-conflicts-resolve-before-phase-1) section first — if any of Conflicts #1–#3 are still unresolved, ask for them before doing anything else.
3. Find the lowest-numbered phase that isn't `COMPLETED` or `SKIPPED`.
4. Check its prerequisites/blockers.
5. Ask only for what's genuinely missing — never re-ask questions already answered in the Locked Decisions Log.
6. Do not restart. Do not re-derive decisions already made above.
