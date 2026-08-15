# Diagnostic Tools Reference

Tool-to-symptom mapping. Point the user at the *smallest* set of tools that will resolve the ambiguity — usually one, occasionally two.

## Temperatures (thermal throttling suspected)

**Windows:** HWMonitor or HWiNFO64 (free). Have them open it, run the task that causes the slowdown (game, export, etc.), and report back peak CPU and GPU temps.
**Any platform:** Any decent monitoring overlay works (MSI Afterburner + RivaTuner for GPU during games).

**What to look for:**
- CPU sustained above ~90-95°C on most consumer chips (varies by CPU — some Intel/AMD chips are designed to run near their thermal limit under load by default, so pair this with checking if clock speed is also dropping)
- GPU sustained above ~83-87°C is high for most cards (some cards are rated higher — check the specific model if borderline)
- The real tell isn't just a high number, it's **clock speed dropping** at the same time the temperature is high — that's throttling. If HWMonitor/HWiNFO shows clock speed, have them check that too.

## CPU vs GPU bottleneck (stutter, unclear which)

**Windows:** Task Manager → Performance tab (or Resource Monitor for more detail), or HWiNFO64 for more precision. Have them watch CPU usage and GPU usage simultaneously while the stutter happens.

**What to look for:**
- GPU usage near 100%, CPU well below → GPU-bound (GPU is the limiter)
- CPU usage near 100% on one or more cores while GPU sits lower → CPU-bound
- Neither maxed out but stutter still happens → look at storage or RAM instead, or check for background process spikes at the exact moment of stutter

## RAM / memory pressure

**Windows:** Task Manager → Performance → Memory tab, or Resource Monitor. Have them check total RAM usage during normal use, and specifically watch for a spike toward 100% right when the slowdown hits, plus high "Committed" memory relative to installed RAM.

**What to look for:**
- Usage regularly near 90-100% → likely genuine memory pressure, upgrade or close background apps
- Frequent hard page faults in Resource Monitor → system is paging to disk, which is slow and feels like general lag
- Also worth checking: Task Manager → Performance → Memory shows speed and number of channels in use — single-channel when dual-channel is possible is a real (and free-to-fix) performance loss

## Storage health/speed

**Windows:** CrystalDiskInfo (drive health/SMART status — free) and CrystalDiskMark (drive speed benchmark — free). 
**What to look for:**
- CrystalDiskInfo status anything other than "Good" → drive may be failing, back up data immediately
- CrystalDiskMark sequential/random read-write numbers far below the drive's rated spec (check the model's spec sheet) → misconfiguration (wrong PCIe mode, SATA vs NVMe confusion) or a drive that's degrading
- Also check: how full is the drive? SSDs slow down as they approach capacity — this alone explains a lot of "my SSD got slow" complaints

## Benchmarking new/underperforming components

**CPU:** Cinebench (R23 or 2024) — free, gives a single-core and multi-core score comparable against published results for that CPU model.
**GPU:** 3DMark (Time Spy for most GPUs, Steel Nomad or Speed Way for very high-end) — has a free version with enough for comparison. Or in-game benchmark tools if the user's main concern is a specific game.
**RAM speed:** Task Manager → Performance → Memory shows current running speed — compare against the RAM's rated speed printed on the module or its listing. If lower, XMP/EXPO likely isn't enabled in BIOS.

**What to do with the number:** Have the user search "[exact CPU/GPU model] Cinebench/3DMark score" or similar to find published benchmarks for that exact part, and compare. More than ~10-15% below typical published scores for the same part is worth investigating further (BIOS, drivers, thermals, power limits) rather than assuming it's just normal variance.

## Power delivery (less common, but worth knowing)

If a new/upgraded GPU underperforms and rules out the above, an undersized or degrading PSU can cause power limiting or instability under load. Signs: crashes/reboots under heavy load (not just slowdown), performance that recovers after a fresh boot but degrades again under sustained load. Harder to self-diagnose without a wattmeter — if suspected, the walkthrough should suggest checking the PSU wattage against the GPU's recommended PSU spec, and testing with non-essential peripherals unplugged.
