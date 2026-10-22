# Developer Job Tracker — for Muse

A private job-search tracker that lives inside your own Muse: a board of job positions grouped by search folders, automatic daily position checks, and tailored resume/cover-letter packages for the positions you pick.

This repository does **not** contain a runnable app. Muse apps (artifacts) are private to each person's Muse and can't be handed over as a working copy. What this repo contains is the complete build spec: paste it to your Muse and it builds the tracker for you, in your Muse, with your data.

## How to use it

1. Open `SETUP.md` and copy the whole file.
2. Paste it into a chat with your Muse.
3. Answer the setup questions it asks (your name, email, timezone, resume facts, and the folders you want).
4. Your Muse builds the tracker and, if you want, a daily scheduled check that files new positions every morning.

## What's inside

- `SETUP.md` — the full build spec: data model, board layout, folder/search settings, run behavior (live filing, progress, per-folder runs, token estimates), the listing-level speed rules, the daily check with fresh + catch-up lanes, and the standing rules (verified salary only, packages only for positions you select, delete = one specific posting). Its appendix is the exact technical specification — tables, fields, actions, constants, and derived rules — so a rebuild matches the original's behavior, not just its idea.

## Privacy

The spec contains placeholders only — no one's real positions, companies, salaries, or contact details. Whatever you track stays in your own Muse.

## Sharing

This repo is private. The owner can invite you under **Settings → Collaborators** in this repository. Please don't redistribute it further without asking.
