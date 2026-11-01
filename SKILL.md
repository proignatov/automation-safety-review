---
name: automation-safety-review
description: "Use when reviewing automations before unattended use."
version: 0.1.0
author: Alexander Ignatov
---

# Automation safety review

Review an automation by what happens when it is wrong, not just by how often it is right.

This skill is a procedure for an agent or operator. It does not implement access controls, stop a running job, or prove that an automation is safe. Keep existing permissions and approval requirements in force. Never create your own authorization or treat a payload hash as permission.

## When to use

Use before deploying an automation, changing its permissions, expanding its scope, or removing an approval step. Also use to audit an existing unattended process.

Do not use this procedure as permission to run live tests, contact people, spend money, or change production data.

## 1. Describe the action and its consequences

Record the input, intended output, owner, and exact scope. Follow the result downstream, including decisions a person may make from a read-only answer.

For each action, answer:

- How long before someone notices an ordinary mistake during a busy week?
- Who notices, and do they have the authority to stop the process?
- What does undoing it involve, including effects on other systems?
- How many items can be affected before the process stops?
- What evidence shows that the reviewer can detect the error before acting on it?

If these answers are missing, record the gap and do not approve unattended use on that basis.

## 2. Set the approval boundary

Classify consequences, not tool names:

| Class | Example | Default handling |
|---|---|---|
| A: safely repeatable | A new local draft that does not overwrite source data | May run within the agreed scope |
| B: recoverable with work | A record update with saved previous values and a tested rollback | A new process requires approval before changes; unattended use requires a separate owner decision |
| C: costly or impossible to take back | Sending a message, publishing, paying, or deleting without a verified recovery path | Prepare the action and wait for explicit authorization |

A read-only recommendation can still influence a costly decision. An isolated draft write is different from a live write without history. Use the most consequential effect to set the process-level boundary.

An owner may authorize a specific batch or a narrow standing rule where existing policy permits it. Record its limits. Do not silently expand, revoke, or reinterpret an existing authorization.

## 3. Record the baseline and failure condition

Before launch, complete both statements:

> As of [date], [metric] is [value], measured over [period], where [metric] means [definition].

> If [condition] occurs, or [metric] misses [threshold] by [date], we stop and return to [specific fallback]. [Named owner] can make that decision.

Define immediate stop conditions for harmful actions separately from the business review date. Do not wait for a scheduled review to stop an incident.

Keep a change log for metric definitions: date, change, decision owner, reason. Preserve the distinction between a real improvement and a change in how results are counted.

## 4. Bind approval to what will actually happen

Show the reviewer the actual destination, content, attachments, amounts, and other relevant parameters. Do not substitute a model-generated summary for the action being approved.

Record who approved what, the operation identifier, limits, and expiry through the existing trusted approval mechanism. A digest can identify the approved inputs; it does not authenticate the approver.

Use a frozen payload or an equivalent verified mechanism so the reviewed parameters cannot change between approval and execution. For personalized output, bind the template, data source, and allowed substitutions. Reject changes outside that agreement. Silence, rejection, expiry, or a mismatched action does not authorize execution.

## 5. Plan for failure before scheduling

Describe the states and transitions:

`waiting → validate → prepare → approve if required → execute → verify → confirmed success`

Include rejection, expiry, failure, unknown outcome, stopping, and manual recovery. For each state, specify its entry condition, time limit, log entry, and allowed next step.

Require:

- A stable operation ID across retries and a durable record of processed items.
- A concurrency control that prevents two workers from performing the same action.
- An explicit batch limit, timeout, and retry limit.
- Saved previous values and a tested recovery method for recoverable writes.
- A stop control and an alert route to a named person; verify that the alert can be received.

If an action may have happened but its acknowledgment was lost, mark the outcome UNKNOWN. Do not blindly repeat it or assign a fresh ID. Reconcile the actual effect first. If it cannot be established, stop for a manual decision unless an already approved mechanism makes repetition demonstrably safe.

Missing or damaged history is not proof that an item is new. Stop before further consequential actions until history is restored or the outcome is reconciled.

## 6. Test the controls and the reviewer

Load [the acceptance checks](references/acceptance-checks.md). Before running them, freeze the inputs, expected results, and acceptance criteria. Explain any non-applicable checks before testing.

Use synthetic data and isolated destinations. Block unintended network access and production writes. A test flag or a missing credential is not, by itself, evidence of isolation. Live tests require separate authorization.

Exercise ordinary execution, refusal, failure, restart, and recovery. Compare actual effects with an independently specified reference. Do not accept the executor's success message as evidence. For rollback, inspect the restored values, not just the return code.

Where safety depends on a person, give that person a mix of correct outputs and examples with known errors under realistic time and workload constraints. Agree on critical errors beforehand. If the reviewer misses one, do not count that review method as sufficient protection. A simulated reviewer is not a test of the real person.

After a change, rerun the applicable checks. Do not weaken the expected result to make a failed test pass.

## 7. Decide what may run unattended

The owner may relax a class B approval step only after reviewing evidence against the predefined cases and approving the exact scope. A count of successful runs or a quiet month is insufficient. Class C actions must remain covered by explicit authorization or a permitted, narrowly defined standing rule.

Report these separately:

- Whether an agent made the right decision.
- Whether the technical control actually enforced it.
- Whether a real reviewer detected the error.

Use PASS, FAIL, NOT RUN, or NOT IMPLEMENTED. Passing one layer does not pass the others. A finite test set does not guarantee performance on future inputs.

## Review output

Return the action scope and consequence class, baseline and stop conditions, approval boundary, test evidence, stop/recovery instructions, and unresolved gaps. State whether unattended use is approved by the owner, blocked, or still awaiting a decision.

Do not change a live schedule or repair production data merely because the review found a defect.
