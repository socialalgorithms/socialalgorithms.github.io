---
layout: default
---
# Assignment 3: Scattergories, Competition, and Diversity

**Due Date: Monday 3/9 11:59p**

## Overview

In this assignment, you'll build a small empirical replication of key ideas in *Competition and Diversity in Generative AI* (Raghavan, 2024), using LLMs to play Scattergories.

You will study diversity in two ways:

1. **Within-model diversity**: how sampling controls (especially temperature, and where available top-k) change output distributions
2. **Across-model diversity**: how competition between different models changes scores and answer overlap

The assignment builds in stages from a single prompt and one model, to self-play, to cross-model competition.

You should read `raghavan.pdf` before you begin.

## Learning goals

By the end of this assignment, you should be able to:

- Design prompts for controlled generation experiments
- Measure how decoding choices change empirical output distributions
- Compare model-implied probabilities to empirical frequencies (when API support exists)
- Implement repeated-game simulation and scoring for a simple competitive environment
- Evaluate whether model heterogeneity improves outcomes under competition

## Required Setup

You must use:

- One OpenAI model endpoint (for judging)
- Ollama with at least three different local player models

You are also provided a question bank for later parts:

- `scattergories_questions.csv`

### API note (important)

As of February 2026:

- OpenAI Chat Completions supports token log probabilities (`logprobs`, `top_logprobs`) for supported models.
- Ollama model behavior is controlled locally (for example temperature and top-k), but token logprob exposure depends on your exact tooling path.

So for this assignment, OpenAI is required for the judge, and Ollama is required for player generation.

### Install Ollama (required)

Mac:

1. Download installer: `https://ollama.com/download/mac`
2. Install and open the Ollama app once.
3. In Terminal, verify with `ollama --version`.

Windows:

1. Download installer: `https://ollama.com/download/windows`
2. Install and open Ollama once.
3. In PowerShell, verify with `ollama --version`.

Linux:

1. Run `curl -fsSL https://ollama.com/install.sh | sh`
2. Start server with `ollama serve` (if it is not already running as a service).
3. Verify with `ollama --version`.

Sanity check:

1. Run `ollama run llama3.2:1b \"Return only: OK\"`
2. You should get `OK`.

### Pull local player models (required)

Minimum requirement:

- Choose at least 3 different local models.
- If your hardware supports it, include at least 1 model in the 7B-8B range.
- If it does not, use the fallback list below and document the constraint in your README.

Standard track (most 2026 laptops with enough RAM/VRAM):

- `ollama pull qwen2.5:7b`
- `ollama pull mistral:7b`
- `ollama pull llama3.2:3b`
- `ollama pull gemma2:2b`

Fallback track (resource-constrained laptops):

- `ollama pull llama3.2:1b`
- `ollama pull qwen2.5:1.5b`
- `ollama pull qwen2.5:3b`
- `ollama pull gemma2:2b`

Recommended set for this assignment (target 4 models):

- Prefer the Standard track if it runs stably on your machine.
- Otherwise use the Fallback track.

If your machine is very resource-constrained, prioritize:

- `llama3.2:1b`
- `qwen2.5:0.5b`

If your machine is stronger, optional larger models include:

- `qwen2.5:7b`
- `llama3.1:8b`
- `mistral:7b`
- `gemma2:9b`

### Player/Judge scaffold (required)

You must implement two separate roles in code:

1. `player_model`: generates candidate Scattergories answers
2. `judge_model`: evaluates whether a candidate answer is valid for a given `(letter, category)`

Rules:

- The judge must be an **OpenAI model endpoint** from the GPT family (recommended: `gpt-5-mini`; optional: `gpt-5`).
- The judge should be run deterministically (`temperature = 0` if supported).
- The judge must return exactly one token/word: `yes` or `no`.
- The judge prompt must include both the `letter` and `category` so letter constraints are checked explicitly.
- The judge should not see player metadata (model family, temperature, provider, or whether output came from self-play vs cross-play).
- You must cache judge decisions by normalized key `(letter, category, answer_normalized)` to reduce repeated API calls.

Validation policy:

