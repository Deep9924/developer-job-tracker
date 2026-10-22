# Build me a Developer Job Tracker in Muse

Paste this whole file to your Muse. It will ask you for the bracketed details, then build the tracker as a private web app (artifact) inside your Muse, plus an optional daily scheduled check.

Everything in [BRACKETS] is yours to fill in. Nothing here is anyone else's real data.

---

## 1. Who I am (the Muse should ask me for these if I didn't fill them in)

- Name: [YOUR NAME]
- Email (for applications): [YOUR EMAIL]
- Phone: [YOUR PHONE]
- City/region: [YOUR CITY, PROVINCE/STATE]
- Timezone: [YOUR TIMEZONE, e.g. America/Toronto]
- My verified resume facts (skills, experience, portfolio projects — the tracker and the check may only ever claim these; never pad):
  - Languages: [e.g. TypeScript, JavaScript, Python, SQL]
  - Frontend: [e.g. React, Next.js, Tailwind]
  - Backend: [e.g. Node.js, Express, REST APIs]
  - Data: [e.g. PostgreSQL, MySQL, Drizzle ORM]
  - Cloud/tools: [e.g. AWS, Docker, Vercel, GitHub Actions]
  - Work history: [EMPLOYER, ROLE, DATES, 2-3 FACT LINES EACH]
  - Portfolio projects: [PROJECT NAME — what it is, stack, size facts that are true]
  - Skills I do NOT have (name gaps honestly when a posting requires them): [e.g. Go, Kubernetes, GraphQL]

## 2. What to build

A fullstack web artifact named **Developer Job Tracker**, private to me, that stores:

- **Positions**: company, role, location, work arrangement (remote/hybrid/on-site as stated), employment types (as stated on the posting, normalized), salary text exactly as stated + whether it is *verified* (employer-stated) or *unverified* (board/aggregator), source URL, posting date, deadline/apply-by date when stated, the folder that found it, a fit/gate note, a status, a "package selected" switch (default off), and attached documents (tailored resume + cover letter, PDF and DOCX).
- **Folders (search profiles)**: the search definitions in section 4.
- **Deleted positions**: postings I removed (section 6).
- **Check runs + check log**: every check run, its outcome, and a dated log line per run.
- A plain-text **register file** in my goal/workspace that mirrors the board (tracked positions, gates, deadlines, folder mirror, deleted list, check log) so the daily check and I share one canonical list.

**Statuses** (exactly these 8): New find, Package ready, Intro drafted, Applied, Interviewing, Offer, Rejected, Closed.

## 3. The board (layout)

- Positions are **grouped by folder**. Folders are header rows (name, a summary line like "11 positions · 4 closing soon · 7 fresh" — segments only when non-zero — chevron, Run button, on/off switch), not enclosing cards.
- Positions are **bordered boxes**. On desktop the folders are always open and every box shows all its content; on mobile a box shows the main info and expands on tap.
- Position box contents: deadline at top-right when present ("Nov 18" + "in 28 days"); company (bold) and role; job types as tags directly under the role; meta line = location · Posted <date> (<relative age>); salary with a verified/unverified label and an Hourly/Yearly kind label; a Fresh pill for postings 0-2 days old or a Catch-up tag; status text; a compact "View posting" button on that same line when a source URL exists. Actions row: status select, Details/Hide details (view-only details — no editing fields), View posting, Delete — one uniform 40px bordered-button row.
- Ordering inside each folder: **New first (newest first), then pipeline order (Package ready → Intro drafted → Applied → Interviewing → Offer), Rejected/Closed at the bottom. Within each status group, sort by closing date: earliest closing first, later ones after; positions with no closing date sit at the end of their group.**
- Top: a sticky counts header (open / fresh / closing soon / needs your call) and a single sun/moon button for Light/Dark (device default on first load, choice persists).
- One bordered **filter box**: a search bar on its face ("Search company, role, or place", one clear × button), the status filter as ONE horizontally scrollable row of uniform 40px cells (active = filled + bold), and an "Advanced filters" expander with two salary rows ("Hourly min" / "Yearly min", $ prefix, /hour or /year suffix) plus "Include no salary" with a standard-size switch. Salary filtering: a posting's pay kind is as the posting states it (explicit unit wins; otherwise top figure < 1000 = hourly, >= 1000 = yearly); the two minimums never cross-apply; figures are never converted.
- A compact **closing-soon banner**: company — role, closes date, time left, status, folder name; tapping it opens that folder, expands the position, and scrolls to it.
- One always-present **status line at fixed height**: idle shows "Last run: <date, time>" (date/time only); running swaps in progress (section 5). Run/cancel controls swap in place inside fixed-width slots — nothing on the page shifts or mounts/unmounts between states.
- Bottom: "Deleted positions · N" and "Check log · N entries" as small muted rows, one tap to open; Deleted has its own small search bar.
- Plain system sans-serif, flat surfaces, no decorative fonts, no heavy shadows or gradients. Mobile must never scroll sideways at page level. Scrolling is plain browser-default — no custom scroll CSS or JS.

