**1. Name of the Skill**



pc-performance-diagnostics



**2. Purpose**



I built this skill because PC performance issues are one of the most common things people ask Claude about, but generic advice ("run some benchmarks," "check your temps") wastes time. Most performance complaints actually fall into a handful of recognizable patterns — thermal throttling, CPU/GPU bottlenecks, RAM pressure, storage degradation — and an experienced builder can often spot the cause just from the symptom description, without making someone run five different tools first. I wanted Claude to diagnose the same way: recognize the obvious cases directly, and only send someone to run a specific tool when the cause is genuinely ambiguous.



**3. How It Works**



The skill triggers whenever someone describes a performance problem — stutter, throttling, "feels slower than it should," a new part underperforming — on a Windows or general desktop/laptop setup.



It works in three steps:



Pattern-match the symptom. A reference file maps common symptom signatures (e.g., slowdown that builds up during sustained load + loud fans = thermal throttling) to likely causes. If the match is confident, Claude skips straight to the fix.

Targeted diagnostics if needed. If the cause is unclear, Claude points to the one or two specific tools that will actually disambiguate it (e.g., Task Manager's GPU vs CPU usage to tell a GPU bottleneck from a CPU bottleneck) — not a generic checklist — and tells the user exactly what value to look for.

Deliver a step-by-step walkthrough. The final output is always an ordered, actionable fix — not just a diagnosis — using a step-by-step card format so it's easy to follow.



It also rules out common false leads first (background processes, malware, outdated drivers) before assuming it's a hardware fault, since those get misread as hardware problems constantly.



**4. Benefits**



For anyone using it, it turns "my PC feels off somehow" into a concrete, actionable path — without the back-and-forth of running tools that weren't going to be useful anyway. It saves time for people who already have a strong hint about the cause (no unnecessary diagnostics) and gives clear direction to people who don't (the right diagnostic, not a shotgun list). Because it's built as a skill, it's reusable — anyone who has it installed gets consistent, expert-level triage on this topic instead of a one-off improvised answer, and it can be refined over time as new symptom patterns or tools come up.