- For closed tasks with finite known sets (like days of week), use explicit ground-truth sets.
- For open-ended Scattergories tasks, use the judge model, not hand-written full answer lists.

Recommended player compute strategy:

- Use local Ollama models for all high-volume player sampling.
- Reserve paid OpenAI API calls for the judge.

### Cost-control requirements

- Set hard spend limits on API accounts before running large sweeps.
- Run a small pilot first (for example 5 questions, 10 rounds) to validate prompts and parsing.
- Cache both player outputs and judge outputs whenever possible.
- Record total API calls and estimated token usage in your README.

### Starter scaffold (provided)

Starter code is provided in:

- `starter/assignment3_starter.py`
- `starter/prompts.py`
- `starter/README.md`
- `starter/requirements.txt`
- `starter/prompts_template.md`

Quick start:

1. `python3 -m venv .venv && source .venv/bin/activate`
2. `pip install -r starter/requirements.txt`
3. Set API key: `export OPENAI_API_KEY=...`
4. Run a calibration baseline:
   - `python3 starter/assignment3_starter.py calibrate --model llama3.2:3b --task day --samples 1000 --temperatures 0.0,0.2,0.4,0.6,0.8,1.0 --top-k 7 --outdir outputs`
5. Run one self-play baseline:
   - `python3 starter/assignment3_starter.py self-play --model qwen2.5:3b --questions-csv scattergories_questions.csv --rounds 50 --temperature 0.7 --judge-model gpt-5-mini --outdir outputs`
6. Run pairwise cross-play across at least 3 models:
   - `python3 starter/assignment3_starter.py pairwise --models qwen2.5:3b,llama3.2:3b,gemma2:2b --questions-csv scattergories_questions.csv --rounds 30 --temperature 0.7 --judge-model gpt-5-mini --outdir outputs`

Important:

- This scaffold is intentionally incomplete.
- You are expected to improve prompts, run additional sweeps, add analysis/plots, and audit judge quality.
- Treat starter outputs as a baseline, not a final submission.

## Part I: Single-Model Calibration

### I.1 Warm-up: one simple Scattergories question

Use a single **local Ollama player model** first.

Task:

1. Use the question: **"Name a day of the week."**
2. Write a prompt that forces the model to output exactly one short token-like answer from a fixed set (for example `Mon Tue Wed Thu Fri Sat Sun`).
3. Verify your answer set with the tokenizer for your chosen model. If your labels are not single-token for that model, switch to a guaranteed single-token coding scheme (for example `1`..`7` with a fixed mapping).
4. Sample **1000 generations per temperature** over a grid of temperatures.
5. Plot histograms of answer frequencies at each temperature.
6. Your goal is to make the distribution as close to uniform as possible over 7 choices.

Report at least these metrics:

- Empirical entropy
- KL divergence to uniform
- Total variation distance to uniform

### I.2 Next-token probabilities and top-k

For the same day-of-week setup:

1. Explain top-k sampling in your report:
   - top-k keeps only the k highest-probability next tokens, renormalizes, and samples from that restricted set.
2. Ensure **k >= 7** whenever top-k is available, so all seven days can remain reachable.
3. If your stack exposes token probabilities/logprobs, collect top next-token probabilities.
4. Compare model-implied probabilities (when available) to empirical frequencies from your 1000-sample runs.

Important caveat:

- If your chosen player stack does not expose logprobs, state that clearly and do the analysis using empirical histograms only.
- Optional extension: run one OpenAI player baseline with logprobs for this part only.

### I.3 Harder prompt

Now repeat the histogram exercise with a harder prompt:

- **"Fruits that start with b"**

Task:

1. Keep the same overall experimental pipeline.
2. Sample 1000 generations per temperature.
3. Build histograms and diversity metrics.
4. Compare how calibration behavior differs from the day-of-week case.

In your write-up, explain why parameters that worked in I.1 may not transfer cleanly to I.3.

---

## Part II: Self-Play with One Model

Now let one model play against itself in a 2-player Scattergories game.

Use the provided question bank:

- `scattergories_questions.csv`

### II.1 Game definition

For each row `(letter, category)`:

