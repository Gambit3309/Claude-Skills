---
name: n8n-agent-commit-writer
description: Writes Git commit messages for changes to n8n AI agent workflows — a short imperative summary line plus a body explaining which nodes/logic changed and why the agent's behavior changed as a result. Works from either an exported workflow JSON diff (before/after), a plain description/screenshot of what changed in the n8n canvas, or a mix of these. Use whenever the user asks to write a commit message for an n8n workflow, agent, or automation change, or pastes n8n workflow JSON and wants it committed. Trigger this proactively any time n8n workflow JSON appears alongside a request related to git, commits, or "what did I just change."
---

# n8n Agent Commit Writer

Writes Git commit messages for changes to n8n AI agent workflows, from whatever the user hands you: a before/after JSON diff, a plain-language description, a screenshot of the canvas, or some mix.

The goal of the commit message is **future-you clarity**: when the user looks back at this commit in a month, they should immediately understand what changed and why the agent now behaves differently — not just "updated workflow.json".

## Input handling

Figure out what you've been given and follow the matching path. If the user provides more than one kind of input (e.g. JSON diff + a sentence of context), use all of it — the plain-language context often explains *why*, which the JSON alone can't tell you.

### Path A — Workflow JSON diff (before/after)

See `references/json_diff_guide.md` for the full method. In short:
1. Parse both JSON blobs (or the diff if already provided).
2. Walk the `nodes` array and diff by node `id`/`name`: added, removed, or modified nodes.
3. For modified nodes, pay special attention to fields that change *agent behavior*, not cosmetic ones — see the reference for the full list (system prompts, tool bindings, model/parameters, conditional logic, connections between nodes).
4. Diff the `connections` object separately — a rewired connection can change agent behavior with zero node edits.
5. Ignore pure noise: `position` (canvas coordinates), `id` regeneration with no other change, `pinData`, timestamps, and version metadata unless the user asks about them.

### Path B — Plain-language description

The user tells you what they changed in their own words (may be messy or shorthand). Ask yourself: what node(s) were touched, and what does that imply about behavior? Don't just restate their words back as the commit message — translate into the "what changed / why it matters" structure below. If their description is too vague to know which nodes were involved (e.g. "fixed the bot"), ask one clarifying question rather than guessing.

### Path C — Screenshot of the canvas

Read the screenshot for node names, visible connections, and any visible config panels. Screenshots rarely show a "before" state, so if the user only supplies an "after" screenshot, ask what the prior behavior was, or write the message from what's inferable (new node present, connection pattern) and flag anything you're inferring rather than confirming.

## Commit message format

**Subject line** (required, ~50-72 chars, imperative mood, no period):
```
<verb> <what changed, in n8n terms>
```
Examples: `Add retry logic to invoice-parsing agent`, `Swap GPT-4o for Claude in support-triage node`, `Fix broken tool binding on Slack notifier`.

**Body** (blank line after subject, then plain prose or short bullets — whichever fits the size of the change):
- What changed, node by node if more than one node was touched (name each node).
- Why it matters: how the agent's *behavior* is different now (not just "config updated" — say what the config controlled and what changes as a result).
- If there's a clear reason from context (bug being fixed, feature being added), state it plainly.

Keep the body proportional to the change. A one-node tweak might just need 1-2 sentences. A multi-node restructure can use bullets, one per node/area touched.

Do not invent motivations you weren't given — if the "why" isn't clear from the input, describe the "what" precisely and skip speculation about intent, or ask.

## Output

Give the user the commit message in a fenced code block, ready to copy into `git commit -m "..." -m "..."` or a commit editor. Don't wrap it in extra commentary — a one-line lead-in ("Here's the commit message:") is enough.

## Worked example

Input: JSON diff showing a node named `Classify Intent` had its `systemMessage` parameter changed from a 2-category prompt to a 4-category prompt, and a new `Escalate to Human` node was added, connected from `Classify Intent`'s new 4th branch.

Output:
```
Add human escalation branch to intent classifier

- Classify Intent: expanded system prompt from 2 categories
  (question/complaint) to 4 (question/complaint/urgent/spam)
- Added Escalate to Human node, wired to the new "urgent" branch

Agent now routes urgent messages to a human instead of
attempting to auto-respond.
```
