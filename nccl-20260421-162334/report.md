# NCCL benchmark report — 2026-04-21T16:24:31Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Intra-node (8 GPU, dgx052)

- Transport: `IBext_v8`
- Expected plateau: NVLink NV18 full mesh ~500-700 GB/s busbw

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.04 | 27.94 | **48.90** |
| 2.00 MiB | 0.04 | 51.07 | **89.37** |
| 4.00 MiB | 0.05 | 78.11 | **136.69** |
| 8.00 MiB | 0.08 | 107.87 | **188.78** |
| 16.00 MiB | 0.12 | 140.69 | **246.21** |
| 32.00 MiB | 0.20 | 168.04 | **294.07** |
| 64.00 MiB | 0.33 | 205.58 | **359.76** |
| 128.00 MiB | 0.59 | 226.69 | **396.70** |
| 256.00 MiB | 1.12 | 238.89 | **418.06** |
| 512.00 MiB | 2.16 | 248.20 | **434.35** |
| 1.00 GiB | 4.07 | 263.90 | **461.83** |
| 2.00 GiB | 8.02 | 267.78 | **468.61** |
| 4.00 GiB | 15.87 | 270.66 | **473.66** |
| 8.00 GiB | 31.46 | 273.03 | **477.80** |

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.21 | 5.08 | **9.52** |
| 2.00 MiB | 0.15 | 13.84 | **25.96** |
| 4.00 MiB | 0.21 | 19.95 | **37.40** |
| 8.00 MiB | 0.30 | 28.36 | **53.17** |
| 16.00 MiB | 0.48 | 35.25 | **66.09** |
| 32.00 MiB | 0.84 | 39.76 | **74.55** |
| 64.00 MiB | 1.61 | 41.66 | **78.11** |
| 128.00 MiB | 3.08 | 43.63 | **81.80** |
| 256.00 MiB | 5.96 | 45.03 | **84.43** |
| 512.00 MiB | 11.73 | 45.77 | **85.82** |
| 1.00 GiB | 23.26 | 46.17 | **86.57** |
| 2.00 GiB | 46.44 | 46.24 | **86.70** |
| 4.00 GiB | 92.13 | 46.62 | **87.41** |
| 8.00 GiB | 184.17 | 46.64 | **87.45** |

## Side-by-side busbw (larger = more bandwidth)

| Message | Intra busbw | Inter busbw | Ratio intra/inter |
|---:|---:|---:|---:|
| 1.00 MiB | 48.90 | 9.52 | 5.14x |
| 2.00 MiB | 89.37 | 25.96 | 3.44x |
| 4.00 MiB | 136.69 | 37.40 | 3.65x |
| 8.00 MiB | 188.78 | 53.17 | 3.55x |
| 16.00 MiB | 246.21 | 66.09 | 3.73x |
| 32.00 MiB | 294.07 | 74.55 | 3.94x |
| 64.00 MiB | 359.76 | 78.11 | 4.61x |
| 128.00 MiB | 396.70 | 81.80 | 4.85x |
| 256.00 MiB | 418.06 | 84.43 | 4.95x |
| 512.00 MiB | 434.35 | 85.82 | 5.06x |
| 1.00 GiB | 461.83 | 86.57 | 5.33x |
| 2.00 GiB | 468.61 | 86.70 | 5.40x |
| 4.00 GiB | 473.66 | 87.41 | 5.42x |
| 8.00 GiB | 477.80 | 87.45 | 5.46x |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

