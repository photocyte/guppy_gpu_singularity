Singularity file for Oxford Nanopore Technologies guppy gpu basecaller

Based off https://hub.docker.com/r/genomicpariscentre/guppy-gpu/dockerfile
(But I wanted to use Ubuntu 18.04 and a guppy gpu v. >= 4.0.11)

Image building handled by singularity-hub.org

### Usage

```
singularity pull shub://photocyte/guppy_gpu_singularity
singularity exec --nv genometools_singularity_latest.sif guppy_basecaller --help
```

See here for more details on the experimental `--nv` Nvidia CUDA support through Singularity: https://sylabs.io/guides/3.5/user-guide/gpu.html
