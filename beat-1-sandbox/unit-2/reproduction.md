# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

hangolehai

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5905170568

Hi, I'd like to claim this issue.

`Orchestrator` keeps one `ContextManager` and never empties it, so one orchestrator handling two reviews returns the first run's result for every tool whose input has not changed. The issue names `agent/orchestrator.py`, `agent/memory/session_store.py`, and `agent/memory/context_manager.py`.

Next I will call `Orchestrator.run` twice on one instance, with two profile ids, and report whether a tool whose input stays the same returns the first run's result. I will post that reproduction report on this issue. I am not promising a fix or a date.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5905170691

## Environment

- OS: macOS 26.6.2, arm64
- Python: 3.13.3, venv at `.venv` with `structlog` 26.1.0 and `redis` 5.x installed so `agent.orchestrator` can import
- Repo: fork `hangolehai/pathreview-ai301-fa26-s3`, branch `main`, commit `2f4e82f`
- No Redis session store was attached. I did not start Docker.

## Steps

From the repo root I ran this script with `.venv/bin/python`. `CountingTool` records how many times `execute` runs. The two profiles use different `resume_text` values, so `skill_extractor` input changes. `_build_plan` still appends `market_analyzer` with `{"detected_skills": {}}` whenever the plan is non-empty, so that tool's input is the same on both runs.

```python
import sys
sys.path.insert(0, ".")
from agent.orchestrator import Orchestrator
from agent.tools.base import BaseTool, ToolResult

class CountingTool(BaseTool):
    def __init__(self, name):
        self.name = name
        self.description = "counting stand-in"
        self.calls = 0
    def execute(self, input_data: dict) -> ToolResult:
        self.calls += 1
        return ToolResult(success=True, data={"call_number": self.calls, "tool": self.name})

skill = CountingTool("skill_extractor")
market = CountingTool("market_analyzer")
orch = Orchestrator(tools={"skill_extractor": skill, "market_analyzer": market})

first = orch.run("profile-a", {"resume_text": "alpha resume"})
second = orch.run("profile-b", {"resume_text": "beta resume"})

print("FIRST_MARKET", first["tool_results"].get("market_analyzer"))
print("SECOND_MARKET", second["tool_results"].get("market_analyzer"))
print("FIRST_SKILL", first["tool_results"].get("skill_extractor"))
print("SECOND_SKILL", second["tool_results"].get("skill_extractor"))
print("MARKET_CALLS", market.calls)
print("SKILL_CALLS", skill.calls)
```

## What I observed

`market_analyzer` logged `tool_result_cache_miss` on profile-a and `tool_result_cache_hit` on profile-b, both for key `market_analyzer:95e4a8f9276c0150ec6978a4ef9d9a2cac4d341eeaa23a74725f91d9eea97930`. Printed results:

```
FIRST_MARKET {'call_number': 1, 'tool': 'market_analyzer'}
SECOND_MARKET {'call_number': 1, 'tool': 'market_analyzer'}
FIRST_SKILL {'call_number': 1, 'tool': 'skill_extractor'}
SECOND_SKILL {'call_number': 2, 'tool': 'skill_extractor'}
MARKET_CALLS 1
SKILL_CALLS 2
```

Profile-b received profile-a's `market_analyzer` result (`call_number` 1). `execute` ran once for that tool. `skill_extractor` ran twice, because its input changed with the resume text. That matches the issue: one `ContextManager` created in `Orchestrator.__init__` is reused, and the cache key is `tool_name` plus the hash of the tool input, not `profile_id`.

I did not reproduce the saved-session half. `run` only calls `session_store.get` and `session_store.set` when a store is passed, and I passed none, so I have no observation about deleting a profile's Redis session at the start of `run`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `python3 run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --limit 3`: `agreement: 3/3 scored items`.
2. Confirming full run, the one saved with `--save-run`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

The last score matches the agreement line in `eval-run.txt`.

**Package analysis**

`pkg-20` (source line in the bundle: `ghostty-org/ghostty#13604`). Gold label is reject. My rubric's verdict was reject. The saved run says `pkg-20  reject  reject   yes`.

The report itself shows the issue's behavior. The single-theme run prints `^[[?997;2n` and the conditional-pair control prints `^[[?997;1n`, and the report says "Actual: the single-theme run answers `997;2` (light, first capture), the conditional-pair run answers `997;1` (dark, second capture)". Environment and steps are present. The reject comes from `ai_disclosure`. The repo-facts line says: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The candidate claim and repro never state that an assistant was used. The check says "Fail only when the policy says AI use must be disclosed and neither comment states that. Treat every course package as AI-assisted work." So the package fails that required check, and the verdict rule rejects it.

**Check rationale**

Quoted from the uploaded `rubric.md`, row `ai_disclosure`:

`| ai_disclosure | The contribution-policy line in the repo-facts block, then the claim comment and the repro report. | Pass when the policy does not require disclosing AI use, including silence, "AI is welcome if you understand the work", and "comments must be written by a human" when the comment is in the author's own words. Pass when the policy requires disclosure and a comment states that an AI assistant was used. Fail only when the policy says AI use must be disclosed and neither comment states that. Treat every course package as AI-assisted work. | required |`

I wrote the fail condition that narrow because the disclosure category is one package, and the other policies in the set are conditions rather than disclosure walls. `pkg-05` says "generative AI tools welcome; you are responsible for all contributions" and is a clear accept, so a check that failed every missing "I used AI" sentence would reject packages the gold label accepts. `pkg-07` does disclose, and this wording still passes it.

**Trade-offs**

The check gives up reports on repos whose policy never says "must be disclosed." A comment can be AI-assisted there and still pass, which is what I want for `pkg-05`, and it is also a case I accept missing. I did not loosen the check after the smoke run. The confirming run stayed `agreement: 20/20 scored items  (bar: 18/20: PASS)` with `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`, so the disclosure fail on `pkg-20` did not flip a clear accept.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
