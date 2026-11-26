# IELTS Quest: Kingdom of Words

IELTS practice in the browser for all four skills. A daily plan picks today's work, timed sets check the answers, and every session is stamped into a tracker. Progress is stored in `localStorage`, so there is no sign-up.

![Next.js 16](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)
![React 19](https://img.shields.io/badge/React-19-087EA4?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)
![Tailwind CSS 4](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?logo=tailwindcss&logoColor=white)

## Repository layout

The application lives in [`speakup-ielts-source`](https://github.com/FarisAbrar29/speakup-IELTS), added here as a git submodule and pinned to a specific commit. Clone with submodules to get the code:

```bash
git clone --recurse-submodules https://github.com/FarisAbrar29/IELTS-QUEST.git
cd IELTS-QUEST/speakup-ielts-source
npm install
npm run dev
```

Open http://localhost:3000. The app needs no environment variables and no database.

If you cloned without submodules, run `git submodule update --init --recursive` inside the project first.

## What it does

- **Daily plan.** You pick a target band, an exam date and a daily time budget. The app builds a plan of focused sessions, with a weekly strip and a streak that counts real work.
- **Four skill banks.** Listening, Reading and Writing entries are generated from the question banks, so the catalogue cannot promise a set that does not exist. Speaking runs on its own drills: Part 1 sprint, Part 2 long turn, Part 3 discussion, flashcards, topic wheel and a band self-assessment.
- **Two modes.** Study mode allows hints and explanations. Exam mode runs the strict timer with no help.
- **Mock test.** Listening, Reading and Writing run in order under exam conditions, each with its own timer. Speaking stays as practice drills: there is no chained timed simulation for it yet, and the screen says so.
- **Vocabulary.** 304 words in 15 themed decks, each with an English definition, an Indonesian gloss, collocations and an example sentence. Words come back on a review schedule.
- **Tracker.** A calendar heatmap, a streak counter, session history and a per-day breakdown of minutes and answers.
- **Honest band estimates.** Reading, Listening and Writing are scored locally. Writing shows the word counts, connectors and repeated words behind its band. Every number is labelled as a practice estimate.

## Practice bank

| Skill | Content |
| --- | --- |
| Listening | 8 sets, 10 questions each, with a script read aloud by the browser |
| Reading | 8 passages, 10 or 12 questions each, covering the common IELTS question types |
| Writing | 1 Task 1 report with chart data, 1 Task 2 essay, both with model answers |
| Speaking | 7 drills across 15 topics |
| Vocabulary | 304 words across 15 decks |
| Placement | 8 quick questions that suggest a starting point |

## Stack

- Next.js 16 App Router and React 19, TypeScript in strict mode
- Tailwind CSS v4 with shadcn/ui primitives on Radix
- zustand with `persist` for progress, settings and the focus timer
- Recharts for progress charts, GSAP for entrance motion
- `three` and `@react-three/fiber` for the optional 3D scene on Home, which can be switched off in Settings
- Web Speech API for speaking transcription and listening audio, MediaRecorder for voice notes

## Routes

The app uses hash routes, so deep links work without server rewrite rules.

| Route | Screen |
| --- | --- |
| `#/` | Home: today's plan, skill cards, daily challenge |
| `#/practice` | Practice catalogue by skill |
| `#/practice/bank` | Question bank, browsable by type |
| `#/practice/vocabulary` | Vocabulary decks and review |
| `#/practice/start` | Beginner on-ramp: test format, skills check, first week |
| `#/practice/<skill>` | One skill's sets |
| `#/practice/<skill>/<setId>` | The player. Add `?mode=exam` or `?mock=1` for exam conditions |
| `#/warmup`, `#/new-vocab` | Short vocabulary sessions from the daily plan |
| `#/mock-test` | Mock test |
| `#/progress` | Tracker, heatmap, session history |
| `#/settings` | Target band, text size, audio accent, Indonesian help, 3D scene, reduced motion |

## Source layout

Inside `speakup-ielts-source`:

```
src/
  app/            Next.js entry, layout, global tokens
  views/          Top-level screens (home, practice, mock test, progress, settings)
  skills/         Reading, Listening and Writing players
  components/     App chrome, plan widgets, tracker, vocabulary, shadcn/ui
  data/           Question banks as JSON, plus typed loaders
  lib/            Routing, scoring, plan scheduling, catalogue, economy
  store/          zustand stores (progress, settings, session timer)
  hooks/          Hash routing, speech recognition, recorder, browser capabilities
tests/            node:test files for the pure logic
docs/             Design concept and UX audit
```

Adding content means adding JSON under `src/data/<skill>/`, exporting it from that folder's `index.ts`, and running `npm run validate:data`. The Practice catalogue, mock test list and Home recommendations pick it up automatically.

## Checks

```bash
npx tsc --noEmit     # next build ignores type errors, this does not
npm run lint
npm test
npx next build
```

## Notes

- Band scores are practice estimates from local scoring rules, not an official IELTS result.
- Speaking transcription needs the Web Speech API. Chrome and Edge support it; Firefox may not.
- The Prisma schema in the app is not used at runtime. All learner data stays in the browser.
- Interface copy is English. Indonesian glosses and help text appear only when enabled in Settings.

The visual direction is written up in [`speakup-ielts-source/docs/design-concept.md`](https://github.com/FarisAbrar29/speakup-IELTS/blob/main/docs/design-concept.md): a field journal made of paper, ink and one highlighter colour.
