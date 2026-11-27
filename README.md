# PLE Rubric First — cloud check repo

Kaggle Physical-Engineering's Last Exam (PLE) entry: the **Rubric First**
harness, built on the `pi-opus` sample.

**Kit check PASSED — GitHub Actions run 45403834792, free Gemini (gemini-3.5-flash-lite), 24 Nov 2026. Hugging Face dataset access APPROVED 26 Nov 2026.**

## Hackathon Requirements Met

Kaggle Buildathon — Physical-Engineering's Last Exam. Deadline 5 Jan 2027 23:59 UTC. Prizes $400,000 cash (1st $250,000, 2nd $100,000, 3rd $50,000); Stage 2 winners are the top 3 by mean score on the performance-cost Pareto frontier, and a mean of at least 0.138 is needed to qualify. All official entries go via Kaggle Writeups. Each requirement, with proof:

- [x] Harness built — Rubric First: the agent writes a licensed reviewer's checklist, solves against it, then self-grades with evidence per item. Ablation switch `RUBRIC_FIRST=0`.
- [x] `kit validate` PASSED 22 Nov 2026 ("ok: pi-opus, google-cloud/claude-opus-5-5; egress none").
- [x] Kit check PASSED 24 Nov 2026 via the free-Gemini smoke check in GitHub Actions (run 45403834792, gemini-3.5-flash-lite, success after 8 failures).
- [x] Entry repo created: https://github.com/skymiller1509-blip/ple-rubic-first — this repo is the CHECK repo, not the entry. The entry is `my-harness` on its own.
- [x] Hugging Face dataset access APPROVED 26 Nov 2026 (`google/physical-engineerings-last-exam`, dev set 29 tasks).
- [ ] Dev-set run (29 tasks, Rubric First ON and OFF) — waits on the HF_TOKEN repo secret (Eddie's step: add it on GitHub, like GEMINI_API_KEY).
- [ ] Failure analysis from REAL failures — 25 of the 100 writeup points, the biggest single section. Only possible after the dev-set run.
- [ ] Kaggle Writeup submitted before 5 Jan 2027 23:59 UTC.

Failure-proofing: there is no demo video here — the writeup is the submission. The dev-set run goes through GitHub Actions (local Docker is a hard blocker), and the check only runs by hand (`workflow_dispatch`), so it cannot burn API credits by accident.

## Judging Highlights

- **Innovation:** Rubric First — the agent writes a licensed reviewer's checklist, solves against it, then self-grades with evidence per item. The writeup is scored out of 100, and the failure analysis from real failures is the biggest single section (25 points).
- **Technical execution:** built on the `pi-opus` sample; real entry model `google-cloud/claude-opus-5-5`; `kit validate` and the kit check both passed.
- **Impact:** PLE is a real benchmark (85 tasks, 6-hour agent limit, graded 0–1 in an isolated verifier container) — the dev set is 29 real tasks, not a toy.
- **Completeness:** check repo + harness + writeup outline all exist; the dev-set run and the writeup are the two pieces left; honest limits stated below.

This repo exists for one reason: the full `kit check` needs Docker, and the
builder has no desktop with Docker. GitHub-hosted Ubuntu runners ship Docker
preinstalled, so the check runs here in the cloud instead.

## Layout

- `ple-kit/` — the PLE Competition Kit (`google-research/ple`, Apache-2.0).
  Used only to run the checks; not part of the submission.
- `my-harness/` — the harness itself: the `pi-opus` sample plus the Rubric
  First layer (the agent writes a licensed reviewer's checklist, solves
  against it, then self-grades with evidence). Real entry model:
  `google-cloud/claude-opus-5-5`. For the free smoke check only,
  `submission.yaml` temporarily uses `gemini/gemini-3.5-flash-lite` —
  revert before the real entry. (3.8-flash 503'd on high demand in
  runs 1-2 and again in the 16:15 WAT retry; 2.5-flash is retired,
  HTTP 404; 3.6-flash 503'd too in the 16:33 WAT run. 3.5-flash-lite
  is the last free Flash variant.) The submission repo is `my-harness` on
  its own; this repo is only the check vehicle.
- `.github/workflows/kit-check.yml` — runs `kit check` with a paid
  Anthropic key. `.github/workflows/kit-check-gemini.yml` — runs it with
  a free Gemini key instead (smoke check only, see the file header).

## How to run the check

1. Add an Anthropic API key as a repo secret named `ANTHROPIC_API_KEY`
   (Settings > Secrets and variables > Actions). The key holder adds it
   themselves; it is never sent in chat.
2. Actions tab > `kit-check` > Run workflow. It only runs by hand
   (`workflow_dispatch`), never on push, so it cannot burn API credits
   by accident.

## Honest limits

- `kit check` runs smoke/MCP/cancel tasks only. It does NOT run the full
  29-task dev set — that is a separate step. Hugging Face approval is DONE
  (26 Nov 2026); the dev-set run now waits on the HF_TOKEN repo secret,
  and model calls are billed.
- Open question: the kit's model stage may still want the google-cloud
  model proxy (`MODEL_PROXY_URL` + `MODEL_PROXY_KEY`) even though the
  harness calls Claude natively. The first run will tell us.
- Nothing here has run the dev set yet. No artifacts, no submission.
