# NCCL benchmark report — 2026-04-21T17:27:10Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.22 | 4.84 | **9.08** |
| 2.00 MiB | 0.14 | 14.58 | **27.34** |
| 4.00 MiB | 0.21 | 20.17 | **37.83** |
| 8.00 MiB | 0.23 | 35.86 | **67.24** |
| 16.00 MiB | 0.31 | 54.45 | **102.09** |
| 32.00 MiB | 0.45 | 74.87 | **140.39** |
| 64.00 MiB | 0.58 | 115.20 | **216.00** |
| 128.00 MiB | 0.88 | 151.91 | **284.83** |
| 256.00 MiB | 1.51 | 178.07 | **333.89** |
| 512.00 MiB | 2.67 | 200.80 | **376.50** |
| 1.00 GiB | 4.99 | 215.14 | **403.39** |
| 2.00 GiB | 9.65 | 222.58 | **417.33** |
| 4.00 GiB | 18.82 | 228.21 | **427.89** |
| 8.00 GiB | 37.08 | 231.64 | **434.32** |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

