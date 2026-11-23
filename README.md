# PLE Rubric First — cloud check repo

Kaggle Physical-Engineering's Last Exam (PLE) entry: the **Rubric First**
harness, built on the `pi-opus` sample.

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
  `submission.yaml` temporarily uses `gemini/gemini-2.5-flash` — revert
  before the real entry. (First tried `gemini-3.8-flash`; Google
  returned 503 high demand on both runs 2026-11-23.) The submission repo is `my-harness` on
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
  29-task dev set — that is a separate step (needs Hugging Face approval
  for the dev data first, and model calls are billed).
- Open question: the kit's model stage may still want the google-cloud
  model proxy (`MODEL_PROXY_URL` + `MODEL_PROXY_KEY`) even though the
  harness calls Claude natively. The first run will tell us.
- Nothing here has run the dev set yet. No artifacts, no submission.
