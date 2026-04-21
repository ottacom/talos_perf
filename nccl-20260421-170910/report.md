# NCCL benchmark report — 2026-04-21T17:11:02Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Intra-node (8 GPU, dgx052)

- Transport: `IBext_v8`
- Expected plateau: NVLink NV18 full mesh ~500-700 GB/s busbw

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.03 | 31.54 | **55.20** |
| 2.00 MiB | 0.04 | 51.39 | **89.93** |
| 4.00 MiB | 0.05 | 78.07 | **136.63** |
| 8.00 MiB | 0.08 | 107.82 | **188.68** |
| 16.00 MiB | 0.12 | 140.62 | **246.08** |
| 32.00 MiB | 0.20 | 167.62 | **293.34** |
| 64.00 MiB | 0.33 | 205.71 | **359.99** |
| 128.00 MiB | 0.59 | 226.16 | **395.79** |
| 256.00 MiB | 1.12 | 239.41 | **418.97** |
| 512.00 MiB | 2.16 | 248.33 | **434.58** |
| 1.00 GiB | 4.07 | 263.78 | **461.62** |
| 2.00 GiB | 8.01 | 268.00 | **469.01** |
| 4.00 GiB | 15.87 | 270.58 | **473.52** |
| 8.00 GiB | 31.48 | 272.84 | **477.46** |

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.31 | 3.33 | **6.25** |
| 2.00 MiB | 0.15 | 13.62 | **25.54** |
| 4.00 MiB | 0.22 | 18.79 | **35.24** |
| 8.00 MiB | 0.30 | 28.25 | **52.96** |
| 16.00 MiB | 0.47 | 35.88 | **67.28** |
| 32.00 MiB | 0.85 | 39.56 | **74.17** |
| 64.00 MiB | 1.61 | 41.64 | **78.07** |
| 128.00 MiB | 5.51 | 24.38 | **45.71** |
| 256.00 MiB | 10.68 | 25.13 | **47.11** |
| 512.00 MiB | 21.62 | 24.83 | **46.56** |
| 1.00 GiB | 42.20 | 25.44 | **47.71** |
| 2.00 GiB | 84.30 | 25.47 | **47.77** |
| 4.00 GiB | 172.40 | 24.91 | **46.71** |
| 8.00 GiB | 343.41 | 25.01 | **46.90** |

## Side-by-side busbw (larger = more bandwidth)

| Message | Intra busbw | Inter busbw | Ratio intra/inter |
|---:|---:|---:|---:|
| 1.00 MiB | 55.20 | 6.25 | 8.83x |
| 2.00 MiB | 89.93 | 25.54 | 3.52x |
| 4.00 MiB | 136.63 | 35.24 | 3.88x |
| 8.00 MiB | 188.68 | 52.96 | 3.56x |
| 16.00 MiB | 246.08 | 67.28 | 3.66x |
| 32.00 MiB | 293.34 | 74.17 | 3.95x |
| 64.00 MiB | 359.99 | 78.07 | 4.61x |
| 128.00 MiB | 395.79 | 45.71 | 8.66x |
| 256.00 MiB | 418.97 | 47.11 | 8.89x |
| 512.00 MiB | 434.58 | 46.56 | 9.33x |
| 1.00 GiB | 461.62 | 47.71 | 9.68x |
| 2.00 GiB | 469.01 | 47.77 | 9.82x |
| 4.00 GiB | 473.52 | 46.71 | 10.14x |
| 8.00 GiB | 477.46 | 46.90 | 10.18x |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

