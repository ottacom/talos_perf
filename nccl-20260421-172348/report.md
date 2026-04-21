# NCCL benchmark report — 2026-04-21T17:24:14Z

Cluster: `symphony-gpu`  Talos: `v1.12.6`

## Inter-node (16 GPU, dgx052+dgx053)

- Transport: `IBext_v8`
- IB NICs detected: 11
- Expected plateau: IB-bound ~150-300 GB/s busbw with tuning

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.22 | 4.73 | **8.87** |
| 2.00 MiB | 0.17 | 12.03 | **22.56** |
| 4.00 MiB | 0.20 | 20.82 | **39.03** |
| 8.00 MiB | 0.24 | 35.48 | **66.52** |
| 16.00 MiB | 0.31 | 54.09 | **101.42** |
| 32.00 MiB | 0.46 | 72.84 | **136.57** |
| 64.00 MiB | 0.57 | 117.49 | **220.28** |
| 128.00 MiB | 0.87 | 153.52 | **287.85** |
| 256.00 MiB | 1.54 | 174.12 | **326.48** |
| 512.00 MiB | 2.69 | 199.45 | **373.96** |
| 1.00 GiB | 5.00 | 214.61 | **402.40** |
| 2.00 GiB | 9.71 | 221.06 | **414.49** |
| 4.00 GiB | 18.73 | 229.34 | **430.01** |
| 8.00 GiB | 37.01 | 232.09 | **435.18** |

## Key observations

- **NVLink peak**: max busbw intra-nodo visto dall'ultimo row
- **IB peak**: max busbw inter-nodo visto dall'ultimo row
- **Ratio > 2x**: conferma che IB è il collo di bottiglia multi-nodo (atteso)
- **Ratio ~= 1x**: problema — NCCL sta facendo fallback a socket o altro

