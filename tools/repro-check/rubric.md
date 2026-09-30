# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env_recorded | The repro report's environment record, read against the issue body and the repo-facts block. | Pass when the report names the OS and the version of the tool the issue is about. If the issue says another setting changes the failure (driver, shell, build profile, language), that setting is named too. A version or OS that differs from the issue's target passes only when the report states the difference. An honest cannot-reproduce that records the environment it actually tried passes. No environment record fails. | required |
| steps_followable | The reproduction steps in the repro report, read against the trigger in the issue body. | Pass when a stranger could start from a public state and reach the trigger using the steps and inputs in the report. An honest cannot-reproduce whose steps show the attempt that was run passes. Fail when the steps exist only in a private repo or unshared config, omit the trigger the issue names, or skip a condition the issue says is required. | required |
| behavior_matches | Output excerpts, logs, or other artifacts in the repro report, read against the error or symptom the issue describes. | Pass when an artifact shows the issue's behavior (the same error, exit, or symptom the issue names), or when the report is an honest cannot-reproduce whose artifacts show what happened and name how that differs from the issue. Fail when there is no artifact; when the artifact is a different command, error, or exit and the report narrates it as the issue's behavior; or when the version differs from the issue's target, the report never says so, and the shown behavior is that other version's behavior. | required |
| honest_outcome | The report's stated conclusion, read next to its artifacts. | Pass when the conclusion does not claim more than the artifacts show. An evidenced cannot-reproduce passes. A report that matches the issue's symptom and says so passes. Fail when the report says the bug is reproduced, guaranteed, or root-caused, and no artifact shows that, or when the stated expected behavior contradicts the artifact. | required |
| claim_specific | The claim comment, read against the issue title and body. | Pass when the comment names this issue's specific behavior or a concrete next step on this issue, and does not promise a fix by a date or a guaranteed result. Fail on a bare +1, an interchangeable "assign me", or boilerplate that could be posted on any issue. | required |
| ai_disclosure | The contribution-policy line in the repo-facts block, then the claim comment and the repro report. | Pass when the policy does not require disclosing AI use, including silence, "AI is welcome if you understand the work", and "comments must be written by a human" when the comment is in the author's own words. Pass when the policy requires disclosure and a comment states that an AI assistant was used. Fail only when the policy says AI use must be disclosed and neither comment states that. Treat every course package as AI-assisted work. | required |

## Verdict rule

Accept only when every required check passes. Any required check graded fail or unclear rejects the package. Preferred checks never change the verdict. Unclear counts as fail.

In live mode on a claim-only draft, leave out every required check the skill grades `unclear` with evidence `not yet applicable: claim-only draft`. Accept the draft when every remaining required check passes.
