# JCE Cryptonote CPU+GPU Miner
The Fastest Cryptonote CPU Miner ever, plus some tastes of GPU!

BitcoinTalk Topic: https://bitcointalk.org/index.php?topic=3281187.0

:heavy_exclamation_mark: Security Alert! Some hackers pack a Trojan in a fake JCE release, complete with the doc. :heavy_exclamation_mark: That's usually a small .rar when official JCE are big .zip\
Download JCE only from this Github page.

Here's the GPU prototype version, which still offers the CPU mode, plus a OpenCL GPU mode.\
While targetting AMD cards, it may work on nVidia too, i cannot test since I own zero nVidia card. To be tested.

# Index

* [Speed](#speed)
* [Warming-up](#warming-up)
* [Fees](#fees)
* [Configuration](#configuration)

## Speed

It's hard to compare GPU Miners. There are a lot of external parameters: the card itself, its memory, the biosmod, the drivers, the overclocking, the Power Limit...

However, accoring to the first feedbacks, here's the status:

* JCE is faster than any other miner on small RX cards (RX550 and RX560). They are my favorite cards, and the ones I use on mosyt of my rigs.
* JCE is always faster than the Wolf0-based miners (Stak, Xmrig, Gateless...). This is not fair since I can read their code and they cannot read mine, but it's a proof JCE is not a copy-paste of their code.
* JCE is surprisingly fast on Vega, reaching 2050+ on Vega 64 on CN-v7.
* JCE is close to SRB and Claymore otherwise, sometimes slightly above, sometime slighty below.
* JCE is disapointing on CN-Heavy, but that's the code I optimized the less so far.
* I got mixed results on HD7800. I measured higher hashrate than other miners on my rig, but got opposite comments from some users. To be tested.
* JCE is bad on small 1G cards compared to the legendary Claymore 9.7, but this miner is no longer supported.

## Warming-up

JCE lets the OpenCL driver allocate computing power progressively, and does not push the card at max immediately.\
**It starts at ~80% speed and grows up to 100% in about 5 minutes.** So let the miner warm up before comparing its speed to other miners.

## Fees

Current fees are:
* 0.9% on the GPUs
* 1.5% on the AES CPUs
* 3.0% on the non-AES CPUs

If you mix CPU and GPU, fees are adjusted proportionally.

## Configuration

**The prototype has no GPU autoconfig**, and so the --auto parameter will configure the same CPU threads than the CPU version, and no GPU threads.\
GPU autoconfig will come later. **The GPU can be enabled with manual config**, here's an example:

```
"cpu_threads_conf" : 
[ 
     { "cpu_architecture" : "auto", "affine_to_cpu" : 0, "use_cache" : true, "multi_hash":1 },
     { "cpu_architecture" : "auto", "affine_to_cpu" : 1, "use_cache" : true, "multi_hash":1 },
     { "cpu_architecture" : "auto", "affine_to_cpu" : 2, "use_cache" : true, "multi_hash":1 },
     { "cpu_architecture" : "auto", "affine_to_cpu" : 3, "use_cache" : true, "multi_hash":1 },
],

"gpu_threads_conf" : 
[ 
     { "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : 0, "multi_hash":464 },
     { "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : 0, "multi_hash":464 },
     { "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : 1, "multi_hash":208 },
     { "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : 1, "multi_hash":224 },
]
```

This is a real-life example from my pretty *exotic and old* rig:
* The CPU part is the exact same as in JCE CPU. I used a Core2-quad so i simply write the four normal threads.
* The GPU part is new:

*mode* : can be GPU and APU, but APU is not available yet, so keep GPU

*worksize* : exact same meaning than in other miners, that's the OpenCL parralelism level

*the greeks* : minor tuning values, in order of importance. Good values for alpha are 32, 64 or 128, and for beta 8 or 16. Other values will probably have no real impact, keep 8 or 4.

*index* : the GPU number. Run JCE with parameter --probe to get the list. It's in native OpenCL order, and GPU 0 is not always the main boot GPU.

*multi_hash* : often called intensity by other miners. Claymore parameter for it was -h. Like for CPU, that's the number of hashes computed at the same time. While CPUs go from 1 to 6, GPUs go much higher. Often the higher the faster, but not that simple. Must be a multiple of 16.

### Best known configurations

* HD7950 (Tahiti 3GB and more) Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
```
Twice the same to use the double-mem mode.


* HD7790 (Bonaire 1GB) Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":208 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":224 },
```
Two threads to use the double-mem mode, but the little 1GB VRAM doesn't allow to use 224+224 so I use 208+224.

* HD7870 (Pitcairn 2GB) Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 128, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 128, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
```
Twice the same to use the double-mem mode.

* RX560 (Baffin 2GB) Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 128, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 128, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta":4, "index" : ..., "multi_hash":464 },
```
Twice the same to use the double-mem mode. If a screen is plugged in the card, you may need to lower to 448 or 432

* RX570 8G Cryptonight v7
```
{ "mode": "GPU", "worksize": 4, "alpha": 128, "beta": 8, "gamma": 4, "delta": 4, "epsilon": 4, "zeta": 4, "index": ..., "multi_hash": 1008 },
{ "mode": "GPU", "worksize": 4, "alpha": 128, "beta": 8, "gamma": 4, "delta": 4, "epsilon": 4, "zeta": 4, "index": ..., "multi_hash": 1008 },
```
Twice the same to use the double-mem mode. If a screen is plugged in the card, you may need to lower to 448 or 432

* RX580 4G Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash": 944},
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash": 944}, 
```
Twice the same to use the double-mem mode.

* RX580 8G Cryptonight v7

```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1696 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1696 }, 
```
Twice the same to use the double-mem mode.

* RX580 8G Cryptonight v7

```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1696 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1696 }, 
```
Twice the same to use the double-mem mode.

* RX580 8G Cryptonight-Heavy/Haven

```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":832 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 8, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":832 }, 
```
Twice the same to use the double-mem mode.

* Vega 64 Cryptonight v7
```
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 16, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1920 },
{ "mode" : "GPU", "worksize" : 8, "alpha" : 64, "beta" : 16, "gamma" : 4, "delta" : 4, "epsilon" : 4, "zeta" : 4, "index" : ..., "multi_hash":1920 },
```
Twice the same to use the double-mem mode.
