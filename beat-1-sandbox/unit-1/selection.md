# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15

**Verdict output**

All evidence gathered. Here's the grade for issue #15 ("Agent session state is not cleared between reviews for the same user").

**Required checks**
- **not_archived** — pass: `isArchived: false` on the repo.
- **recent_human_commits** — pass: newest default-branch commit `2f4e82f` dated 2026-09-16 (within 365 days), authored by `Aburke225`, a human.
- **ai_policy_allows_assist** — pass: `docs/CONTRIBUTING.md` sets workflow conditions (branch naming, CI, remove xfail markers) but states no AI ban; silence/conditions pass.
- **unclaimed** — pass: assignees none, no linked PRs (`closedByPullRequestsReferences` empty), zero comments.
- **newcomer_scope** — pass: one bounded bug with current behavior ("keeps one `ContextManager` and never empties it… returns the first run's result") and expected behavior ("Empty that cache and delete the profile's saved session state when `run` starts"), three named files, 3–4h estimate. No maintainer flagged core-internals rework; not an umbrella/design/product issue.

**Preferred checks** (rank only)
- **maintainer_filed** — pass: opener `author_association` is COLLABORATOR.
- **good_first_label** — pass: carries "good first issue" (also "tier-1" starter).

Every required check passes → **accept**. Both preferred checks also pass, making this a strong candidate. One fit caveat worth noting: your profile says you want to avoid core internals — this bug lives in the agent orchestrator/memory layer, so it leans more "internals" than a UI or docs fix, though it's a well-bounded, maintainer-filed starter with explicit current/expected behavior.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15",
  "checks": [
    {"name": "not_archived", "grade": "pass", "evidence": "Repo isArchived: false"},
    {"name": "recent_human_commits", "grade": "pass", "evidence": "Newest commit 2f4e82f on 2026-09-16 by human Aburke225"},
    {"name": "ai_policy_allows_assist", "grade": "pass", "evidence": "docs/CONTRIBUTING.md sets conditions but no AI ban"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees none, no linked PRs, no comments"},
    {"name": "newcomer_scope", "grade": "pass", "evidence": "Bounded bug with current+expected behavior and three named files, 3-4h estimate"},
    {"name": "maintainer_filed", "grade": "pass", "evidence": "author_association is COLLABORATOR"},
    {"name": "good_first_label", "grade": "pass", "evidence": "'good first issue' label present"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run, `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --limit 3`: `agreement: 3/3 scored items`.
2. Confirming full run, the one saved with `--save-run`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

The last score matches the agreement line in `eval-run.txt`.

**Issue analysis**

`issue-19` (source line in the bundle: `zxcalc/zxlive#517`). Gold label is accept. My rubric's verdict was reject. The saved run says `issue-19  accept  reject   NO     failed: newcomer_scope, good_first_label (preferred)`.

The required failure is `newcomer_scope`. The issue body says: "There are two potential causes which should be fixed:" and then lists matcher speed and a UI update waiting on the matching thread, plus "Additional suggestions" that name multi-processing, matching only expanded categories, and applying the rewrite on a separate thread. The check fails an issue "when it is a list of separate issues meant to be split" or "when the thread shows the design is still debated and no maintainer has settled it." The grader treated those two causes and three suggestions as more than one bounded change, so `newcomer_scope` failed and the verdict rule rejected the issue. `good_first_label (preferred)` is in the note but cannot change the verdict. The bundle also says "assignees: none; linked PRs: none" and the repo is active, which is why the other required checks did not sink it.

**Check rationale**

Quoted from the uploaded `rubric.md`, row `newcomer_scope`:

`| newcomer_scope | Eval: the Issue title, body, and Comments. Live: the same text on the issue page. | Pass when the issue asks for one bounded change: a bug with current and expected behavior, a documentation change that names the pages or files to edit (several named pages still count as one task), or a small UI fix. Do not fail a short writeup or a missing reproduction checklist. Fail when the issue is an umbrella, tracking issue, or megaissue; when it is a list of separate issues meant to be split; when it asks for a codebase-wide change; when the thread shows the design is still debated and no maintainer has settled it; when a maintainer says the fix must change core internals such as the parser; when the body is only a usage question; or when the request depends on a product decision (what the asset is, or whether the feature belongs) that no maintainer has decided. A good-first-issue label does not override a fail. | required |`

I wrote it this way so a friendly label cannot rescue an umbrella or an unsettled design, while a short bug report or a docs task that names its pages can still pass. The fail list is the scope family from the evidence guide: umbrella or tracking issues, codebase-wide work, an open design debate, core internals, usage questions, and an undecided product choice.

**Trade-offs**

That wording is what rejected `issue-19`. The body really is one performance bug with named causes ("The matchers are slow for certain rewrites" and "UI update is waiting for the matching thread"), and the gold label is accept, but the numbered "two potential causes" plus "Additional suggestions" looks like a list to split, so the check fails a case it was meant to allow. I am leaving the check as written: loosening "list of separate issues" would also risk accepting megaissues such as `issue-10`, whose title is "Documentation request megaissue". The full run still matches the other 19 items, including scope 4/4, so the miss is this one arguable read and not a whole category.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Issue 15 fits the time I have. The body estimates 3–4 hours and names three files. I wanted a single bug with a clear current and expected behavior, and this is that: the orchestrator keeps a context cache, so a second review can return the first review's result, and the fix is to empty that cache when `run` starts.
2. The verdict was right that the issue is unclaimed, the repo is active, and the work is one bug rather than an umbrella. What the rubric could not weigh is the fit note in the same output: the bug sits in the agent orchestrator and memory layer, which is closer to internals than a docs fix. I still chose it because the files and the expected behavior are spelled out.
3. Claiming it should be straightforward. There is no assignee and no comment yet. The harder part will be reproducing the stale session across two reviews before I change the cache, and keeping the fix inside those three files.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
