---
name: pc-performance-diagnosis
description: Diagnose PC hardware performance problems and walk the user through fixing them, step by step. Use this any time a user describes a performance complaint about their desktop or laptop — stuttering, lag, fps drops, thermal throttling, slow boot/load times, a system that "feels slower than it should," fan noise spikes, random freezes, or a new component (CPU, GPU, RAM, SSD) that isn't performing to its rated spec. Also use it when a user shares benchmark scores, temperature readings, or monitoring tool output (HWMonitor, MSI Afterburner, Task Manager, Resource Monitor, HWiNFO) and wants help interpreting them. Trigger even if the user doesn't name specific hardware or use technical terms — e.g. "my pc feels sluggish lately" or "why does my laptop get so hot and slow down" should trigger this skill. Covers both Windows and general/cross-platform desktops and laptops.
---

# PC Hardware Performance Diagnosis

Help the user figure out *why* their PC is underperforming and walk them to a fix. Think like an experienced PC builder doing remote triage: don't jump straight to "run this benchmark" if the symptom already points somewhere specific, but don't guess wildly either when the picture is unclear.

## Overall approach

1. **Read the symptom carefully first.** Many complaints have a recognizable signature — you often don't need any tooling to form a strong hypothesis.
2. **If you can confidently name the likely cause**, explain it and give the step-by-step fix directly. Don't send the user off to run benchmarks just to confirm something you already have good reason to believe.
3. **If the symptom is ambiguous** (could plausibly be several different root causes), don't guess — walk the user through the specific monitoring/benchmarking step that will disambiguate, tell them exactly what to look for, then interpret what they report back.
4. Either way, deliver the result as a **step-by-step walkthrough**, not a wall of diagnostic theory. The user should always know what to do next.

Use the `step_card_display_v0` tool for the walkthrough when you have 3+ concrete ordered steps — that's what this skill is built around. If you're just asking a quick clarifying question before you can even start triaging, ask in plain text first.

## Step 1: Build a picture from the symptom

Before reaching for tools, mentally sort the complaint into a category. The categories and their classic tells are in `references/symptom_patterns.md` — read it before responding. It covers:

- Thermal throttling (heat-related slowdowns)
- CPU-bound stutter vs GPU-bound stutter vs storage-bound stutter
- RAM/memory pressure (paging, browser-tab slowdowns, freezes under multitasking)
- Storage degradation (slow boots, slow loads, general "everything feels laggy")
- New-component underperformance (bottlenecking, misconfiguration, driver/BIOS issues)
- Background software/OS causes that mimic hardware problems (worth ruling out first — they're the most common false lead)

If the user's description clearly matches one pattern and there's no strong competing explanation, skip straight to a fix (Step 3) — no need to make them run tools first.

If it's genuinely ambiguous, or multiple categories fit equally well, or the user hasn't given enough detail to tell — go to Step 2.

## Step 2: Targeted diagnostics (only when needed)

Don't hand the user a generic "run these five benchmarks" checklist. Pick the *one or two* tools that will actually separate the competing hypotheses, and tell them precisely what to check.

`references/diagnostic_tools.md` has the tool-to-symptom mapping (what to install/open, what metric to watch, what values are normal vs concerning) for CPU, GPU, RAM, storage, and thermals, on both Windows and general/cross-platform setups.

When asking the user to run something:
- Name the specific tool and the specific metric (e.g., "open Task Manager's Performance tab and watch GPU usage while the stutter happens" — not just "check your GPU").
- Tell them what result would point where, so they know what to report back and don't need a second round-trip if they can read it themselves.
- If they paste back numbers, temps, or a screenshot, interpret it against the reference thresholds and move to Step 3.

## Step 3: Deliver the walkthrough

Once you have (or already had) enough to diagnose, give the fix as an ordered, concrete walkthrough. Structure:

1. **Likely cause** — one or two sentences, plain language, no hedging if you're confident.
2. **Ordered steps** — via `step_card_display_v0`, each step being a real action (open X, change setting Y, reseat Z, reapply thermal paste, update driver, etc.), not a diagnostic question.
3. **If the fix might not fully resolve it**, say what to check next and when it's worth considering a hardware replacement/RMA rather than more tinkering.

Keep the tone practical and confidence-calibrated — say plainly when something is a near-certain diagnosis vs an educated guess that needs the user to confirm afterward whether it worked.

## A note on scope

This covers general consumer desktops and laptops, Windows-first but not Windows-exclusive. It's about *performance* problems (slowness, stutter, throttling, underperformance vs spec) — not physical damage, won't-boot/dead-hardware triage, or pure software bugs unrelated to hardware performance. If the user's issue turns out to be one of those, say so and redirect rather than forcing it into this framework.
