# Snakemake executor plugin: pcluster-slurm v_0.0.7_

# Snakemake Executor Plugins (generally)
[Snakemake plugin catalog docs](https://snakemake.github.io/snakemake-plugin-catalog/plugins/executor).

## `pcluster-slurm` plugin
### AWS Parallel Cluster, `pcluster` `slurm`
[AWS Parallel Cluster](https://aws.amazon.com/hpc/parallelcluster/) is a framework to deploy and manage dynamically scalable HPC clusters on AWS, running SLURM as the batch system, and `pcluster` manages all of the creating, configuring, and deleting of the cluster compute nodes. Nodes may be spot or dedicated.  **note**, the `AWS Parallel Cluster` port of slurm has a few small, but critical differences from the standard slurm distribution.  This plugin enables using slurm from pcluster head and compute nodes via snakemake `>=v8.*`.

#### [Daylily Bfx Framework](https://github.com/Daylily-Informatics/daylily)
[Daylily](https://github.com/Daylily-Informatics/daylily) is a bioinformatics framework that automates and standardizes all aspects of creating a self-scaling ephemeral cluster which can grow from 1 head node to many thousands of as-needed compute spot instances (modulo your quotas and budget). This is accomplished by using [AWS Parallel Cluster](https://aws.amazon.com/hpc/parallelcluster/) to manage the cluster, and snakemake to manage the bfx workflows. In this context, `slurm` is the intermediary between snakemake and the cluster resource management. The `pcluster` slurm variant does not play nicely with vanilla slurm, and to date, the slurm snakemake executor has not worked with `pcluster` slurm. This plugin is a bridge between snakemake and `pcluster-slurm`.



# Pre-requisites
## Snakemake >=8.*
### Conda
```bash
conda create -n snakemake -c conda-forge -c bioconda snakemake==8.20.6
conda activate snakemake
```

# Installation (pip)
_from an environment with snakemake and pip installed_
```bash
pip install snakemake-executor-plugin-pcluster-slurm
```

# Example Usage [daylily cluster headnode](https://github.com/Daylily-Informatics/daylily)
```bash
mkdir -p /fsx/resources/environments/containers/ubuntu/cache/
export SNAKEMAKE_OUTPUT_CACHE=/fsx/resources/environments/containers/ubuntu/cache/
snakemake --use-conda --use-singularity -j 10  --singularity-prefix /fsx/resources/environments/containers/ubuntu/ip-10-0-0-240/ --singularity-args "  -B /tmp:/tmp -B /fsx:/fsx  -B /home/$USER:/home/$USER -B $PWD/:$PWD" --conda-prefix /fsx/resources/environments/containers/ubuntu/ip-10-0-0-240/ --executor pcluster-slurm --default-resources slurm_partition='i64,i128,i192' --cache  --verbose -k
```

# Other Cool Stuff
## Real Time Cost Tracking & Use Throttling via Budgets, Tagging ... and the `--comment` sbatch flag.
I etensively make use of  [Cost allocation tags with AWS ParallelCluster](https://github.com/Daylily-Informatics/aws-parallelcluster-cost-allocation-tags) in the [daylily omics ana\
lysis framework](https://github.com/Daylily-Informatics/daylily?tab=readme-ov-file#daylily-aws-ephemeral-cluster-setup-0714) _$3 30x WGS analysis_(https://github.com/Daylily-Inform\
atics/daylily?tab=readme-ov-file#3-30x-fastq-bam-bamdeduplicated-snvvcfsvvcf-add-035-for-a-raft-of-qc-reports)  to track AWS cluster usage costs in realtime, and impose limits wher\
e appropriate (by user and project). This makes use of overriding the `--comment` flag to hold `project/budget` tags applied to ephemeral AWS resources, and thus enabling cost trac\
king/controls.

* To change the	--comment flag in v`0.0.8` of the pcluster-slurm plugin, set the comment flag value in the envvar `SMK_SLURM_COMMENT=RandD` (RandD is the default).
