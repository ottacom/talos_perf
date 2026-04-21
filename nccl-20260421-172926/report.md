# NCCL benchmark report — 2026-04-21T17:30:14Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Intra-node (8 GPU, dgx052)

- Transport: `IBext_v8`
- Expected plateau: NVLink NV18 full mesh ~500-700 GB/s busbw

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.03 | 31.65 | **55.38** |
| 2.00 MiB | 0.04 | 51.19 | **89.58** |
| 4.00 MiB | 0.05 | 78.00 | **136.50** |
| 8.00 MiB | 0.08 | 108.08 | **189.14** |
| 16.00 MiB | 0.12 | 140.30 | **245.52** |
| 32.00 MiB | 0.20 | 167.97 | **293.95** |
| 64.00 MiB | 0.33 | 205.77 | **360.10** |
| 128.00 MiB | 0.59 | 226.38 | **396.16** |
| 256.00 MiB | 1.12 | 239.88 | **419.80** |
| 512.00 MiB | 2.16 | 248.41 | **434.72** |
| 1.00 GiB | 4.07 | 263.87 | **461.77** |
| 2.00 GiB | 8.02 | 267.86 | **468.75** |
| 4.00 GiB | 15.86 | 270.72 | **473.76** |
| 8.00 GiB | 31.48 | 272.88 | **477.54** |

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.22 | 4.72 | **8.85** |
| 2.00 MiB | 0.15 | 13.97 | **26.19** |
| 4.00 MiB | 0.20 | 21.00 | **39.38** |
| 8.00 MiB | 0.25 | 33.03 | **61.94** |
| 16.00 MiB | 0.31 | 54.40 | **101.99** |
| 32.00 MiB | 0.42 | 79.44 | **148.95** |
| 64.00 MiB | 0.53 | 126.37 | **236.95** |
| 128.00 MiB | 0.93 | 143.78 | **269.58** |
| 256.00 MiB | 1.52 | 176.60 | **331.12** |
| 512.00 MiB | 2.75 | 195.07 | **365.75** |
| 1.00 GiB | 5.02 | 213.89 | **401.05** |
| 2.00 GiB | 9.77 | 219.69 | **411.93** |
| 4.00 GiB | 18.73 | 229.36 | **430.05** |
| 8.00 GiB | 37.14 | 231.26 | **433.62** |

## Side-by-side busbw (larger = more bandwidth)

| Message | Intra busbw | Inter busbw | Ratio intra/inter |
|---:|---:|---:|---:|
| 1.00 MiB | 55.38 | 8.85 | 6.26x |
| 2.00 MiB | 89.58 | 26.19 | 3.42x |
| 4.00 MiB | 136.50 | 39.38 | 3.47x |
| 8.00 MiB | 189.14 | 61.94 | 3.05x |
| 16.00 MiB | 245.52 | 101.99 | 2.41x |
| 32.00 MiB | 293.95 | 148.95 | 1.97x |
| 64.00 MiB | 360.10 | 236.95 | 1.52x |
| 128.00 MiB | 396.16 | 269.58 | 1.47x |
| 256.00 MiB | 419.80 | 331.12 | 1.27x |
| 512.00 MiB | 434.72 | 365.75 | 1.19x |
| 1.00 GiB | 461.77 | 401.05 | 1.15x |
| 2.00 GiB | 468.75 | 411.93 | 1.14x |
| 4.00 GiB | 473.76 | 430.05 | 1.10x |
| 8.00 GiB | 477.54 | 433.62 | 1.10x |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

