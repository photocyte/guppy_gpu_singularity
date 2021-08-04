Singularity file for Oxford Nanopore Technologies guppy gpu basecaller

Based off https://hub.docker.com/r/genomicpariscentre/guppy-gpu/dockerfile
(But I wanted to use Ubuntu 18.04 and a guppy gpu v. >= 4.0.11)

(2021-08-02 update: The source dockerfile above was updated for `CUDA 11.1`, `guppy-gpu 5.0.11`, and `Ubuntu 18.04`, so I would just use that!)

```
singularity pull docker://genomicpariscentre/guppy-gpu
singularity exec --nv guppy-gpu_latest.sif guppy_basecaller
```
---

Image building handled by singularity-hub.org

But: https://singularityhub.github.io/singularityhub-docs/
```
Notice
Singularity Hub is no longer online as a builder service, but exists as a read only archive. Containers built before April 19, 2021 are available at their same pull URLs.
```

### Usage

```
singularity pull shub://photocyte/guppy_gpu_singularity
singularity exec --nv guppy_gpu_singularity_latest.sif guppy_basecaller --help
```

See here for more details on the experimental `--nv` Nvidia CUDA support through Singularity: https://sylabs.io/guides/3.5/user-guide/gpu.html

Confirmed working on GPU nodes of the SDSC TSCC cluster (Centos 7.8.2003):
```
singularity exec --nv guppy_gpu_singularity_latest.sif guppy_basecaller -i fast5_pass/fast5_pass/ -s guppy_test -c /opt/ont/guppy/data/dna_r9.4.1_450bps_hac.cfg --device cuda:all:100% --num_callers 16 --gpu_runners_per_device 32 --chunks_per_caller 10000000 --read_batch_size 1000000
```
(Don't know if those parameters are optimal, but seems to go faster than the default)

### TSCC Troubleshooting

- Try running in interactive mode `qsub -I -V -q YOUR_GPU_QUEUE -A YOUR_GROUP -l nodes=A_BEEFY_GPU_NODE:ppn=16 -l walltime=6:00:00`, to be sure you are on a GPU node.  
- On the node, try `nvidia-smi; nvidia-smi -L` to confirm you can see the CUDA GPUs, and what type of GPUs they are.
- Confirm the node installed GPUs are [Compute Capability >= 6.1](https://developer.nvidia.com/cuda-gpus) (Somewhere Oxford Nanopore's help says that is the minimum version for guppy). E.g., the GeForce RTX 2080 Ti has a Compute Capability of 7.5
- Confirm via `nvidia-smi` that the node installed Nvidia CUDA libraries are >= 418 (matching [what the container is supposed to have](https://github.com/photocyte/guppy_gpu_singularity/blob/f4376d20ccbff97ea39909aad302887f028359ac/Singularity#L51))
- You can find GPU nodes with `pbsnodes -a` , look for the `gpu_status` field.
