# Payout Reconciler

Match e-wallet settlements, kiosk cash files and app orders, then flag what's missing or mismatched.

> **Status: in development.** This repo is the plan and the write-up; the interactive demo is being built next.
> **Demo preview:** https://julynard.github.io/payout-reconciler/ (becomes the live demo when it ships).

Part of [Julynard Tago-on's portfolio](https://julynardtago-on.netlify.app/#projects). Based on the payments reconciliation system I built at CoreProc (97 of 107 commits, from empty repo to delivery).
Client code is private, so this is a clean-room demo with fictional data. No client names or real data.

## What the demo will show

- [ ] Load three sample feeds: e-wallet settlement report, kiosk cash "SFTP drop", mobile app orders
- [ ] Per-row validation with a downloadable **failed-rows report**; re-uploading the same file is detected as duplicate (idempotent)
- [ ] **Run matching** over a date range (max one month): reference no. + amount + date → Matched / Unmatched / Amount mismatch
- [ ] Review queue: mark an unmatched row as matched with a remark (reviewer + reviewed-at are recorded)
- [ ] Export the reconciliation result as CSV

Every key action will have an **"In production"** note explaining how the real system does it.

## How the production system works

The real system runs on Laravel. The demo simulates these behaviours in the browser so it can stay fast and always on.

- Laravel 12 + Nova 5 as the operations UI, Octane for serving
- Feeds arrive by CSV upload, SFTP/FTP pulls and a daily mobile-API pull, all scheduled
- Large files are streamed and split 100k → 20k-row sub-chunks and processed as a Horizon `Bus::batch` on a named `imports` queue
- Raw rows stay in one table per source; a separate `recon_transactions` table holds results, so matching can be re-run and audited
- An enum with `#[Meta]` attributes registers each SFTP feed with its processor and dated filename; adding a feed is one enum case
- Larastan level 9 from day one

## Planned stack

- Vue 3 + TypeScript + Vite
- Tailwind CSS v4
- Pinia (state saved in the browser, with a **Reset demo** button)
- Vitest for the domain logic, ESLint + Prettier + `vue-tsc` in CI
- GitHub Actions → GitHub Pages (free, always on)

## Roadmap

1. Project scaffold, CI (lint, type-check, test, build) and GitHub Pages deploy
2. Domain logic with Vitest tests
3. UI with seeded data, guided steps and "In production" notes
4. Screenshot/GIF in this README and the live demo link

## Author

**Julynard Tago-on**, Laravel / PHP full-stack engineer · [Portfolio](https://julynardtago-on.netlify.app) · [GitHub](https://github.com/Julynard) · [LinkedIn](https://www.linkedin.com/in/julynard-tago-on/)

## License

[MIT](LICENSE)
