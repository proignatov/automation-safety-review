# Automation safety review

A shareable draft by Alexander Ignatov, developed following a discussion with Raisa Belluyan about reversibility and the limits of human review.

The package contains:

- `SKILL.md`: the procedure an agent or operator should follow.
- `references/acceptance-checks.md`: sixteen checks to adapt to the workflow under review.

## How to use

Give your agent `SKILL.md` and the linked acceptance checks before asking it to design or review an automation. In systems that support skill folders, use that system's documented installation process. Otherwise, read the files as a review checklist. Merely copying the folder does not prove the agent will load or follow it.

The procedure asks for a failure condition before launch and distinguishes approval from actual verification. It also asks whether a human reviewer can detect the errors they are supposed to catch.

This package contains no runtime guards, credentials, integrations, or executable tests. It does not modify a workflow or authorize actions. Technical controls still have to be implemented and tested for each automation.

## Status

Version 0.1.0, public-review draft. The procedure is unchanged by this documentation update.

A limited decision-only pilot on 2026-11-01 compared agents given this public package with agents not given it. On six synthetic scenarios, with two independent repetitions per condition, both conditions produced 12/12 correct answers with sufficient justification. Neither condition produced a critical error or an unnecessary refusal of an allowed action.

**The pilot did not demonstrate an incremental benefit from the skill.** Both conditions reached the ceiling of this test. This is not evidence that the skill is useless in other settings, or that an automation is safe.

Read the [pilot method, results and limitations](references/pilot-2026-11-01.md). The pilot tested decisions from stated facts, not real workflow execution, technical enforcement or a real human reviewer's ability to catch errors. It does not count as a passing implementation of the sixteen acceptance checks. No benchmark generalisation, certification or production-safety guarantee is claimed.

Internal project details, host paths, operational data and private operational audit findings are intentionally excluded. Sharing this draft does not imply endorsement by anyone mentioned above.
