# Portfolio website

**Project type:** Web app with UI · **Status:** Shaped

Greenfield build. The repo holds only `.claude/settings.json`. Slice 0 is a
walking skeleton and needs stack decisions at `/spec`. No design system
exists yet; `/design system` runs once before the first UI slice.

Reshaped 2026-09-16 after the owner added two things: the site carries the
whole CV (education, work, projects, certifications), and the site is a game,
a controllable avatar exploring a 3D third-person world.

## Problem

A freelance client sizing up Dzafran today has only GitHub and LinkedIn to go
on: a list of repos with no story, no visuals, and no clear way to hire. Nothing
online shows that Dzafran can build a polished, moving interface, which is the
kind of work the site should win.

Secondary goal, stated by the owner: learning 3D and motion on the web is the
point. The site is the excuse.

## Decisions that shape everything

| Decision | Owner's answer | What it costs |
|---|---|---|
| The world | 3D, third-person, free movement | The most expensive of the three options offered. Every zone is a place to model, light and test. |
| On a phone | The game, with touch controls | An on-screen joystick and a phone-safe scene. This is the biggest technical risk (O4). |
| In a hurry | Always a shortcut | A menu or map jumps to any section. The walk is optional. This is what keeps a busy client from leaving. |
| Content | Whole CV: education, work, projects, certifications, plus contact | The owner writes all of it. Content, not code, is the long pole (O5). |

Plain statement: this is two ideas fused. One is a CV site. The other is a
small 3D game. The game is the wrapper and the reason to build; the CV is what
a client actually needs. The slices below keep the CV readable without the
game at every step, so the site is useful even when the game is half done.

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

One sentence: proof of delivery and a way to get in touch live in two places,
and neither one looks like the work.

## The world, as a picture

```
   ┌────────────────────────────────────────────────────────┐
   │  overlay (plain HTML, always on top)                    │
   │  [ Projects ] [ Work ] [ Education ] [ Certs ] [ Email ] │
   ├────────────────────────────────────────────────────────┤
   │                                                         │
   │        ┌──────────┐              ┌──────────┐           │
   │        │ Projects │              │   Work   │           │
   │        │  plaza   │              │ building │           │
   │        └────┬─────┘              └────┬─────┘           │
   │             │        (o)  avatar      │                 │
   │             └─────────┬───────────────┘                 │
   │                       │                                 │
   │        ┌──────────┐   │          ┌──────────┐           │
   │        │Education │───┴──────────│  Certs   │           │
   │        │  hall    │              │  shelf   │           │
   │        └──────────┘              └──────────┘           │
   │                                                         │
   │  desktop: WASD / arrows      phone: on-screen joystick  │
   └────────────────────────────────────────────────────────┘
```

Walking into a zone opens that zone's panel. Clicking the overlay opens the
same panel and moves the avatar there. The panels are ordinary HTML, so the
content is readable by a screen reader, a search engine, and a client whose
device cannot run the 3D.

## Slices

```
  0 walking skeleton ──┬──► 1 projects zone ──┬──► 3 project story
  (avatar walks in an  │    (walk to plaza,   │    (open a project,
   empty world, keys + │     read 3-6 hand-   │     read problem /
   touch, overlay with │     written cards,   │     built / result)
   name + email)       │     click links)     │
                       │                      └──► 4 GitHub facts
                       │                           (stars, language,
                       │                            last push, at build)
                       └──► 2 CV zone
                            (work, education,
                             certifications)
```

Arrows mean "must exist first". 1 and 2 are independent. 3 and 4 are
independent.

