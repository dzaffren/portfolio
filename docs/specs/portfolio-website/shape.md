# Portfolio website

**Project type:** Web app with UI · **Status:** Shaped

Greenfield build. The repo holds only `.claude/settings.json`. Slice 0 is a
walking skeleton and needs stack decisions at `/spec`. No design system
exists yet; `/design system` runs once before the first UI slice.

## Problem

A freelance client sizing up Dzafran today has only GitHub and LinkedIn to go
on: a list of repos with no story, no visuals, and no clear way to hire. Nothing
online shows that Dzafran can build a polished, moving interface, which is the
kind of work the site should win.

Secondary goal, stated by the owner: learning 3D and motion on the web is the
point. The site is the excuse. When the two goals conflict on a phone, the
client experience wins and the 3D degrades to something simpler.

## Today

```
  freelance client                     what they see
  ───────────────                      ────────────
  "can this person build it?"
        │
        ├──► github.com/dzaffren ────► repo list, README fragments,
        │                              no screenshots, no "hire me"
        │
        └──► linkedin.com/... ───────► job history, no running work
                                                │
                                                ▼
                                       client guesses, or moves on
```

One sentence: the pain is that proof of delivery and a way to get in touch are
in two places, and neither one looks like the work.

## Slices

```
  0 walking skeleton ──┬──► 1 project gallery ──┬──► 2 project story
  (deployed, one       │    (3-6 hand-written   │    (problem / built /
   motion element,     │     cards, GitHub +    │     result, screenshots)
   email + socials)    │     live links)        │
                       │                        └──► 4 GitHub facts
                       │                             (stars, language,
                       │                              last push, at build)
                       └──► 3 interactive hero
                            (scene reacts to
                             pointer and scroll)
```

Arrows mean "must exist first". 3 and 4 are independent of each other.

| # | Slice | What ships | Why this order |
|---|-------|-----------|----------------|
| 0 | Walking skeleton | One deployed page at a real URL: name, one-line pitch, email link, social links, and one live 3D or motion element with a static fallback when the device cannot run it or asks for reduced motion. One browser e2e test walks it. CI runs the test and a page-weight check on every push. | Proves every rail: build, 3D rendering, the phone fallback, deploy, CI. A client landing here already knows who Dzafran is and how to reach them. |
| 1 | Project gallery | A client scrolls through 3 to 6 project cards written by hand in the repo: title, one-paragraph blurb, tech list, one image, links to GitHub and to a live demo where one exists. Cards animate in on scroll. | This is the value. Without it the site is a business card. Hand-written content ships without any API work. |
| 2 | Project story | A client opens a project and reads the case: the problem, what was built, the result, with screenshots. Back to the gallery with a page transition. | Freelance clients buy outcomes, not repos. Comes after the gallery because a card is the door to the story. |
| 3 | Interactive hero | The slice-0 scene becomes a scene that responds to pointer movement and scroll. Still falls back on phones and with reduced motion. | This is the learning goal. Needs only slice 0. Sits after 1 and 2 because a client needs the content before the spectacle. |
| 4 | GitHub facts | Each card also shows stars, main language, and last push, read from the GitHub API at build time. A failed fetch keeps the last good values. | Lowest value for a client, so last. Depends on the gallery. Build-time keeps tokens out of the browser and avoids rate limits. |

Slice test, checked per slice: each cuts UI, content and deploy; each has one
e2e path; each is worth shipping alone; none has more than five scenarios.

Cross-cutting rule carried by every slice, not a slice of its own: respect the
user's reduced-motion setting, keep a page-weight budget in CI, and never block
first paint on a 3D asset. This is the "client experience wins" decision.

## Not doing

| Considered | Rejected because |
|---|---|
| Contact form | Owner chose email plus social links. A form needs a backend or a form service. Can be a later slice if leads need capturing. |
| Blog or writing section | Not part of the problem. Adds a content burden the owner has not asked for. |
| A CMS for projects | 3 to 6 hand-written entries in the repo is cheaper and versioned. |
| Pulling everything from GitHub | Owner chose hand-written story plus GitHub facts. Repo descriptions do not sell work. |
| A fully 3D, WebGL-only site | Conflicts with "client experience wins". 3D is a layer that degrades. |
| Analytics | Success is defined as learning, not traffic. Add when the owner wants to know who visits. |

## Open items

| ID | What | Type | Raised at | Owner | Status | Answer |
| -- | ---- | ---- | --------- | ----- | ------ | ------ |
| O1 | Which 3 to 6 projects go in, and which have screenshots or a live demo? | question | shape | user | Open | — |
| O2 | Is there a domain name already, or does slice 0 ship on a host subdomain first? | question | shape | user | Open | — |
| O3 | Assuming a static site with no backend: email link, hand-written content, GitHub read at build time. | assumption | shape | user | Open | — |
| O4 | A 3D scene can load and run acceptably on a mid-range phone without hurting first paint. If not, the fallback is the phone experience. | unproven | shape | poc | Open | — |
| O5 | Writing the story, blurbs and screenshots for each project is the owner's work and will likely take longer than the code. Slices 1 and 2 wait on it. | flag | shape | user | Open | — |
| O6 | Any reference sites for the look and the kind of motion wanted? Feeds `/design system`. | question | shape | user | Open | — |
