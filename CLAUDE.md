# CLAUDE.md — blog-experiments

Public repo of small, runnable .NET labs. Each lab is how Mohamad **learns** one backend topic, and becomes a post
on [mohamaddoughman.dev](https://mohamaddoughman.dev) plus a LinkedIn post.
Owner: Mohamad (backend engineer, .NET, 5 years). This repo is **public**: anything committed is visible to everyone.

Related:
- Blog repo (private): `../Blog` (GitHub `mohamaddoughman/Blog`). Its `CLAUDE.md` covers the site, the API and Azure.
- Planning lives in Notion (see below). The roadmap order is there, not here.

## Goal

Become a senior backend engineer by learning system design and design patterns **hands-on**: read about a topic,
run a lab that shows it (and breaks it), measure, then explain it in a post. Cloud topics come later.

## The flow, per topic (follow it every time)

1. **Pick the topic:** the lowest `Order` in the Notion Content pipeline that isn't Published (or one Mohamad names).
   Set its Status to **Researching**.
2. **Reading guide** (Claude): what the topic is, the 3–5 concepts that matter, common mistakes, and 2–3 good sources
   (official docs, well-known articles/papers). Short. No lab code yet.
3. **Lab** (Claude prepares, in this repo): the smallest code that shows the answer, **broken first, then fixed**, with
   a README from `labs/_template/README.md`. Status: **Experimenting**.
4. **Mohamad runs it**, changes things ("Try changing" section), asks questions; Claude explains what he sees.
   Fill the README's **Results** with his real numbers (machine + date).
5. **Post:** Mohamad writes it **in his own words**. Claude proposes an outline, checks technical accuracy, and polishes
   the language, but does not write the post for him (credibility on LinkedIn and in interviews). Status: **Drafting → Ready**.
6. **Publish** (in the Blog repo): the post links to its lab folder here. Then a short LinkedIn post (the finding, one
   number, the link). Update Notion: Status **Published**, Blog URL, LinkedIn posted. Add the lab to the table in `README.md`.

## How we work

- **One step at a time.** Explain the step, do only that step, wait for Mohamad's go-ahead.
- **Explain the why**, not just the what. Plain words; define terms the first time.
- Learning comes first: prefer code that makes the concept **visible** over clever or production-heavy code.
- Never invent facts about companies (case studies): only what their own engineering blogs, papers or talks say, with
  links. If unsure, say so.
- Small PRs into `main` from short-lived branches (`lab/NN-topic`).
- Commit messages: plain, explain the why.

## Lab conventions

- Folder: `labs/NN-topic/` where **NN = the roadmap `Order`** (e.g. `labs/01-database-indexes`, `labs/03-race-conditions`).
- Each lab is self-contained: its own `README.md`, `src/` (a console or minimal API project), optional `load/`
  (k6 scripts) or BenchmarkDotNet project.
- README sections, always: **Question → Run → What to watch → Try changing → Results → Learn more** (template in `labs/_template/`).
- Show the **wrong way first** (the race, the slow query, the stampede), then the fix, so the difference is measured.
- Keep dependencies minimal and pinned. .NET 10 (`global.json`), strict build (`Directory.Build.props`: warnings are errors).
- Data: generated in the lab (seed scripts / Bogus), never real data.

## Infrastructure (local only)

`docker compose up -d` from the repo root starts:

| Service | Image | Port (localhost only) |
| --- | --- | --- |
| SQL Server 2022 | `mcr.microsoft.com/mssql/server:2022-latest` | `14330` |
| Redis 8.6 | `redis:8.6` | `63790` |

- Settings: `.env` (copied from `.env.example`, git-ignored). Labs read `ConnectionStrings__Sql` and `ConnectionStrings__Redis`.
- The images are already on Mohamad's machine; **his connection to mcr.microsoft.com is very slow**, so don't add
  images from mcr without asking. Docker Hub images are fine. If a lab needs another service (Kafka, RabbitMQ), add
  it to `docker-compose.yml` in that lab's PR, as a profile if it's heavy.
- Load tests: k6 (installed, v2.3.0): `"/c/Program Files/k6/k6.exe" run load/<script>.js` on Windows.

## Safety (public repo)

- Never commit secrets, real connection strings, or anything from the blog's Azure setup. Only local throwaway containers.
- The password in `.env.example` is a local-container example, not a secret; keep it that way.

## Notion (planning)

Workspace: [Blog](https://app.notion.com/p/40da246b161e80fc93a7df510593f4d9). Start page: [Start here](https://app.notion.com/p/412a246b161e8167903ac92368ead1c0).

- [Content pipeline](https://app.notion.com/p/1cf6bb60a5fa4128b625e4edb44d33cc) = the learning roadmap (67 items).
  Fields: Status (Idea → Researching → Experimenting → Drafting → Ready → Published), **Order** (global learning order 1–67),
  **Stage** (1. Data & concurrency … 7. Capstone case studies, then Later), Series + Part, Level, Pillar (Lab series /
  Case study), Question, Experiment, Target date, Launch post, Blog URL, LinkedIn posted.
  Views: 🧭 Roadmap (by stage, in order), Board, By level, Next 7 days, Launch posts.
- [Topics](https://app.notion.com/p/94811462a48b4c72b384ca583734abb0) = decisions and technical work (view 📌 Pending).
- Update Notion as the status changes (Researching → … → Published). Keep it light: status, dates, URLs.

## Roadmap (stages, in order)

1. Data & concurrency (#1–11): indexes, isolation levels, race conditions, Redis vs DB lock, optimistic vs pessimistic,
   idempotency, lock expiry, seat holds, multi-seat + double clicks, idempotency keys, Stripe case study
2. Data access & performance (#12–17)
3. Caching (#18–22)
4. APIs & resilience (#23–33)
5. Design & architecture (#34–38)
6. Messaging & distributed systems (#39–53)
7. Capstone case studies (#54–58)

The exact list and order are in Notion (🧭 Roadmap view). This list is only a summary; Notion wins.

## Launch (blog)

The blog launches **Sat Nov 28, 2026** with 3 posts. Suggested launch posts: #1 Database indexes, #3 Race conditions,
#4 Redis lock vs DB lock (not decided yet; see the Topics item "Choose the 3 launch posts").

## Current status (updated Nov 8, 2026)

- Repo scaffolded (README, docker-compose with SQL Server + Redis, template, strict build). Containers verified healthy.
- **Next: #1 Database indexes and query plans** → start with the reading guide.
