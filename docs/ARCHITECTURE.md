# Houdini UI Architecture

The UI is intentionally separated from mining execution so it can later consume a local telemetry/API endpoint without changing the CUDA/Rust core.

```text
Houdini Miner
    │
    ├── Stratum / Pool
    ├── Scheduler
    ├── Producer / Data-gen
    ├── Prefilter
    ├── HOU-GEMM Consumer
    └── Share / Telemetry
             │
             ▼
       Local API / IPC
             │
             ▼
       Houdini Dashboard
```

## Future API contract

Suggested endpoints:

- `GET /api/status`
- `GET /api/workers`
- `GET /api/pool`
- `GET /api/pipeline`
- `GET /api/telemetry`
- `GET /api/logs`

The initial dashboard uses deterministic demo data so it can be developed independently of the miner runtime.
