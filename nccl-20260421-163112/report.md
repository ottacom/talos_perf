# NCCL benchmark report — 2026-04-21T16:31:50Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.23 | 4.63 | **8.68** |
| 2.00 MiB | 0.15 | 13.67 | **25.62** |
| 4.00 MiB | 0.21 | 19.88 | **37.27** |
| 8.00 MiB | 0.29 | 28.82 | **54.04** |
| 16.00 MiB | 0.48 | 35.31 | **66.21** |
| 32.00 MiB | 0.84 | 39.83 | **74.69** |
| 64.00 MiB | 1.61 | 41.60 | **78.00** |
| 128.00 MiB | 3.08 | 43.63 | **81.80** |
| 256.00 MiB | 5.96 | 45.03 | **84.43** |
| 512.00 MiB | 11.74 | 45.72 | **85.73** |
| 1.00 GiB | 23.24 | 46.21 | **86.64** |
| 2.00 GiB | 46.20 | 46.48 | **87.16** |
| 4.00 GiB | 92.14 | 46.62 | **87.40** |
| 8.00 GiB | 183.94 | 46.70 | **87.56** |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

