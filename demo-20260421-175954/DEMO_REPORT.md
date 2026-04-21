# Talos DGX Cluster — Demo Report

Generated: **2026-04-21T18:00:50Z**
Cluster: **symphony-gpu**

## Headline

| | |
|---|---:|
| Total nodes | **5** |
| GPU nodes (DGX) | **2** |
| Total GPUs exposed to Kubernetes | **16** |
| Kubernetes | `v1.35.4` |
| Talos | `v1.12.6` |
| NCCL | `2.22.3+cuda12.6` |
| IB NICs detected (per DGX) | **11** |
| **Peak intra-node busbw (NVLink)** | **476.85 GB/s** |
| **Peak inter-node busbw (IB)** | **437.58 GB/s** |
| Ratio intra / inter | **1.09x** |

## Performance verdict

| Fabric | Peak busbw | Verdict |
|---|---:|---|
| Intra-node (NVLink/NVSwitch) | 476.85 GB/s | ✓ HEALTHY — NVLink near theoretical max |
| Inter-node (InfiniBand) | 437.58 GB/s | ✓ EXCELLENT — GDRDMA full throughput, ACS disabled |

## NCCL data path signals

From `nccl-internode.log`:

| Indicator | Count | Meaning |
|---|---:|---|
| GDRDMA mentions | 392 | GPUDirect RDMA active (should be > 0) |
| NVLink P2P paths | 392 | NVLink intra-node channels |
| TCP fallback (NET/Socket) | 0
0 | Should be 0 (IB should win) |

## Intra-node all-reduce (8 GPUs)

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.07 | 15.61 | **27.32** |
| 2.00 MiB | 0.07 | 30.90 | **54.07** |
| 4.00 MiB | 0.07 | 61.08 | **106.90** |
| 8.00 MiB | 0.08 | 105.99 | **185.48** |
| 16.00 MiB | 0.12 | 137.47 | **240.57** |
| 32.00 MiB | 0.20 | 166.04 | **290.57** |
| 64.00 MiB | 0.33 | 204.35 | **357.62** |
| 128.00 MiB | 0.59 | 225.90 | **395.32** |
| 256.00 MiB | 1.12 | 239.31 | **418.80** |
| 512.00 MiB | 2.16 | 248.39 | **434.68** |
| 1.00 GiB | 4.07 | 263.94 | **461.90** |
| 2.00 GiB | 8.02 | 267.81 | **468.66** |
| 4.00 GiB | 15.86 | 270.77 | **473.85** |
| 8.00 GiB | 31.52 | 272.49 | **476.85** |

## Inter-node all-reduce (16 GPUs)

| Message | Avg ms | algbw GB/s | busbw GB/s |
|---:|---:|---:|---:|
| 1.00 MiB | 0.23 | 4.63 | **8.67** |
| 2.00 MiB | 0.15 | 14.31 | **26.83** |
| 4.00 MiB | 0.20 | 20.80 | **38.99** |
| 8.00 MiB | 0.23 | 36.94 | **69.26** |
| 16.00 MiB | 0.30 | 55.97 | **104.95** |
| 32.00 MiB | 0.42 | 79.53 | **149.12** |
| 64.00 MiB | 0.55 | 122.34 | **229.38** |
| 128.00 MiB | 0.86 | 156.75 | **293.90** |
| 256.00 MiB | 1.46 | 183.58 | **344.21** |
| 512.00 MiB | 2.61 | 205.41 | **385.14** |
| 1.00 GiB | 4.92 | 218.40 | **409.49** |
| 2.00 GiB | 9.57 | 224.40 | **420.75** |
| 4.00 GiB | 18.64 | 230.38 | **431.97** |
| 8.00 GiB | 36.81 | 233.38 | **437.58** |

## CUDA + NVLink topology (dgx052)

