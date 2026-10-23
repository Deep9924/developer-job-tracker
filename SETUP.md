# Build me a Developer Job Tracker in Muse

Paste this whole file to your Muse. It will ask you for the bracketed details, then build the tracker as a private web app (artifact) inside your Muse, plus an optional daily scheduled check.

Everything in [BRACKETS] is yours to fill in. Nothing here is anyone else's real data.

**For the Muse building this: sections 1-8 describe the product. The APPENDIX at the end is normative — exact tables, fields, actions, constants, and rules. Build to the appendix where the two differ in precision.**

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
- Positions are **bordered boxes**. Folders and boxes behave the SAME at every width: folders start closed and open on click/tap of the header row, and a box shows the main info and expands on click/tap (card header or the Details button). There are no desktop-only always-open overrides — desktop is not a separate mode (exact structure and sizes in A12).
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
- **Companies** (specific names — checked at their own career pages/ATS) and **Categories** (retail, IT/technology, healthcare, finance, government/public sector, education, manufacturing, trades/construction, hospitality/food, transportation/logistics, energy/utilities, or my own labels). Targeting is ADDITIVE ONLY: listed companies are checked directly IN ADDITION TO the folder's normal sources — a find from an unlisted employer files normally, and a filed position is never pruned for coming from an unlisted company or category. Only the skip lists below ever block an employer.
- **Companies to skip** and **Categories to skip**: omit lists — a check never files anything from a skipped company or a skipped category (same category names as targeting; e.g. add healthcare/health to never see health roles). A skip beats a target: if a company or category is in both the target and skip lists, it is skipped. Skips are judged at the listing stage and tallied in the run's skipped line ("skipped company" / "skipped category").
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

---

# APPENDIX — Exact technical specification (normative)

Build to this appendix. Where sections 1-8 and the appendix differ in precision, the appendix wins.

## A1. Data model (tables and fields)

