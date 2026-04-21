# NCCL benchmark report — 2026-04-21T17:28:12Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.22 | 4.86 | **9.12** |
| 2.00 MiB | 0.15 | 14.06 | **26.35** |
| 4.00 MiB | 0.20 | 21.08 | **39.53** |
| 8.00 MiB | 0.25 | 34.05 | **63.84** |
| 16.00 MiB | 0.31 | 54.74 | **102.63** |
| 32.00 MiB | 0.42 | 80.26 | **150.48** |
| 64.00 MiB | 0.52 | 129.29 | **242.42** |
| 128.00 MiB | 0.87 | 154.10 | **288.94** |
| 256.00 MiB | 1.47 | 182.15 | **341.54** |
| 512.00 MiB | 2.62 | 204.56 | **383.54** |
| 1.00 GiB | 4.93 | 217.66 | **408.11** |
| 2.00 GiB | 9.53 | 225.25 | **422.34** |
| 4.00 GiB | 18.66 | 230.16 | **431.56** |
| 8.00 GiB | 37.05 | 231.88 | **434.77** |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

