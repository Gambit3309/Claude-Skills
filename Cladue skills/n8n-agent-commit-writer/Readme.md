**1. Name of the Skill**



n8n-agent-commit-writer



**2. Purpose**



I built this because I kept committing n8n workflow changes with messages like "updated workflow" or "fixed stuff," then coming back weeks later with zero clue what I actually changed or why. n8n's JSON exports are dense and full of noise (positions, ids, pinned data) so eyeballing a diff to write a decent commit message takes way longer than it should. I wanted something that could just look at what changed and tell me, in plain terms, what happened and why the agent behaves differently now.



**3. How It Works**



You give it whatever you've got — a before/after JSON export of the workflow, a screenshot of the canvas, or just a quick description of what you changed (even a messy one). It figures out which input type it's looking at and works from there. For JSON diffs specifically, it walks the nodes and connections, ignores cosmetic junk like canvas position or regenerated ids, and zeroes in on stuff that actually changes agent behavior — system prompts, model swaps, tool bindings, branching logic, rewired connections. Then it spits out a proper commit message: a short imperative subject line, plus a body that names the nodes touched and explains what's different about how the agent acts now.



**4. Benefits**



Anyone working with n8n agents who's ever been burned by their own git log will get value here. It turns "what did I even change here" into an actual answer in seconds, so your commit history becomes something you can trust instead of something you have to re-derive by staring at JSON. It's also handy for teams — someone reviewing your commit doesn't have to open n8n and reconstruct the diff themselves to understand the behavioral impact of a change.

