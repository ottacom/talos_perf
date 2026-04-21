# NCCL benchmark report

| Field | Value |
|---|---|
| Generated | 2026-04-21T18:09:28Z |
| Cluster | `symphony-gpu` |
| Talos version | `v1.12.6` |
| Kubernetes | `` |
| Nodes | 5 total, 2 GPU nodes |

## Executive summary

| Metric | Value |
|---|---:|
| Peak busbw (intra-node, NVLink/NVSwitch) | **477.82 GB/s** |
| Peak busbw (inter-node, InfiniBand) | **159.44 GB/s** |
| Ratio intra / inter | **3.00x** |
| NCCL version | `2.22.3+cuda12.6` |
| IB NICs detected (inter-node) | 11 |

## Intra-node results (8 GPUs on dgx052)

- Transport: `IBext_v8`
- Fabric: NVLink 4th gen + NVSwitch (NV18 full mesh)
- Theoretical ceiling: ~900 GB/s per GPU (SXM5 peer-to-peer)
- Target busbw plateau: 500-700 GB/s for NCCL all-reduce

Bandwidth curve (busbw per message size):
```
▁▂▂▃▄▅▆▆▇▇▇▇▇█
```

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.05 | 22.88 | **40.04** |
| 2.00 MiB | 0.04 | 51.06 | **89.36** |
| 4.00 MiB | 0.05 | 77.57 | **135.74** |
| 8.00 MiB | 0.08 | 107.72 | **188.51** |
| 16.00 MiB | 0.12 | 137.43 | **240.50** |
| 32.00 MiB | 0.20 | 167.90 | **293.83** |
| 64.00 MiB | 0.33 | 205.67 | **359.92** |
| 128.00 MiB | 0.59 | 226.54 | **396.44** |
| 256.00 MiB | 1.12 | 238.63 | **417.60** |
| 512.00 MiB | 2.16 | 247.99 | **433.98** |
| 1.00 GiB | 4.07 | 263.54 | **461.19** |
| 2.00 GiB | 8.02 | 267.86 | **468.75** |
| 4.00 GiB | 15.85 | 270.91 | **474.10** |
| 8.00 GiB | 31.46 | 273.04 | **477.82** |

## Inter-node results (16 GPUs, dgx052 + dgx053)

- Transport: `IBext_v8`
- IB NICs used: 11
- Fabric: InfiniBand (ConnectX-7, aggregated 400 Gbps/NIC)
- Target busbw plateau (no SHARP): 100-150 GB/s
- Target busbw plateau (ACS disabled, no SHARP): 300-450 GB/s
- Target busbw plateau (SHARP enabled on switch): 400-500 GB/s

Bandwidth curve (busbw per message size):
```
▁▂▃▄▅▆▆▆▇▇▇▇█▇
```

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.21 | 5.10 | **9.57** |
| 2.00 MiB | 0.14 | 15.37 | **28.82** |
| 4.00 MiB | 0.16 | 26.89 | **50.41** |
| 8.00 MiB | 0.21 | 40.70 | **76.31** |
| 16.00 MiB | 0.30 | 55.81 | **104.64** |
| 32.00 MiB | 0.52 | 64.22 | **120.40** |
| 64.00 MiB | 0.95 | 70.76 | **132.68** |
| 128.00 MiB | 1.88 | 71.45 | **133.97** |
| 256.00 MiB | 3.42 | 78.55 | **147.29** |
| 512.00 MiB | 6.63 | 80.96 | **151.81** |
| 1.00 GiB | 12.82 | 83.73 | **157.00** |
| 2.00 GiB | 25.83 | 83.14 | **155.90** |
| 4.00 GiB | 50.51 | 85.04 | **159.44** |
| 8.00 GiB | 101.96 | 84.25 | **157.96** |

## Side-by-side bus bandwidth

