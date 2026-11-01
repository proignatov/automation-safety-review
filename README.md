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

Version 0.1.0, public-review draft. It is adapted from a privately used procedure; this public adaptation has not itself undergone a separate execution trial. No benchmark, certification, or production-safety guarantee is claimed. The acceptance checks have no recorded passing results in this package.

Internal project details, host paths, operational data, and audit findings are intentionally excluded. Sharing this draft does not imply endorsement by anyone mentioned above.
