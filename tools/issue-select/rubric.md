# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not_archived | Eval: the `archived:` flag on the repo line under Repo facts. Live: the archived banner on the repository front page. | Pass only when archived is no. An archived repository fails. | required |
| recent_human_commits | Eval: "last 5 default-branch commits" under Repo facts, measured against the bundle capture date. Live: the newest commits on the default branch, measured against today. | Pass when the newest of those commits is within 365 days, and at least one of the five is authored by a human or is a bot commit that merges a human pull request. A username ending in `[bot]` is not a human author. Older than 365 days fails. | required |
| ai_policy_allows_assist | Eval: the "contribution policy" line under Repo facts. Live: `CONTRIBUTING.md` (repo root or `.github/`) and any AI policy file it links, such as `AI_POLICY.md`. | Pass when the policy says nothing about AI, welcomes AI, or only sets conditions (disclose AI use, personally understand and test the change, human review). Fail only on an outright ban that rejects AI-generated code or documentation with no allowance for assisted use. A ban on fully generated work that still allows assisted use passes. | required |
| unclaimed | Eval: "this issue: assignees:" and "linked PRs:" under Repo facts, plus the Comments section. Live: the Assignees box, the Development box, and the comment thread. When the sidebar and the thread disagree, believe the thread. | Pass when assignees is none, no linked PR is open, and no comment in the 90 days before the capture date (live: before today) says the author is taking or already working on the issue ("I'll take this", "working on this", "can I work on this", "in progress") unless a later Owner, Member, or Collaborator comment invites other people to take it. Closed or merged PRs do not count. A claim older than 90 days does not count. | required |
| newcomer_scope | Eval: the Issue title, body, and Comments. Live: the same text on the issue page. | Pass when the issue asks for one bounded change: a bug with current and expected behavior, a documentation change that names the pages or files to edit (several named pages still count as one task), or a small UI fix. Do not fail a short writeup or a missing reproduction checklist. Fail when the issue is an umbrella, tracking issue, or megaissue; when it is a list of separate issues meant to be split; when it asks for a codebase-wide change; when the thread shows the design is still debated and no maintainer has settled it; when a maintainer says the fix must change core internals such as the parser; when the body is only a usage question; or when the request depends on a product decision (what the asset is, or whether the feature belongs) that no maintainer has decided. A good-first-issue label does not override a fail. | required |
| maintainer_filed | Eval: the issue opener's `author_association`. Live: the opener's Owner, Member, or Collaborator badge. | Pass when the opener is OWNER, MEMBER, or COLLABORATOR. | preferred |
| good_first_label | Eval: the labels on the issue opening line. Live: the labels on the issue sidebar. | Pass when a label contains "good first issue" or "easy". | preferred |

## Verdict rule

Accept only when every required check passes. Any required check graded fail or unclear rejects the issue. Preferred checks never change the verdict; they only rank issues that are already accepted. Unclear counts as fail.

In live mode on the Path Review repo, apply the house rule in `scope.md`: other students' claim comments do not fail `unclaimed`. Eval mode ignores `scope.md`, so claim comments in a bundle still count.