| Message | Intra busbw GB/s | Inter busbw GB/s | Ratio |
|---:|---:|---:|---:|
| 1.00 MiB | 40.04 | 9.57 | 4.18x |
| 2.00 MiB | 89.36 | 28.82 | 3.10x |
| 4.00 MiB | 135.74 | 50.41 | 2.69x |
| 8.00 MiB | 188.51 | 76.31 | 2.47x |
| 16.00 MiB | 240.50 | 104.64 | 2.30x |
| 32.00 MiB | 293.83 | 120.40 | 2.44x |
| 64.00 MiB | 359.92 | 132.68 | 2.71x |
| 128.00 MiB | 396.44 | 133.97 | 2.96x |
| 256.00 MiB | 417.60 | 147.29 | 2.84x |
| 512.00 MiB | 433.98 | 151.81 | 2.86x |
| 1.00 GiB | 461.19 | 157.00 | 2.94x |
| 2.00 GiB | 468.75 | 155.90 | 3.01x |
| 4.00 GiB | 474.10 | 159.44 | 2.97x |
| 8.00 GiB | 477.82 | 157.96 | 3.02x |

## Performance assessment

Automated evaluation based on expected thresholds for DGX H100 fabric.

### Intra-node (NVLink)

- Peak busbw: **477.82 GB/s**
- Verdict: HEALTHY — NVLink/NVSwitch operating near theoretical.

### Inter-node (InfiniBand)

- Peak busbw: **159.44 GB/s**
- Verdict: BOTTLENECKED — traffic likely routed through CPU root complex. Check PCIe ACS (disable it), GPUDirect RDMA.

## Key indicators from NCCL logs

These confirm the data path taken by NCCL:

| Signal | Occurrences in inter-node log | Meaning |
|---|---:|---|
| `GDRDMA` | 0
0 | GPUDirect RDMA active (GPU↔NIC direct). GOOD if > 0 |
| `P2P/CUMEM` or `NVL` | 224 | Intra-node NVLink P2P paths. GOOD if ~equal to inter-node GDRDMA |
| `NET/Socket` | 0
0 | TCP fallback. BAD if > 0 (should be 0 with working IB) |
| `IBext` plugin | 440 | NVIDIA IB extension for NCCL. GOOD if > 0 |
| `CollNet` setup | 131 | SHARP CollNet attempted. Harmless if 0 (no SHARP) |
| `SHARP` mentions | 45 | SHARP-related log lines (detection in NIC != active use) |

## Recommendations

Based on the measurements, in order of expected impact:

1. **If inter-node busbw < 100 GB/s**: almost certainly PCIe ACS is enabled,
   forcing GPU↔NIC traffic through the CPU root complex. Apply the
   `manifests/disable-pcie-acs.yaml` DaemonSet — expected 3-5x jump.

2. **If inter-node busbw around 400 GB/s, no SHARP**: you are at the
   practical ceiling of this fabric without switch-side aggregation. To
   push further ask the fabric team to enable SHARPv3 (Quantum-2 switches)
   and set `NCCL_COLLNET_ENABLE=1`. Expected extra +5-15%.

3. **If intra-node < 400 GB/s**: check Fabric Manager is Running on every
   DGX, confirm 4× NVSwitch nodes in `/dev`, and that NVLS is enabled.

4. **Tuning env vars already applied** (from `nccl-tests-2node-maxperf.yaml`
   with fallback to `nccl-tests-2node-tuned.yaml`):
   `NCCL_IB_QPS_PER_CONNECTION=8`, `NCCL_IB_SPLIT_DATA_ON_QPS=1`,
   `NCCL_IB_TC=106`, `NCCL_IB_PCI_RELAXED_ORDERING=1`, `NCCL_CROSS_NIC=2`,
   `NCCL_NET_GDR_LEVEL=PIX`, `NCCL_NVLS_ENABLE=1`, `NCCL_CUMEM_ENABLE=1`,
   `NCCL_BUFFSIZE=16777216`, `NCCL_NTHREADS=512`. The `maxperf` manifest
   additionally exposes `USE_SHARP=1` (SHARP/CollNet + SAT).

5. **Firmware**: verify ConnectX-7 firmware version ≥ 28.39 for latest
   GDR optimizations. Older firmware can lose 5-10% at large messages.

6. **Link rate**: each IB port should negotiate 400 Gb/sec (NDR). HDR
   (200 Gb/sec) cables or firmware mismatches halve throughput silently.
   Verify via `cat /sys/class/infiniband/mlx5_0/ports/1/rate`.

