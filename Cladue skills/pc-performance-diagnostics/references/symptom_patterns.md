# Symptom Patterns

Quick-reference for matching a described symptom to a likely root cause category. Use this to decide whether you can diagnose directly (Step 1 in SKILL.md) or need targeted diagnostics (Step 2).

## Rule out software/OS causes first

These mimic hardware problems constantly and are the most common actual cause. Before assuming hardware, consider:
- Too many startup programs / background processes (general sluggishness, especially after time since last reboot or after installing new software)
- Windows Update running in the background (sudden, temporary slowdown + disk activity)
- Malware/crypto-miners (persistent high CPU/GPU usage with no obvious cause, fans always spinning, especially if it correlates with a recent download)
- Outdated or corrupted GPU/chipset drivers (stutter that started right after a driver update, or a Windows update)
- Browser tab bloat (RAM pressure symptoms, but only in browser-heavy usage)

If the symptom could easily be one of these and the user hasn't mentioned ruling them out, ask or suggest checking Task Manager for a rogue process before concluding it's hardware.

## Thermal throttling

**Tells:** Performance drops progressively during sustained load (not immediately), especially during gaming, video export, compiling, or other CPU/GPU-intensive tasks. Often worse in summer, in a closed space, on a laptop on a soft surface (bed, lap), or after the machine has been dusty/unused for a while. Fans ramp to max and stay there. Sometimes accompanied by sudden shutdowns (extreme case) or coil whine.

**Common causes:** dust buildup, degraded thermal paste (laptops/older desktops especially — paste dries out over 2-4 years), blocked intake/exhaust, laptop on a soft/enclosed surface, undersized cooler for the CPU/GPU, poor case airflow, fans not spinning up correctly.

**Confidence:** High if the pattern matches (slow ramp during sustained load + fan noise + environmental factor). Can often go straight to fix (clean dust, reapply paste, elevate laptop, check fan curves) without diagnostics.

## CPU-bound stutter/slowdown

**Tells:** Stutter or low frame rate that correlates with CPU-heavy scenarios — many NPCs/physics in games, background compiling, video encoding, heavily multitasking. GPU usage stays well below 100% during the stutter (if the user already knows this). Older/budget CPU paired with a much newer GPU.

**Common causes:** CPU bottleneck (GPU is waiting on the CPU), background processes competing for CPU time, insufficient cooling causing CPU throttling specifically, outdated BIOS/chipset drivers, RAM running below rated speed (affects CPU-bound performance significantly, especially AMD platforms).

## GPU-bound stutter/low fps

**Tells:** Frame rate scales directly with graphics settings (drop resolution/settings → smoother). GPU usage pegged near 100% during the stutter. Especially visible in demanding, graphically-rich games or GPU-heavy workloads (rendering, video work).

**Common causes:** GPU underperforming vs its rated spec (check against known benchmarks for that model), outdated/corrupted GPU driver, insufficient PSU wattage causing power limiting, VRAM running out (stutter specifically when textures/assets stream in), thermal throttling on the GPU specifically.

## RAM / memory pressure

**Tells:** Slowdown that gets worse the longer the session runs or the more apps/tabs are open, general system-wide unresponsiveness (not just one app), heavy disk activity when switching between apps (paging to disk), freezes when opening something memory-heavy.

**Common causes:** Insufficient RAM for the workload, RAM running in single-channel instead of dual-channel, a memory leak in a specific application, RAM speed/timings not matching the motherboard's optimal profile (XMP/EXPO not enabled).

## Storage-bound slowness

**Tells:** Slow boot times, slow app/game load times, general system feels laggy but CPU/GPU usage look normal, freezing specifically when saving/loading files, an older HDD in the mix, or an SSD that's nearly full.

**Common causes:** Drive nearing end of life or heavily fragmented (HDD), SSD approaching full capacity (performance degrades notably above ~85-90% full on many drives), still booting from an HDD instead of an SSD, a drive running in a slower mode than it should (e.g., an NVMe SSD accidentally running in a SATA-speed PCIe lane, or AHCI vs RAID mode misconfiguration).

## New component underperforming vs spec

**Tells:** User just installed/upgraded a CPU, GPU, RAM, or SSD and it's not hitting expected benchmark numbers or "feels" no faster than before.

**Common causes, roughly in order of likelihood:**
1. Bottleneck from another component (e.g., new GPU capped by an old CPU) — not actually a fault, just an expectation mismatch
2. RAM running at default JEDEC speed instead of rated XMP/EXPO speed (very common, huge and easy win)
3. Driver not installed/updated for the new part
4. BIOS out of date and not fully supporting the new part (common with new CPU generations on older boards)
5. Component not seated properly / running in a suboptimal slot (e.g., GPU in a PCIe slot with fewer lanes, RAM in the wrong DIMM slots for dual-channel)
6. Power limits set conservatively in BIOS/software
7. Genuinely defective unit (least likely, but worth floating once everything else is ruled out — suggest checking against verified reviewer benchmarks for that exact model)

This category almost always benefits from Step 2 diagnostics, since "underperforming vs spec" requires an actual number to compare against.