| # | Slice | What ships | Why this order |
|---|-------|-----------|----------------|
| 0 | Walking skeleton | One deployed page at a real URL. A 3D avatar stands on a flat ground and walks with keys on desktop and an on-screen joystick on a phone. A plain HTML overlay shows name, one-line pitch, email link and social links. Reduced-motion or a device that fails to start the 3D gets the overlay alone. One browser e2e test walks it. CI runs the test and a page-weight check on every push. | Proves every rail at once: 3D rendering, character control, touch input, the no-3D fallback, deploy, CI. Everything risky in this plan lives here, on purpose. A client landing here still knows who Dzafran is and how to make contact. |
| 1 | Projects zone | A client walks to the projects plaza, or picks Projects from the overlay, and reads 3 to 6 hand-written project cards: title, blurb, tech list, one image, GitHub and live-demo links. | Projects are the proof a freelance client wants. Independent of slice 2, so it can go first. |
| 2 | CV zone | A client walks to the work, education and certifications spots, or picks them from the overlay, and reads each as a timeline or list. Hand-written content in the repo. | The rest of the CV. Same panel mechanism as slice 1, different content. Could ship before 1 if the project content is late (O5). |
| 3 | Project story | A client opens a project card and reads the case: the problem, what was built, the result, with screenshots. | Clients buy outcomes, not repos. Needs the cards from slice 1. |
| 4 | GitHub facts | Each project card also shows stars, main language and last push, read from the GitHub API at build time. A failed fetch keeps the last good values. | Lowest value for a client, so last. Build-time keeps tokens out of the browser and avoids rate limits. |

Slice test, checked per slice: each cuts UI, content and deploy; each has one
e2e path; each is worth shipping alone; none has more than five scenarios.

Rules every slice carries, not slices of their own:

- Every piece of content is plain HTML in a panel, reachable from the overlay
  without walking. The 3D is a layer over the CV, never the only way in.
- Respect the user's reduced-motion setting: no free-moving camera, the
  overlay alone.
- A page-weight budget in CI. The avatar and world never block first paint;
  the overlay renders first.
- World polish (avatar idle and walk animation, ambient motion, lighting)
  arrives inside the zone slices, not as a separate slice.

## Not doing

| Considered | Rejected because |
|---|---|
| 2D top-down world | Owner chose 3D third-person. Kept here as the fallback plan if O4 fails badly. |
| 3D on rails (avatar follows the scroll) | Owner wants free movement. Also a fallback if O4 fails. |
| Walking as the only way to reach content | Owner chose "always a shortcut". A busy client would leave. |
| Contact form | Owner chose email plus social links. A form needs a backend or a form service. |
| Blog or writing section | Not part of the problem. |
| A CMS for content | A handful of hand-written entries in the repo is cheaper and versioned. |
| Pulling everything from GitHub | Owner chose hand-written story plus GitHub facts. Repo descriptions do not sell work. |
| Analytics | Success is defined as learning, not traffic. |
| Collectibles, scores, achievements | Fun, but nothing a client needs. Can be a later slice once the world exists. |
| Multiplayer, chat, an NPC that talks | Needs a backend and moderation. Out. |

## Open items

| ID | What | Type | Raised at | Owner | Status | Answer |
| -- | ---- | ---- | --------- | ----- | ------ | ------ |
| O1 | Which 3 to 6 projects go in, and which have screenshots or a live demo? | question | shape | user | Open | — |
| O2 | Is there a domain name already, or does slice 0 ship on a host subdomain first? | question | shape | user | Open | — |
| O3 | Assuming a static site with no backend: email link, hand-written content, GitHub read at build time. | assumption | shape | user | Open | — |
| O4 | A 3D third-person world with a controllable avatar loads and runs acceptably on a mid-range phone with touch controls, without hurting first paint. If it does not, the choices are: simpler world on phone, or the 2D or on-rails options above. | unproven | shape | poc | Open | — |
| O5 | Writing the CV content (work, education, certifications, project blurbs, screenshots) is the owner's work and will take longer than the code. Slices 1 to 3 wait on it. | flag | shape | user | Open | — |
| O6 | Any reference sites or games for the look, the avatar style and the kind of motion wanted? Feeds `/design system`. | question | shape | user | Open | — |
| O7 | Where do the avatar and world models come from: a free asset pack, a bought pack, or made by the owner? Decides art style and licence, and how much modelling work is in each zone. | question | shape | user | Open | — |
| O8 | A 3D free-movement world with touch controls is much more work than the earlier motion-only plan. I do not have a number; the slice-0 spec and the O4 spike will give one. The owner should weigh this against the learning goal before `/spec`. | flag | shape | user | Open | — |
| O9 | Assuming every content panel is real HTML and the overlay is the accessibility path, so no separate "plain CV page" is built. | assumption | shape | user | Open | — |