**positions**
- id; profile_id (folder, nullable = unfiled); company (text); role (text); location (text)
- salary_text (exactly as the posting states it, e.g. "$50-$55/hour" or "CAD $133,000-$183,000"); salary_source: `verified` (employer-stated) or `unverified` (board/aggregator); no salary = empty salary_text and the card shows "No salary posted"
- source_url (nullable; http/https only — writes with any other scheme are rejected, and the render of "View posting" is defensively guarded the same way)
- posted_date (YYYY-MM-DD, real calendar date, validated); deadline (YYYY-MM-DD, nullable — only when the posting states an ending/apply-by date; never inferred)
- employment_types (array of canonical types, from the posting's own statement, normalized per A4; empty when the posting states none)
- arrangement (remote / hybrid / on-site, only as stated)
- catch_up (boolean — true when found in the catch-up lane)
- status: one of `new`, `package_ready`, `intro_drafted`, `applied`, `interviewing`, `offer`, `rejected`, `closed` (display labels: New find, Package ready, Intro drafted, Applied, Interviewing, Offer, Rejected, Closed)
- package_selected (boolean, default false); documents status derived from document_files (not drafted until files exist)
- note (fit/gate text, e.g. eligibility flags, honest gaps); job_description (text, when captured)
- created_at / found ordering fields so "newest first" is deterministic within the pre-sort list (the board re-sorts per A6)

**search_profiles** (folders)
- id; name; active (boolean — the on/off switch)
- include_terms[]; exclude_terms[]
- employment_types[]; type_match: `any` | `all`
- salary_min_hourly (number, nullable); salary_min_yearly (number, nullable); include_no_salary (boolean, default true)
- regions[]
- sites[]; excluded_sites[]; whole_web (boolean — the "Also search the whole web" switch). Derived site mode: whole_web on + sites non-empty = `priority`; whole_web on + sites empty = `blacklist` (web sweep, excluded_sites skipped, listed sites included in the pool); whole_web off = `whitelist` (only sites). Store the derived inputs, compute the mode; excluded_sites are skipped in every mode.
- companies[]; categories[]; excluded_companies[] (Companies to skip); excluded_categories[] (Categories to skip)
- skills[]; skills_match: `any` | `all`
- levels[] (subset of entry/mid/senior)
- arrangements[] (subset of remote/hybrid/on-site)
- search_days (integer 1-90, nullable; null = 30)

**deleted_positions**
- A full snapshot of the position row at deletion time, plus: source_url (copied), note (why deleted, e.g. a run-pruning note), deleted_at.
- The future-run block key is: source_url when present, else company + role + posted_date. Matching uses the same similar-wording normalization as titles. Restore returns the position exactly as snapshotted and removes the block.

**document_files**
- id; position_id; kind: `resume` | `cover_letter`; format: `pdf` | `docx`; the file blob stored in the artifact's storage; filename follows `Deep-Patel-Resume-<Company>` style with the user's own name: `<First>-<Last>-Resume-<CompanyToken>.pdf/.docx` and `<First>-<Last>-Cover-Letter-<CompanyToken>`.

**check_runs**
- id; scope: `all` | `profile`; profile_id + profile_name (nullable for `all`); status: `running` | `done` | `failed` | `cancelled`
- started_at; finished_at (nullable); message (outcome/failure text)
- added; catch_up; closed; updated (counts)
- task_id (the spawned worker's handle)
- progress_note (current step text); progress_updated_at (nullable)
- token_estimate (the pre-run estimate shown during the run); token_usage (the run's own reported figure, nullable until reported)
- folders_total; folder_estimates (per-folder estimates for multi-folder runs); skip_counts (map of reason → count); skipped_summary (the one-line text)

**check_log**
- id; entry text (dated run lines and material-change lines); created_at. Displayed newest-first in a collapsed-by-default section; the register file mirrors these lines.

**Indexes**: positions(profile_id), positions(company), deleted_positions(source_url), check_runs(status).

## A2. Actions (the artifact's complete action surface)

1. `getstate` — everything the board renders in one call: positions, profiles, deleted_positions, document metadata, check_log, latest run, **all currently running runs** (running_runs array), and counts. While any run is running the client polls getstate every **5 seconds**; otherwise it does not poll.
2. `add_position` / `update_position` / `upsert_position` — manual position management. All inputs bounded (string/array length caps); dates validated as real YYYY-MM-DD; source_url must be empty or http(s).
3. `apply_partial_results` — the live-filing path used DURING a run: files new finds for a specific run_id as they are verified (same gates as the final report — salary, type, site, window, deleted-block, dedupe), increments the run's counters, and sets progress_note/progress_updated_at. A progress-only call (no finds) is valid and is how the worker checks in. Refused (files nothing) when the run is not in `running` status — this is what makes cancelled runs refuse late results.
4. `apply_check_results` — the run's final report: closures, updates to tracked positions, pruning (A5), any finds not already filed live, the skipped summary, and the run's reported token usage; closes the run as `done`.
5. `append_check_log` — appends a dated log line.
6. `add_profile` / `update_profile` / `delete_profile` — folder CRUD with the fields in A1.
7. `run_position_check` — starts a run. With a profile_id: runs that one folder **even if its switch is off**. Without: runs only folders with active = true (no broad fallback when none are on). Guard is **per folder**: a folder with a run already `running` refuses a second run for itself; other folders are unaffected. "Run check now" skips folders already running. Spawning: the worker is spawned with a dedupe key unique to the run row, `position-check-run-<run_id>` — NEVER a per-folder key (a shared key returns a finished worker's handle and the run hangs silently). Immediately after spawning, the task's status is checked; if it already reads finished/terminal, the run is failed on the spot with the message "The check could not get a live worker."
8. `cancel_position_check` — per run: closes the run row as `cancelled`; filed positions stay; the spawned task itself cannot be force-stopped through its handle (its later reports are refused by rule 3). A second cancel is a no-op; an open cancel confirmation closes automatically when the run ends.
9. `delete_position` — single delete: snapshots the row into deleted_positions, removes it from positions, returns the deleted record's id (the client's Undo targets exactly that id). NO confirmation in the UI.
10. `delete_positions` — bulk delete for folder multi-select: same snapshot semantics per position. The UI confirms first, stating the count.
11. `restore_position` / `restore_positions` — restore from a deleted snapshot (single and bulk share one server helper): the position returns exactly as snapshotted and its block is removed.

Authorization note: the artifact itself has no in-app user/role system; access rests on the platform restricting the artifact to its owner. State this in a comment at the top of the actions file so nobody mistakes it for an oversight.

## A3. Salary rules

- A posting's pay kind is as the posting states it — hourly or yearly — never converted.
- Classification: an explicit unit marker in the salary text ("/hour", "per hour", "/yr", "per year", "annually", etc.) always wins. Otherwise the TOP figure of the stated range classifies for all sources: **< 1000 = hourly, >= 1000 = yearly**. Figures themselves are never changed.
- Folder salary gate: an hourly posting files only if the top of its stated hourly range >= the folder's salary_min_hourly (no hourly minimum set = unrestricted); yearly likewise against salary_min_yearly. A posting with no parseable salary counts as no-salary and files only when include_no_salary is on. Salary skips are tallied in the run's skip_counts under `salary`.
- Board salary filter uses the same classification and the same two independent minimums; minimums never cross-apply.
- Labels: salary shows with `verified` only when employer-stated; board/aggregator figures are `unverified`. Never invent a range.

## A4. Matching rules (shared by the board, the server, and the run instructions — implement ONCE in a shared module both sides import)

- **Title matching** is by similar wording: normalize case, hyphens, spacing, and word order; punctuation ignored. "Full-Stack", "Fullstack", "Full Stack" are one wording.
- **Type normalization**: canonical set = full-time, part-time, permanent, contract, temporary, casual, seasonal, term, internship. Ignore hyphens/spaces ("full time" = "full-time"); one-word forms count ("fulltime", "parttime"); abbreviations FT/PT count; combinations split ("casual part-time" = casual AND part-time; "permanent, full-time" = both; "term or contract" = term + contract; "fixed-term" = term). Type-match `any`: the posting matches if ANY type it names is selected. `all`: it must name EVERY selected type. A posting that states no type fails a folder that has types selected in `all` mode, and passes type filtering in `any` mode only if it names at least one selected type — when in doubt, the posting's own statement rules; never infer a type.
- **Level derivation**: title/requirements naming Senior, Sr., Lead, Principal, Staff, or 5+ years = senior; Junior, entry-level, new grad = entry; anything else or unclear = mid. A posting files only if its derived level is in the folder's levels (empty = unrestricted).
- **Arrangement**: as the posting states it (remote/hybrid/on-site), normalized spelling variants only. A posting files only if its stated arrangement is selected (empty = unrestricted); a remote-only folder skips on-site postings entirely (skip reason tallied, not filed as gated).
- **Skills**: matched against the posting's description/body text, `any` = at least one listed skill mentioned, `all` = every one. Empty = unrestricted. This is a body-level check: it can only run on a candidate that survived the listing gate (A5).
- **Discovery lanes**: age = today − posted_date, in the user's timezone. Fresh lane: age 0-6 days... precisely: posted within the last 7 days. The "Just posted" pill and folder "fresh" counts use age 0-2 days. Catch-up lane: age 8 days up to the folder's search_days (null = 30, clamped 1-90). A folder with search_days < 8 has NO catch-up lane. Finds older than the folder's limit are skipped with reason `old`. Undated postings are outside discovery. Tracked postings are re-verified regardless of age.

## A5. Filing gates, dedupe, pruning (server-side, on every report)

Order per candidate: (1) deleted-block check (A1 key) → skip silently as blocked; (2) dedupe against live positions by company + role + source link — the dedupe index is reloaded on EVERY partial/final report, so concurrent runs on different folders resolve an already-filed posting to the live row instead of double-filing; (3) folder gates: window/lane (A4), title (if include_terms non-empty), excludes, types (A4), salary (A3), skills (body), level, arrangement, site mode/excluded sites. Any failure = skip, tallied by reason in skip_counts (reasons: salary, job type, duplicate, old, title, site, level, arrangement, skills, other). Nothing about a recorded fact (salary, deadline, types) may come from anywhere but the posting itself.

**Targeting is not a gate**: companies[]/categories[] never appear in the gate order above. They only aim discovery (direct career-page checks for listed companies, on top of the folder's normal sources; category-aimed sweeping). A candidate is never skipped, and a filed position is never pruned, for coming from an unlisted company or an unlisted category.

**Listing-level gate (run instructions)**: before any detail fetch, judge from the listing: title wording, shown posting date vs the window, region, stated type/salary/arrangement, source/site, duplicates. First visible mismatch = skip + tally, no fetch. Detail pages open only for survivors; body-level checks (skills, unstated fields) then run in full. Results pages are fetched once and screened whole. A tracked posting whose listing shows it still listed with the same deadline is re-verified from the listing — no detail fetch. High-volume sources: cap 200 candidate listings per folder per source; progress and the log state "Checked N of M listings" and mark a capped sweep as partial coverage.

**Skip lists (omit)**: before all other gates, a candidate whose company is in the folder's excluded_companies, or whose company kind is in its excluded_categories, is dropped server-side and tallied in skip_counts ("skipped company" / "skipped category"). A skip beats a target (a company/category present in both lists is skipped). Because a skipped company/category no longer matches the folder's current filters, adding one also makes that folder's still-New positions from it prunable on the next run (A5 pruning rules apply as usual).

**Pruning**: when a run executes a folder, re-check that folder's already-filed positions against the folder's CURRENT filters. A non-matching position is moved to deleted_positions (with the note that the run removed it for no longer matching the folder's filters) ONLY if its status is `new`. Any other status is never pruned. Pruned counts are reported in the run's log line and outcome. Filter edits and board filtering never prune.

## A6. Board rendering (exact behaviors)

- One implementation of each rule: salary parsing/classification, types, arrangements, dates, level, URL safety live in ONE shared module imported by client and server. (The original drifted when these were copied: "fresh" meant 0-2 days in the client and 0-6 on the server. The two named rules — Just posted 0-2d, fresh lane 0-6d — are separate on purpose.)
- Folder summary line segments, in order, each only when non-zero: "N positions", "· X closing soon" (deadline within 7 days), "· X fresh" (posted 0-2 days ago), "· X closed".
- Position order within a folder: status group order first (New, Package ready, Intro drafted, Applied, Interviewing, Offer, Rejected, Closed); WITHIN each status group, deadline ascending with no-deadline LAST (sort key fallback "9999-12-31"); ties break by company A-Z. The same deadline-first ordering applies inside every single-status filter view. Closing-soon view uses the same rule.
- Status line: always rendered, fixed height (44px). Idle text: "Last run: <Mon D, h:mm a.m./p.m.>" — date/time only, no folder name, no outcome words. Running: spinner + progress (A7).
- Folder run slot: fixed width (11rem) on the header row; Run (compact, ~51x40px) swaps in place to Running… + Cancel within the same slot at constant height. The folder switch is a standard 48px pill; all switches on the board are ONE shared component.
- Filter box: ONE bordered box containing the search bar, the status row (one horizontally scrollable row of uniform 40px cells inside its own container; no page-level horizontal scroll; active cell = filled + bold; chips do not take focus on tap), and the Advanced filters expander (collapsed summary shows the set values). Search matches company/role/location and combines with status + salary filters. Exactly ONE clear × per search bar — a standard 40px button with a 16px glyph; native browser clear suppressed (::-webkit-search-cancel-button hidden).
- Select mode control: a small, subtle "Select" button (32px tall, 12px medium dim text, transparent) sits at the top INSIDE each folder's collapsible content (and the unfiled group's) — it never renders while the folder is closed, and tapping it only enters select mode (propagation stopped; never nested inside the header's open/close button). Select mode is a compact row at the same small size, in this order: "N of M selected" count FIRST, then Select all, Delete selected, Cancel. Cancel leaves select mode and discards the selection (nothing is deleted). Delete selected confirms first, naming the count (Delete N / Cancel), then deletes with one Undo for the whole batch.
- Select mode card behavior: the WHOLE card is the select target — tapping anywhere on a card selects/deselects it, layout unchanged. Each card shows a round DOT on its LEFT side (in a left gutter the card's padding opens): 22px circle, hollow when unselected, filled with a small checkmark when selected — the filled dot is the ONLY selected indication (no tinted background, no outline, no "Selected" pill or other effects). In select mode the card's normal controls are INERT: View posting, Delete, the status picker, and the package switch render dimmed with pointer events off, so tapping them selects/deselects the card instead of acting. The one exception is expanding: a small chevron button (32px, 16px icon) placed BEFORE the Fresh pill and the status text in the card's badge row expands/collapses the card; its tap only expands (stopPropagation both ways — expanding never selects, selecting never expands). The per-card Delete outside select mode keeps the plain old style: one tap, no confirmation, 15-second Undo bar.
- Run-finished notifications: when a check run reaches a finished state, the board raises a small dismissible notice at the bottom of the board, one per run: "Check finished — Folder '<name>'" with the detail "Found N new positions" plus catch-up/updated/closed and skipped counts when any are non-zero; cancelled and failed runs notify with their own wording. Each run notifies ONCE (notified run ids persist in localStorage); a run that finished while the board was closed notifies on the next open; runs already finished on first load are seeded silently (no burst of old notices), while a run still running at load DOES notify when it finishes. Notices auto-dismiss after 30 seconds or via their Dismiss button. The server exposes the latest 10 runs (recent_runs) in its state response so every per-folder finish is seen.
- Boxes and folders at EVERY width: folders start closed (a single openId state, default none open), boxes start collapsed showing main info only and expand in place via the card header or the Details/Hide details button — identical on desktop and mobile. The folder chevron and the Details button render at all widths, and folder/card headers use a pointer cursor because they really toggle.
- Dates on cards: deadline at top-right as "Nov 18" plus relative "in 28 days" / "tomorrow" / "today"; posted age appears ONCE, in brackets in the meta line: "Posted Oct 8 (13 days ago)", "(today)", "(yesterday)".
- Scrolling: plain browser default. NO custom scroll CSS/JS of any kind (no overflow/overscroll/touch-action/scroll-restoration overrides, no JS scroll management). This is deliberate — layered scroll fixes caused device bugs; plain won.
- Closing-soon banner: tapping scrolls to and expands the position; the scroll/focus request fires ONLY on a banner tap, never on load or refresh.

## A7. Run engine (exact behaviors)

- The run's worker instructions (built server-side per run) open with: a **check-in first** — an immediate progress-only partial report per folder before any searching — then re-verify, then discover (A5 gate order), filing each find live via apply_partial_results as it is verified, with progress notes as it goes ("Checked N of M listings…" on sweeps). The final apply_check_results carries only what wasn't filed live, plus closures, updates, pruning, skip_counts, and the run's token usage figure.
- **Direct site sweep (crawl)**: every run CRAWLS the folder's listed Websites and its Companies' own career pages directly — open each site's own search/listing pages scoped to the folder's regions, apply the folder's keywords/types there, sort newest-first, and page through the results, screening listings from those pages themselves (A5 listing gate applies as usual). TRY HARD before any fallback: at least four genuinely different direct attempts per source — the site's own search URL; another way in via its home/jobs pages; an alternate or regional entry point (e.g. the .ca portal, or the URL without filter parameters); the same listings via the employer's ATS board or the site's public feed/API; working through a bot check when one is presented; and a retry later in the run after the other sources. Only when those genuinely fail does the search method apply, scoped to that one source alone, and any find from it is still verified from a posting page actually opened — reported as fallback coverage, never as a crawl. Login walls and paywalls are never defeated: a site needing an account the check does not have is logged plainly as not crawled. A web-search result or a `site:` query does NOT count as checking a listed source — search indexes miss postings; web search serves only to locate a crawl entry point, plus the wider sweep afterward when the folder's web switch allows it (when the switch is off / whitelist mode, the crawl IS the whole check). Progress notes name the crawl per source ("Crawling …", "Crawled N of M listings on …"); a site that blocks direct fetching is named in the run's log line as "Could not crawl …" with the search-based fallback marked as partial coverage for that source — never silently skipped. Every run's final log accounts for every listed site: crawled, capped, or blocked.
- **Progress block** per running run (fixed height, inside the folder's block / status area): elapsed timer ticking every second from started_at; current progress_note (fallback "Starting…"); counts filed so far (from the run's counters; when all zero the line reads "Nothing filed yet — finds appear on the board as they're found."); a progress bar; "Folder X of Y" for multi-folder runs; the token estimate (A7 below); a Cancel control for that run.
- **Token estimate**: at run start, per folder, estimate = the median of that folder's last 5 finished runs' reported token_usage; if the folder has none, the median of any folder's recent reported usages; if none exist at all, a flat base of 50,000. Shown as "≈ N tokens" and stored on the run row (token_estimate; per-folder breakdown in folder_estimates). A finished run's own reported usage is stored (token_usage) and shown on its Last run line next to the skipped summary. Estimates are always labeled with "≈".
- **Watchdog** (evaluated on getstate / run start / cancel): a `running` run with progress_updated_at NULL and now − started_at > **10 minutes** → failed, message "The check never reported in — no worker checked in within 10 minutes." A run that HAS reported and now − progress_updated_at > **20 minutes** → failed as gone quiet. A run whose worker task already reads finished while the row is still `running` → closed as failed on sight. **90 minutes** from started_at remains the absolute cap for any still-running row.
- Multi-folder `all` runs execute their folders under ONE run row with folders_total set and per-folder progress in the progress note; per-folder (profile) runs are separate rows. (Per-folder concurrency in A2.7 still applies: an `all` run and a profile run for a different folder may coexist; the per-folder guard prevents the same folder being executed twice at once.)

## A8. The register file and the daily check

- Register path pattern: `~/workspace/goals/<goal-slug>/hidden_files/position-check-register.md`. Contents: standing rules header; tracked positions table (company, role, location, salary + source label, status, gate/deadline, notes); a section per discovery source batch; a Deleted positions table; a search-profiles mirror section (kept in sync with the dashboard whenever either changes); a Closed section; a dated check log at the end. The dashboard and the register must match after every run; when they diverge, reconcile during the run and note it in the log.
- The daily check is a scheduled agent job (cron), daily 06:00 in the user's timezone, delivery to the goal's chat, owner = the goal. Its body carries: the standing rules (section 8), the profile semantics (A3-A5), live filing (file each find as verified; never file twice), and a log line that ENDS with the skipped summary + the run's estimated token usage ("≈ N tokens"). The reminder shape is section 7, exactly.
- Material changes (a strong new match, a closure, a deadline within 7 days, any pruning) also add a timeline entry to the goal.

## A9. Documents (packages)

Only for positions the user selects. Per selected position: a tailored one-page resume (PDF + DOCX) and a matching cover letter (PDF + DOCX), built ONLY from the user's verified resume facts (section 1), stored as that position's document_files and attached on its card. Tailoring = rewording into the posting's language and ordering the user's real experience for that role; gaps the posting requires are named honestly in the cover letter, never claimed. The position's status becomes Package ready when its package exists.

## A10. Default site set and US/TN gating

- **Default site set**: every folder's Websites list starts as the union of its own sites plus this default set, and NEW folders are created with the set already in their Websites list: SaskJobs.ca, Indeed, Glassdoor, LinkedIn, Canada Job Bank, Greenhouse, Lever, Ashby, Workable, SmartRecruiters, Jobvite, Workday, Wellfound, Jobicy, ZipRecruiter, Dice, Monster. USAJobs is deliberately NOT in the set (US federal jobs require citizenship; the user is on TN status — see below). Each folder's list stays fully editable afterward — removing a site from one folder changes nothing elsewhere and does not change the default for future folders. Site-mode derivation (A1) is unchanged; the set simply makes these boards listed (priority) sites everywhere. The user's own first folder set may differ — the set above is the reference implementation's.
- **US gating for a TN-status user** (the reference user's situation; adapt to the builder's own): the user will work in the US on TN status (Canadian, USMCA professional; Computer Systems Analyst category). Run instructions therefore state: US roles are eligible and filed normally UNLESS the posting requires US citizenship or a security clearance (those stay gated/flagged); postings saying "no sponsorship" are filed but flagged with the note that TN needs an employer support letter, not a sponsorship petition. US federal jobs almost always require citizenship, which is why USAJobs is excluded from the default set.

## A11. Theme colors (exact values — do not approximate)

- ALL orange in the app comes from two CSS custom properties in the theme stylesheet: `--signal` (the accent itself) and `--signal-soft` (its tinted background). Badges, urgent/closing dates, switches, the package switch, and the "Needs your call" gate labels all read these two tokens. There is NO other hardcoded orange anywhere — never add one; use the tokens.
- **Light mode**: `--signal: #e85d1f`.
- **Dark mode**: `--signal: #e07a45` and `--signal-soft: #33231b`. These dark values are deliberately muted one step down from the light accent — the vivid version (`#ff7a3d` on `#3a2417`) was too loud on dark surfaces; do not "brighten" them back, and do not mute them further either. This exact level is the settled one.
- Dark mode is defined in TWO places and BOTH must carry the exact same dark values, or the app will look different depending on how dark mode was reached: (1) the `:root.theme-dark` block (the manual Light/Dark toggle) and (2) the `prefers-color-scheme: dark` fallback block (device default on first load). Keep the two blocks identical for these tokens.
- Everything else about the look stays as section 3 states: plain system sans-serif, flat surfaces, no gradients, no heavy shadows, in both modes.

## A12. Layout structure and sizing (exact)

Page structure, top to bottom, ONE single column at every width (no multi-column grid, ever):

1. Sticky counts header: the four counts (open / fresh / closing soon / needs your call) in ~15px semibold text, and ONE Light/Dark sun/moon button, 34px.
2. The always-present status line, fixed height 44px (A6).
3. The closing-soon banner (only when something closes within 7 days).
4. The filter box (A6): search bar, ONE horizontally scrollable status row of uniform 40px cells, Advanced filters expander.
5. One block per folder, then the "Not in a folder yet" group with the same structure: a header row (name, summary line, chevron — rendered at ALL widths, Run slot fixed at 11rem wide holding a ~51x40px Run button or Running/Cancel, 48px on/off switch pill; header row vertical padding 16px), and collapsible content that renders ONLY when the folder is open: the subtle Select button (32px tall, 12px medium dim text) or the compact select-mode row, then the position boxes, then the closed-by-default "Folder settings" expander (a full-width row at least 48px tall, the folder's single Edit button beside the header).
6. Bottom rows: "Deleted positions · N" and "Check log · N entries". Run-finished notices appear at the bottom of the board (A6).

- **Container widths**: the board column is max-w-3xl (48rem) by default, lg:max-w-6xl (72rem) from the lg breakpoint, xl:max-w-7xl (80rem) from xl. The position list is a single vertical column with 12px gaps (gap-3), 16px at lg and up (lg:gap-4).
- **Position box structure**: bordered box; deadline top-right; company (bold) + role; type tags under the role; meta line (location · Posted date (age)); salary line with verified/unverified + Hourly/Yearly labels; badge row (Fresh pill or Catch-up tag, status text, compact View posting; in select mode the 32px expand chevron with a 16px icon sits BEFORE the Fresh pill); details region (status select, job description, documents, package switch, gate note) shown only when the box is expanded; the actions are one uniform row of 40px bordered buttons. In select mode the card gains a left gutter holding the 22px round dot (A6).
- **Open/close state**: ONE state pair rules all widths — openId for folders (default null = all closed) and one expanded id for cards. No breakpoint may override visibility (no lg:block-style forcing, no hiding the chevron or Details button on desktop, no cursor-default on headers). This parity is the settled behavior: desktop previously forced everything open and was unusable at ~50 positions.
- Touch/click targets: interactive rows and buttons are 40px or taller (folder settings row 48px+, switches 48px wide pills, theme button 34px is the one smaller exception, select-mode chevron 32px).
