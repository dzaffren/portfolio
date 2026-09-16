# Portfolio website

**Project type:** Web app with UI · **Status:** Shaped · **Greenfield:** yes (repo holds only `.claude/settings.json`)

## Problem

Dzafran has more than six finished projects, and they live only as GitHub repos and a line on a CV. Anyone sent there sees code with no story, so the work does not speak for him.

## Today

```
                 sends CV ──────────►  Recruiter (30-second skim)
   Dzafran                                  │ "what did he actually build?"
      │                                     ▼
      └── "here is my GitHub" ────►  Hiring manager (10-minute look)
                                            │
                                            ▼
                                   github.com/dzaffren
                                   ├── repo-1   (README, maybe)
                                   ├── repo-2   (no screenshots)
                                   ├── ...      (no "why", no "what I learned")
                                   └── repo-7+  (which ones matter? unclear)
```

The reader has to dig through raw repos to find out what was built and why. Most do not.

Who reads it: recruiters and hiring managers, weighted equally. Role: general software engineer. Success: a live link Dzafran is happy to put on the CV. No deadline.

## Slices

```
   [0] Walking skeleton
        one page, deployed, one e2e test, CI on push
              │
              ▼
   [1] Home: who I am + the best few projects
              │
      ┌───────┼────────────┐
      ▼       ▼            ▼
   [2] One  [3] Full     [4] Contact
   project  project      + CV
   page     list
```

Slice 2, 3 and 4 each work without the others. Order among them is a choice, not a dependency.

| # | Slice | What ships | Why this order |
|---|-------|-----------|----------------|
| 0 | Walking skeleton | A page with a name on it, live at a free host URL. One browser test that opens it. CI runs that test on every push and deploys on green. | Greenfield. Proves the rails (build, test, deploy) before any real content. Every later slice is then cheap. Needs stack and host decisions at `/spec`. |
| 1 | Home page | One page: a short intro, and the best few projects as cards. Each card: name, one line, a screenshot, links to repo and demo. | The smallest thing worth putting on a CV. A recruiter can skim it in 30 seconds. |
| 2 | Project page | A page per featured project: the problem, what was built, what was learned, screenshots, links. Cards on the home page link through. | This is what a hiring manager reads. Needs the project list from slice 1 to exist. |
| 3 | Full project list | A page listing every project, including the ones not featured. Hand-written list to start, not pulled from GitHub. | "More than six" means the home page cannot show them all. Drops automation on purpose. |
| 4 | Contact and CV | Email, GitHub, LinkedIn links and a downloadable CV, reachable from every page. | Closes the loop: a reader who likes the work can act on it. Small and independent. |

## Not doing

| Considered | Rejected because |
|---|---|
| Custom domain | None exists yet. A free subdomain is fine to start (user, 2026-09-16). Becomes a slice when a domain is bought. |
| Pulling project data from the GitHub API | Adds a dependency and a failure mode for a list that changes a few times a year. Hand-written first; automate the second time it hurts. |
| Blog or writing section | Not asked for. The problem is the projects, not posts. |
| Contact form | A `mailto:` link does the job with no backend and no spam handling. |
| Picking a stack here | Shape does not pick stacks. Slice 0 `/spec` does, one question at a time. |

## Open items

| ID | What | Type | Raised at | Owner | Status | Answer |
| -- | ---- | ---- | --------- | ----- | ------ | ------ |
| O1 | Who reads the site? | question | shape | user | Resolved | Recruiters and hiring managers, weighted equally. General software engineer roles. |
| O2 | Which projects are the "best few", and how many go on the home page? | question | shape | user | Open | — |
| O3 | Language, framework, host, CI for slice 0 | question | shape | user | Open | — (decided at `/spec` slice 0) |
| O4 | What written content exists today: CV text, project write-ups, screenshots? Slices 1 and 2 need it. | question | shape | user | Open | — |
| O5 | Assuming project content is hand-written in the repo, not fetched from GitHub at build time | assumption | shape | user | Open | — |
| O6 | Free subdomain means the URL on the CV changes if a domain is bought later | flag | shape | user | Accepted risk | Fine to start (user, 2026-09-16). Add a redirect when a domain exists. |
| O7 | No design system exists. `/design system` runs once before slice 1. | flag | shape | user | Open | — |
