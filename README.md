# JCE Cryptonote CPU+GPU Miner
The Fastest Cryptonote CPU Miner ever, plus some tastes of GPU!

BitcoinTalk Topic: https://bitcointalk.org/index.php?topic=3281187.0

:heavy_exclamation_mark: Security Alert! Some hackers pack a Trojan in a fake JCE release, complete with the doc. :heavy_exclamation_mark: That's usually a small .rar when official JCE are big .zip\
Download JCE only from this Github page.

Here's the GPU prototype version, which still offers the CPU mode, plus a OpenCL GPU mode.\
While targetting AMD cards, it may work on nVidia too, i cannot test since I own zero nVidia card. To be tested.

# Index

* [Configuration](#configuration)

## Configuration

The prototype has no GPU autoconfig, and so the --auto parameter will configure the same CPU threads than the CPU version, and no GPU threads.\
GPU autoconfig will come later. The GPU can be enabled with manual config, here's an example:

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
