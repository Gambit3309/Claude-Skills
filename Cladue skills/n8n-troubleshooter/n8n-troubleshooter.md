---
name: n8n-troubleshooter
description: Diagnoses and fixes errors thrown while executing or testing an n8n workflow — failed node runs, red error badges, execution log failures, credential/auth errors, expression errors, "Cannot read property of undefined", JSON parse errors, webhook not firing, rate limits, and similar runtime failures. Use this skill whenever the user pastes an n8n error message, an execution log, a screenshot of a failing node, or says something like "I'm getting this error in n8n", "this node keeps failing", "my workflow broke", or is implementing a step from a plan (e.g. from the n8n-workflow-architect skill) and hits an error trying to run it. This is distinct from n8n-workflow-architect, which designs workflows from scratch — reach for THIS skill once something is already built and is erroring out during execution, even if the user doesn't say "debug" or "n8n" explicitly.
---

# n8n Troubleshooter

A skill for diagnosing and fixing runtime errors in n8n workflows — the "I tried to run this and it broke" moment, as opposed to workflow design (see `n8n-workflow-architect` for that).

## When to use this

Trigger on:
- A pasted n8n error message or execution log (JSON or plain text)
- A screenshot of a red/failing node
- "This node keeps failing", "I'm getting this error", "my workflow broke", "why won't this run"
- The user is mid-implementation of a plan (possibly from `n8n-workflow-architect`) and hits an error trying to execute a step

If the user hasn't built anything yet and wants a workflow designed, that's `n8n-workflow-architect`, not this skill.

## Workflow

### 1. Gather whatever evidence exists
The user may hand you any of:
- Raw error text/message
- An exported execution log (JSON)
- A screenshot of the failing node
- Just a description of the symptom

Work with whatever you're given — don't block on demanding a specific format. But if the evidence is too thin to localize the failure (e.g. "it's not working" with no error text and no node named), ask for the exact error message or execution log before guessing. Never invent an error message or fabricate log contents.

### 2. Localize the failure
Identify:
- **Which node** failed (name and type — HTTP Request, Function/Code, Set, IF, Webhook, etc.)
- **Which layer** it failed at:
  - **Trigger layer** — nothing fires at all (webhook never received, cron didn't run, manual trigger misconfigured)
  - **Node layer** — a specific node throws (auth failure, bad expression, malformed input, timeout)
  - **Output layer** — the workflow completes "successfully" but produces wrong/incomplete data

### 3. Classify against common failure patterns
Check the error against `references/common-errors.md` first — it catalogs the failure signatures n8n throws most often (credential/auth errors, expression syntax errors, null/undefined field access, JSON parse errors, webhook registration issues, rate limiting, pagination gaps, node-specific quirks for HTTP Request, Function/Code, Set, IF, Switch, Merge, and common integrations). Most real-world n8n errors fall into one of these known buckets — check there before reasoning from first principles.

### 4. Diagnose root cause
State plainly, in terms of the actual data flowing through the workflow:
- What the node expected
- What it actually received (or what state it was in)
- Why that mismatch produced this specific error

Avoid vague diagnoses like "there's an auth issue" — say *which* credential, *which* field, or *which* upstream node produced the bad data.

### 5. Propose the fix
Give a concrete fix, not just an explanation. Depending on the error, this means:
- A corrected node parameter, expression, or JSON snippet
- A specific setting to change (credential re-auth, retry policy, timeout value)
- A small code/expression fix for Function/Code nodes

### 6. Verify the fix before handing it back
Before presenting the fix, re-check it against the original error:
- Would this change actually eliminate the specific error message/symptom reported, or only a related one?
- Does the fix introduce a new failure mode (e.g. hardcoding a value that breaks on the next run, fixing one record shape but not null/empty cases)?
- If you're not confident the fix resolves the *exact* reported error, say so explicitly and note what to check after applying it, rather than presenting it as certain.

### 7. Add a prevention tip
Wherever practical, suggest the small addition that stops this failing silently next time — e.g. an IF node checking the response before the next step, a Set node defaulting a possibly-missing field, or an Error Trigger branch. Keep this to one or two sentences; it's a bonus, not the main deliverable.

## Output format

Always structure the response as:

1. **Root cause** — one or two sentences, specific to the node/data involved
2. **Fix** — the concrete change, with a corrected snippet/expression/config if applicable
3. **Verified against** — one line confirming the fix addresses the exact reported error (or flagging remaining uncertainty)
4. **Prevention tip** — one line, optional if nothing meaningful to add

Keep it tight. The user is mid-debug, not reading a report — lead with the fix, not a long narrative.

### 8. Offer a deeper explanation

After the structured fix, always close with a short offer along these lines: "Want a deeper explanation of why this happened and why the fix works?"

Do not front-load the deep explanation by default — it slows down someone mid-debug who mostly wants unblocked. But if the user says yes (or in a follow-up asks "why" / "explain"), give a fuller breakdown covering:
- The underlying mechanism (e.g. how n8n evaluates expressions per-item, how OAuth token refresh actually works, how webhook activation registers the listener)
- Why the specific symptom followed from that mechanism
- Why the proposed fix addresses the mechanism, not just the symptom — i.e. why it won't just resurface in a different form later
- If relevant, a brief note on the underlying concept (e.g. optional chaining, idempotency, token lifecycle) for someone building general debugging intuition, not just patching this one case
