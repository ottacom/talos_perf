# NCCL benchmark report — 2026-04-21T17:16:40Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.24 | 4.31 | **8.09** |
| 2.00 MiB | 0.15 | 13.88 | **26.02** |
| 4.00 MiB | 0.21 | 20.12 | **37.72** |
| 8.00 MiB | 0.29 | 28.55 | **53.54** |
| 16.00 MiB | 0.46 | 36.19 | **67.85** |
| 32.00 MiB | 0.85 | 39.64 | **74.33** |
| 64.00 MiB | 1.64 | 40.93 | **76.74** |
| 128.00 MiB | 3.07 | 43.66 | **81.86** |
| 256.00 MiB | 5.95 | 45.08 | **84.53** |
| 512.00 MiB | 11.74 | 45.72 | **85.73** |
| 1.00 GiB | 23.24 | 46.19 | **86.61** |
| 2.00 GiB | 46.21 | 46.47 | **87.13** |
| 4.00 GiB | 92.13 | 46.62 | **87.41** |
| 8.00 GiB | 183.96 | 46.70 | **87.55** |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