```
| NVIDIA-SMI 580.126.20             Driver Version: 580.126.20     CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA H100 80GB HBM3          On  |   00000000:1B:00.0 Off |                    0 |
| N/A   31C    P0             70W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
|   1  NVIDIA H100 80GB HBM3          On  |   00000000:43:00.0 Off |                    0 |
| N/A   31C    P0             71W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
|   2  NVIDIA H100 80GB HBM3          On  |   00000000:52:00.0 Off |                    0 |
| N/A   33C    P0             72W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
|   3  NVIDIA H100 80GB HBM3          On  |   00000000:61:00.0 Off |                    0 |
| N/A   32C    P0             68W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
|   4  NVIDIA H100 80GB HBM3          On  |   00000000:9D:00.0 Off |                    0 |
| N/A   31C    P0             72W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
|   5  NVIDIA H100 80GB HBM3          On  |   00000000:C3:00.0 Off |                    0 |
| N/A   29C    P0             69W /  700W |       0MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+

	[4mGPU0	GPU1	GPU2	GPU3	GPU4	GPU5	GPU6	GPU7	NIC0	NIC1	NIC2	NIC3	NIC4	NIC5	NIC6	NIC7	NIC8	NIC9	NIC10	NIC11	CPU Affinity	NUMA Affinity	GPU NUMA ID[0m
GPU0	 X 	NV18	NV18	NV18	NV18	NV18	NV18	NV18	PXB	NODE	NODE	NODE	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS	0-55,112-167	0		N/A
GPU1	NV18	 X 	NV18	NV18	NV18	NV18	NV18	NV18	NODE	NODE	NODE	PXB	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS	0-55,112-167	0		N/A
GPU2	NV18	NV18	 X 	NV18	NV18	NV18	NV18	NV18	NODE	NODE	NODE	NODE	PXB	NODE	SYS	SYS	SYS	SYS	SYS	SYS	0-55,112-167	0		N/A
GPU3	NV18	NV18	NV18	 X 	NV18	NV18	NV18	NV18	NODE	NODE	NODE	NODE	NODE	PXB	SYS	SYS	SYS	SYS	SYS	SYS	0-55,112-167	0		N/A
GPU4	NV18	NV18	NV18	NV18	 X 	NV18	NV18	NV18	SYS	SYS	SYS	SYS	SYS	SYS	PXB	NODE	NODE	NODE	NODE	NODE	56-111,168-223	1		N/A
GPU5	NV18	NV18	NV18	NV18	NV18	 X 	NV18	NV18	SYS	SYS	SYS	SYS	SYS	SYS	NODE	NODE	NODE	PXB	NODE	NODE	56-111,168-223	1		N/A
GPU6	NV18	NV18	NV18	NV18	NV18	NV18	 X 	NV18	SYS	SYS	SYS	SYS	SYS	SYS	NODE	NODE	NODE	NODE	PXB	NODE	56-111,168-223	1		N/A
GPU7	NV18	NV18	NV18	NV18	NV18	NV18	NV18	 X 	SYS	SYS	SYS	SYS	SYS	SYS	NODE	NODE	NODE	NODE	NODE	PXB	56-111,168-223	1		N/A
NIC0	PXB	NODE	NODE	NODE	SYS	SYS	SYS	SYS	 X 	NODE	NODE	NODE	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS				
NIC1	NODE	NODE	NODE	NODE	SYS	SYS	SYS	SYS	NODE	 X 	PIX	NODE	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS				
NIC2	NODE	NODE	NODE	NODE	SYS	SYS	SYS	SYS	NODE	PIX	 X 	NODE	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS				
NIC3	NODE	PXB	NODE	NODE	SYS	SYS	SYS	SYS	NODE	NODE	NODE	 X 	NODE	NODE	SYS	SYS	SYS	SYS	SYS	SYS				
NIC4	NODE	NODE	PXB	NODE	SYS	SYS	SYS	SYS	NODE	NODE	NODE	NODE	 X 	NODE	SYS	SYS	SYS	SYS	SYS	SYS				
NIC5	NODE	NODE	NODE	PXB	SYS	SYS	SYS	SYS	NODE	NODE	NODE	NODE	NODE	 X 	SYS	SYS	SYS	SYS	SYS	SYS				
```

## Cluster details

```
=== Kubernetes server version ===
v1.35.4

=== Nodes ===
NAME      STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE          KERNEL-VERSION   CONTAINER-RUNTIME
dgx052    Ready    <none>          3h40m   v1.35.2   10.10.5.50    <none>        Talos (v1.12.6)   6.18.18-talos    containerd://2.1.6
dgx053    Ready    <none>          3h41m   v1.35.2   10.10.5.51    <none>        Talos (v1.12.6)   6.18.18-talos    containerd://2.1.6
k8s-m01   Ready    control-plane   6h24m   v1.35.2   10.10.5.11    <none>        Talos (v1.12.6)   6.18.18-talos    containerd://2.1.6
k8s-m02   Ready    control-plane   6h24m   v1.35.2   10.10.5.12    <none>        Talos (v1.12.6)   6.18.18-talos    containerd://2.1.6
k8s-m03   Ready    control-plane   6h24m   v1.35.2   10.10.5.13    <none>        Talos (v1.12.6)   6.18.18-talos    containerd://2.1.6

=== Total GPUs advertised ===
dgx052: 8 GPUs, Talos (v1.12.6)
dgx053: 8 GPUs, Talos (v1.12.6)
k8s-m01: 0 GPUs, Talos (v1.12.6)
k8s-m02: 0 GPUs, Talos (v1.12.6)
k8s-m03: 0 GPUs, Talos (v1.12.6)

=== Control plane HA ===
10.10.5.11 10.10.5.12 10.10.5.13
=== Cilium status (if installed) ===
cilium-4klhl Running
cilium-ftfgr Running
cilium-rvs72 Running
cilium-sshkf Running
cilium-whxgw Running
```

## What this demo proves

- Talos Linux boots and runs Kubernetes on commodity VMs (control plane) and
  on bare-metal NVIDIA DGX H100 (worker nodes).
- NVIDIA open kernel modules (driver + container toolkit + Fabric Manager)
  load as Talos system extensions, zero post-install steps.
- NVLink/NVSwitch fabric intra-node delivers peak bus bandwidth approaching
  the silicon limit for H100 SXM5.
- InfiniBand inter-node with GPUDirect RDMA (ACS disabled) delivers near-
  line-rate bandwidth across multiple ConnectX-7 NICs.
- Any CUDA workload can be submitted as a standard Kubernetes Job referring
  to `runtimeClassName: nvidia` and requesting `nvidia.com/gpu: N`.