## 4. Folders (search profiles)

Each folder carries, all editable behind a closed-by-default "Folder settings" expander (a >=48px full-width row inside the folder's block, one Edit button beside its header; every setting appears exactly once; lists are read-only until Edit):

- **Looks for** (include wordings): title phrases, matched by *similar wording* (hyphenation/spacing/word order don't matter). OPTIONAL: an empty list means NO title restriction — match the other filters only, for any kind of role.
- **Skips** (exclude wordings).
- **Job types**: any of full-time, part-time, permanent, contract, temporary, casual, seasonal, term, internship, with a **type-match** setting: "any" (default) or "all" (the posting must name every selected type). No types selected = no restriction. Type matching normalizes phrasing: hyphens/spaces ignored, "fulltime" = "full-time", "parttime" = "part-time", FT/PT count, combinations split ("casual part-time" = casual + part-time, "fixed-term" = term, "term or contract" = term + contract).
- **Where** (regions).
- **Salary**: two independent minimums (hourly, yearly) + an include-no-salary toggle (default on). A find files only if it passes its own kind's minimum (judged on the top of its stated range); no-salary postings file only when the toggle is on; skipped-on-salary finds are counted in the run's skipped summary.
- **Companies** (specific names — checked at their own career pages/ATS) and **Categories** (retail, IT/technology, healthcare, finance, government/public sector, education, manufacturing, trades/construction, hospitality/food, transportation/logistics, energy/utilities, or my own labels).
- **Skills** (list + any/all match): a posting files only if its description/body actually mentions the listed skills.
- **Level** (entry/mid/senior): Senior/Sr./Lead/Principal/Staff or 5+ years = senior; Junior/entry/new-grad = entry; anything else or unclear = mid.
- **Work arrangement** (remote/hybrid/on-site as stated). A remote-only folder skips on-site roles entirely.
- **Search back (days)**: 1-90, unset = 30 (see the lanes in section 5).
- **Websites** list, **Never use** list, and an **"Also search the whole web"** switch. Site mode is derived: web on + sites listed = priority (listed sites first, wider web allowed); web on + none listed = web search skipping Never-use sites; web off = ONLY the listed sites. Never-use sites are excluded in every mode.

Starter folders to create (adjust to my answers):
1. "Full-stack TypeScript, remote Canada" — Looks for: Full Stack Developer, Full-Stack Software Engineer, Software Engineer, Software Developer, SWE; Skips: internship, co-op, part-time; Types: full-time, permanent, contract (any); Where: Remote [MY COUNTRY], [MY PROVINCE]; Sites: employer career pages, Greenhouse, Lever, [NATIONAL JOB BANK].
2. "Frontend React" — Looks for: Frontend Developer, Front End Engineer, React Developer, Frontend Engineer, UI Developer; Skips: internship, part-time; Types: full-time, permanent; Where: Remote [MY COUNTRY], Remote US; Sites: employer career pages, Greenhouse, Lever, Wellfound.
3. "[MY PROVINCE] local" — Looks for: Software Developer, Developer, Web Developer, Programmer; Skips: internship; Types: full-time, part-time, contract, permanent; Where: [MY CITY], [MY PROVINCE]; Sites: [NATIONAL JOB BANK], employer career pages.

## 5. Check runs (the heart of it)

Runs are started by each folder's **Run** button (runs that folder even if its switch is off), by **Run check now** (runs the switched-ON folders that aren't already running), or by the daily scheduled check (section 7). A run is a Muse agent doing real web research, not a fixed script.

**One run per folder at a time; different folders may run at the same time.** Each run gets its OWN worker — after starting, the run verifies its worker is actually live; if the worker handle turns out to be an already-finished one, the run fails on the spot with a plain message instead of sitting "running" with nobody behind it. (This bug bit us: runs shared one spawn key per folder, so re-runs inherited a dead worker and hung silently.)

For each folder a run executes:

- **Re-verify** that folder's tracked postings (still listed? deadline/salary/location/type changed? mark closures Closed with the date seen).
- **Discover** in two lanes: FRESH = posted within the last 7 days (3 days or newer flagged "fresh"); CATCH-UP = first-time finds posted 8 days ago up to the folder's Search-back limit, labeled "catch-up" with their real age. A folder with Search back under 8 days has no catch-up lane. Undated or older-than-limit postings are outside discovery (tracked ones are still re-verified).
- **Listing-level gate (speed)**: screen each candidate against every filter its listing can settle — title wording, shown posting date vs the window, region, stated type/salary/arrangement, duplicates — and skip at the first mismatch WITHOUT opening the posting, tallying the skip by reason. Read each results page once and screen all its candidates from it. Open a detail page only for candidates that pass everything the listing can judge; body-level checks (skills, unstated fields) get their full read for those survivors only. A tracked posting a listing confirms unchanged (still listed, same deadline) gets no detail-page open. High-volume boards are capped at 200 candidate listings per folder per source, with coverage reported ("Checked N of M listings") — a capped sweep is marked partial, never silent.
- **File live**: each verified find is filed to the board immediately (documents Not drafted, package switch off), not in one batch at the end; anything filed live is never filed twice (dedupe by company + role + link is re-checked on every report, so two folders running at once can't double-file). Salary recorded only from the posting itself, labeled verified (employer-stated) or unverified (board/aggregator).
- **Prune**: positions already filed under the folder are re-checked against its CURRENT filters; one that no longer matches is moved to Deleted ONLY if its status is still New find (any progressed status is never pruned), with a note that the run removed it for no longer matching.
- **Progress**: while running, the folder's block shows elapsed time ticking, the current step, counts filed so far, "Folder X of Y" on multi-folder runs, and a per-folder **token estimate** ("≈ N tokens") calibrated from that folder's previous finished runs (typical of the last 5; falls back to any folder's history, then a flat base) — the estimate improves as the folder builds history; each finished run keeps its own reported usage on its record.
- **Watchdog**: a run that makes no first progress report within 10 minutes, or goes 20 minutes without reporting after having reported, closes as failed with a plain message ("The check never reported in"). 90 minutes is only an absolute cap.
- **End of run**: a one-line **skipped summary** (e.g. "Skipped 3: 2 salary, 1 job type") on the Last run line and in the check log.
- **Cancel**: per run; positions already filed stay (a stop, not an undo); late results for a cancelled run are refused.

## 6. Delete and restore

Delete sits on each position's main card face: one tap deletes instantly, with NO confirmation step, and an Undo bar (about 15 seconds) appears — Undo restores the position exactly and lifts its posting block. Each folder also has a Select mode (checkboxes on the cards, Select all, a count) with a Delete selected action: that one DOES ask first, with a confirmation stating how many positions will be deleted, then deletes and offers the same single Undo to bring the whole batch back. Deleting removes the position from the board and counts and blocks ONLY that specific posting from future runs — identified by its source link, or by company + role + posting date when no link is recorded — never the whole company or the title in general (a genuinely new posting with the same title at the same company files normally). A Deleted list shows deletions with Restore; restoring returns the position exactly as it was and lifts the block. There is no separate "excluded" status — Delete is the one removal method.

## 7. The daily check (scheduled)

Create a scheduled agent job, daily at [06:00 MY TIMEZONE], owned by my job-search goal, that:

1. Reads the register file and the dashboard's active folders (with their deleted lists).
2. Re-verifies every tracked open posting; skips deleted ones entirely.
3. Runs each ACTIVE folder exactly as in section 5 (both lanes, listing gate, live filing, pruning, skipped summary + its estimated token usage in the log line).
4. Updates the register file and the dashboard so they match.
5. Sends me a daily reminder in this chat, every day, even when nothing is new:
   - **New today**: fresh-lane finds grouped by folder (company, role, location, salary labeled verified/unverified, posting date, "fresh" flag if <= 3 days, link) — or "New today: none."
   - **Catch-up finds**: with posting age in days — or "Catch-up finds: none."
   - **Closing soon**: deadlines within 7 days, plus closures and material changes.
   - **Waiting on your call**: gated positions by name (eligibility, location, or contract flags).
   - **Open positions**: count + company names as one compact comma-separated list. Deleted positions are never counted or listed.

## 8. Standing rules (never bend)

- Match roles against my verified resume facts only (section 1); name gaps honestly, never pad a claim.
- Salary: only employer-stated bands are "verified"; board/aggregator figures are labeled unverified; never invent a range or a posting detail.
- Country scope: [MY COUNTRIES]. Remote roles must be checked for eligibility in [MY PROVINCE/STATE]. If my work authorization for another country is unknown, keep those roles flagged as gated — never assume it.
- **Never apply, submit, or message an employer on my behalf.** Finding, verifying, and tracking are the job; submitting is mine.
- **Packages**: tailored resumes/cover letters are built ONLY for positions I select (dashboard switch or naming them in chat). New finds are always filed Not drafted, package off. The check itself never builds packages.
- Verification testing happens only in the sandbox copy — never start or drive check runs against the live tracker just to test a build.
