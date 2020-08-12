Singularity file for Oxford Nanopore Technologies guppy gpu basecaller

Based off https://hub.docker.com/r/genomicpariscentre/guppy-gpu/dockerfile
(But I wanted to use Ubuntu 18.04 and a guppy gpu v. >= 4.0.11)

Image building handled by singularity-hub.org

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
- On the node, try `nvidia-smi` to confirm you can see the CUDA GPUs.
- Confirm the installed GPUs are [Compute Capability >= 6.0](https://developer.nvidia.com/cuda-gpus) (Somewhere Oxford Nanopore's help says that is the minimum version for guppy)
- Confirm the node installed Nvidia CUDA libraries are >= 418 (matching [what the container has](https://github.com/photocyte/guppy_gpu_singularity/blob/f4376d20ccbff97ea39909aad302887f028359ac/Singularity#L51))
