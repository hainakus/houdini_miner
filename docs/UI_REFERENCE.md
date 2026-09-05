# Houdini UI Reference

The supplied Houdini images are the visual source of truth for the first dashboard iteration.

## Layout

- Left navigation rail with Houdini mark and mining sections.
- Large dashboard workspace with compact telemetry cards.
- GPU worker table as the primary operational surface.
- Pipeline visualization for DATA-GEN → PREFILTER → GEMM → CONSUMER.
- Runtime/telemetry panel for low-level miner diagnostics.

## Information hierarchy

### Level 1
Hashrate, accepted shares, pool difficulty, uptime and worker health.

### Level 2
GPU temperature, power, utilization, memory and per-worker hashrate.

### Level 3
Pipeline stage timings, queue depth, GEMM throughput, prefilter candidates and consumer state.

## Design goal

The web UI should eventually become a real-time control/observability surface backed by the Houdini miner rather than a separate marketing dashboard.
