# Houdini

Houdini is a GPU-optimized miner for the Pearl proof-of-work algorithm. It is designed for miners who want a fast, stable, and easy-to-deploy solution for connecting to Pearl-compatible mining pools via the Stratum protocol.



## Getting started

1. Download the latest release package for your platform.
2. Configure your pool, worker, and wallet.
3. Start mining.

For build instructions, configuration details, and advanced options, see the repository documentation.

## HiveOS Flight Sheet – Houdini HOU-GEMM User Configuration

This document describes the recommended flight-sheet configuration for running the **Houdini HOU-GEMM** backend on HiveOS.

### Creating the flight sheet

1. Open the HiveOS web interface.
2. Go to **Flight Sheets** and click **Add Flight Sheet**.
3. Select the appropriate coin and wallet.
4. Select your pool (URL + port).
5. Choose the **Houdini** custom miner.
6. In the **Worker** field use: `WALLET.worker`
7. In the **Password** field use: `x` or the value required by your pool.

### Adding the PEARL configuration

Paste the following block into the flight sheet **Extra config arguments** field, or add it to `h-config.sh` on the rig:

```text
--PEARL_HOU_GEMM_SYNC_MODE=blocking
--PEARL_CUDA_BLOCKING_SYNC=1
--RUST_LOG=info
--PEARL_FORCE_PROOF_GEOMETRY=1
--PEARL_PROOF_M=196608
--PEARL_PROOF_N=196608
--PEARL_PROOF_K=2048
--PEARL_PROOF_RANK=128
--PEARL_SINGLE_WORKER=0
--PEARL_SKIP_CPU_VERIFY=1
--PEARL_VERIFY_QUEUE_TTL_MS=120000
--PEARL_NONCE_RANGE_CHECK_WINDOW=1024
--PEARL_CONSUMER_KERNEL_WS=1
--PEARL_FAST_PREFILTER=0
--PEARL_HOU_GEMM_INLINE_PREFILTER_MATERIALIZE=1
--PEARL_HOU_GEMM_CONSUMER_SLOTS=1
--PEARL_HOU_GEMM_BATCH_VRAM_TARGET_MB=4096
--PEARL_HOU_GEMM_BATCH_CAPACITY=1
--PEARL_HOU_GEMM_AUTO_TUNE=0
--PEARL_NO_ZK=1
--PEARL_ENABLE_ZK=0
--PEARL_RAYON_THREADS=4
--PEARL_SUBMIT_HS=1
--tui
```

## Configuration reference

| Variable | Purpose |
| --- | --- |
| `PEARL_HOU_GEMM_SYNC_MODE` | CUDA synchronization mode (`blocking` for stable timing). |
| `PEARL_CUDA_BLOCKING_SYNC` | Enables blocking CUDA stream synchronization. |
| `RUST_LOG` | Log level (`info`, `warn`, `debug`). |
| `PEARL_FORCE_PROOF_GEOMETRY` | Force the fallback proof geometry. |
| `PEARL_PROOF_M` | Fallback proof matrix rows. |
| `PEARL_PROOF_N` | Fallback proof matrix columns. |
| `PEARL_PROOF_K` | Fallback proof common dimension. |
| `PEARL_PROOF_RANK` | Fallback noise rank. |
| `PEARL_SINGLE_WORKER` | Single worker mode. |
| `PEARL_SKIP_CPU_VERIFY` | Skip CPU-side share verification. |
| `PEARL_VERIFY_QUEUE_TTL_MS` | Max age of verified shares before submission. |
| `PEARL_NONCE_RANGE_CHECK_WINDOW` | Nonce range check window size. |
| `PEARL_CONSUMER_KERNEL_WS` | Consumer kernel work-stealing variant. |
| `PEARL_FAST_PREFILTER` | Fast prefilter mode toggle. |
| `PEARL_HOU_GEMM_INLINE_PREFILTER_MATERIALIZE` | Inline prefilter materialize in HOU-GEMM. |
| `PEARL_HOU_GEMM_CONSUMER_SLOTS` | Number of consumer slots. |
| `PEARL_HOU_GEMM_BATCH_VRAM_TARGET_MB` | Target VRAM for batches (MB). |
| `PEARL_HOU_GEMM_BATCH_CAPACITY` | Maximum batches in flight. |
| `PEARL_HOU_GEMM_AUTO_TUNE` | Auto-tune consumer slots (`0` off). |
| `PEARL_NO_ZK` / `PEARL_ENABLE_ZK` | Disable local ZK proving. |
| `PEARL_RAYON_THREADS` | Rayon CPU thread pool size. |
| `PEARL_SUBMIT_HS` | Submit pool hashrate field with shares. |

## Notes

- This configuration assumes the fallback proof geometry **m = 196608, n = 196608, k = 2048, rank = 128**.
- It disables the dev fee, disables local ZK proving, and runs a single consumer slot with blocking CUDA sync.
- Adjust `PEARL_HOU_GEMM_BATCH_VRAM_TARGET_MB` if your GPU has less than 8 GB of VRAM.
- For multi-GPU rigs, review `PEARL_SINGLE_WORKER` and the consumer-slot settings.

## Applying

After saving the flight sheet, apply it to your rig and wait for the miner to start. You can monitor it from the HiveOS dashboard or with:

```bash
tail -f /var/log/miner/custom/houdini/houdini.log
```