1. Player A samples one answer from the model/prompt/policy.
2. Player B samples one answer independently from the same model/prompt/policy.
3. Score using the standard uniqueness rule:
   - A player gets 1 point if answer is valid **and** not equal to opponent answer
   - Else 0 points

Validity checking requirements:

1. For each generated answer, call your `judge_model` with a strict yes/no prompt.
2. The judge prompt must be role-stable across all experiments (do not tune it differently by model matchup).
3. Normalize answers before judging and collision checks (trim, lowercase, collapse whitespace, strip punctuation at ends).
4. Cache judge outputs and report cache hit rate in your README.
5. Audit quality: randomly sample at least 50 judged examples and manually verify them; report estimated judge error rate.

### II.2 Experiments

1. Run repeated rounds for each question (enough rounds for stable estimates).
2. Start with your calibration settings from Part I.
3. Measure per-question and overall outcomes:
   - Validity rate
   - Collision rate
   - Average score per player
4. Revisit prompt/decoding choices and see whether self-play score improves.

Document what changed and why you think it helped (or did not).

---

## Part III: Cross-Model Competition

Now evaluate the role of diversity across local player models.

### III.1 Local model set

Use at least 3 different local models.

Hardware permitting, use a set like:

- `qwen2.5:7b`
- `mistral:7b`
- `llama3.2:3b`
- `gemma2:2b`

Fallback set:

- `llama3.2:1b`
- `qwen2.5:1.5b`
- `qwen2.5:3b`
- `gemma2:2b`

Run pairwise 2-player competitions for your chosen local models using the same question bank and scoring rule.

### III.2 Analysis

Compare:

1. Self-play for each local model
2. Cross-play for each pair of local models
3. The effect of using 3 models vs 4 models (if you run 4) on diversity and collisions

Report at least:

- Average score per player
- Collision rates
- Validity rates
- Which setup creates more distinct valid answers

Discuss whether cross-model diversity across local models appears to reduce harmful overlap and increase competitive performance.

---

## Part IV (Optional Challenge): More Local Diversity

If you want a harder extension, scale beyond 4 local models or include one remote player model for comparison.

Suggestions:

- Add 2 additional local models with different parameter scales
- Compare smaller vs larger local models under the same judge
- Document what changes in validity, collision rate, and diversity

If you take this route, keep the core required parts complete first.

---

## Deliverables

Submit the following:

1. **Report (PDF, 6 pages max)** with:
   - Part I histograms and calibration analysis
   - Next-token probability comparison (where supported)
   - Self-play results and prompt-revision analysis
   - Cross-model competition analysis
   - Brief discussion connecting your findings to `raghavan.pdf`
2. **Code** (scripts or notebooks) used to run all experiments
3. **Output data** (CSV/JSON) sufficient to reproduce key figures/tables
4. A short **README** with run instructions and environment notes
5. `prompts.md` containing the exact player prompt template(s) and judge prompt template(s) you used

## Grading Rubric

| Component | Weight | Criteria |
|-----------|--------|----------|
| Part I: Calibration + distributions | 25% | Sound experiment design, clear histograms, correct metrics |
| Part I: Probability comparison + top-k discussion | 15% | Correct interpretation of API capabilities and probability/frequency comparison |
| Part II: Self-play implementation + analysis | 25% | Correct game logic, stable estimates, thoughtful retuning/documentation |
| Part III: Cross-model competition analysis | 25% | Clear comparison and evidence-based conclusions about diversity |
| Code and report quality | 10% | Reproducible, readable code and clear writing |

## Technical references

- Paper for this assignment: `raghavan.pdf`
- OpenAI Chat Completions API docs: https://platform.openai.com/docs/api-reference/chat/create
- OpenAI logprobs notebook: https://cookbook.openai.com/examples/using_logprobs
- Ollama docs: https://github.com/ollama/ollama
- Ollama model library: https://ollama.com/library
- Recommended model pages:
  - https://ollama.com/library/qwen2.5
  - https://ollama.com/library/llama3.1
  - https://ollama.com/library/mistral
  - https://ollama.com/library/gemma2

## Academic Integrity

You may work in groups of up to 3 students. List all group members in your report and code. You may use AI tools, but you must briefly describe what you used them for.

Good luck!
