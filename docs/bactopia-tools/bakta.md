---
title: bakta
description: A Bactopia Tool which uses Bakta to rapidly provide extensive annotations (tRNA, tmRNA, rRNA, ncRNA, CRISPR, CDS, pseudogenes, and sORFs) in a standardized fashion.
---
# Bactopia Tool - `bakta`
The `bakta` module uses [Bakta](https://github.com/oschwengers/bakta) to rapidly annotate bacterial 
genomes and plasmids in a standardized fashion. Bakta makes use of a large database ([40+ GB](https://doi.org/10.5281/zenodo.4247252))
to provide extensive annotations including: tRNA, tmRNA, rRNA, ncRNA, CRISPR, CDS, and sORFs.


## Example Usage
```
bactopia --wf bakta \
  --bactopia /path/to/your/bactopia/results \ 
  --include includes.txt  
```

## Output Overview

Below is the default output structure for the `bakta` tool. Where possible the 
file descriptions below were modified from a tools description.

```{bash}
<BACTOPIA_DIR>
├── <SAMPLE_NAME>
│   └── main
│       └── annotator
│           └── bakta
│               ├── <SAMPLE_NAME>-blastdb.tar.gz
│               ├── <SAMPLE_NAME>.embl.gz
│               ├── <SAMPLE_NAME>.faa.gz
│               ├── <SAMPLE_NAME>.ffn.gz
│               ├── <SAMPLE_NAME>.fna.gz
│               ├── <SAMPLE_NAME>.gbff.gz
│               ├── <SAMPLE_NAME>.gff3.gz
│               ├── <SAMPLE_NAME>.hypotheticals.faa.gz
│               ├── <SAMPLE_NAME>.hypotheticals.tsv
│               ├── <SAMPLE_NAME>.tsv
│               ├── <SAMPLE_NAME>.txt
│               └── logs
│                   ├── nf-bakta.{begin,err,log,out,run,sh,trace}
│                   └── versions.yml
└── bactopia-runs
    └── bakta-<TIMESTAMP>
        └── nf-reports
            ├── bakta-dag.dot
            ├── bakta-report.html
            ├── bakta-timeline.html
            └── bakta-trace.txt

```



### Results

#### Bakta

Below is a description of the _per-sample_ results from [Bakta](https://github.com/oschwengers/bakta).


| Extension                     | Description |
|-------------------------------|-------------|
| .blastdb.tar.gz | A gzipped tar archive of BLAST+ database of the contigs, genes, and proteins |
| .embl.gz | Annotations & sequences in (multi) EMBL format |
| .faa.gz | CDS/sORF amino acid sequences as FASTA |
| .ffn.gz | Feature nucleotide sequences as FASTA |
| .fna.gz | Replicon/contig DNA sequences as FASTA |
| .gbff.gz | Annotations & sequences in (multi) GenBank format |
| .gff3.gz | Annotations & sequences in GFF3 format |
| .hypotheticals.faa.gz | Hypothetical protein CDS amino acid sequences as FASTA |
| .hypotheticals.tsv | Further information on hypothetical protein CDS as simple human readable tab separated values |
| .tsv | Annotations as simple human readable tab separated values |
| .txt | Broad summary of `Bakta` annotations |





### Audit Trail

Below are files that can assist you in understanding which parameters and program versions were used.

#### Logs 

Each process that is executed will have a folder named `logs`. In this folder are helpful
files for you to review if the need ever arises.

| Extension    | Description |
|--------------|-------------|
| .begin       | An empty file used to designate the process started |
| .err         | Contains STDERR outputs from the process |
| .log         | Contains both STDERR and STDOUT outputs from the process |
| .out         | Contains STDOUT outputs from the process |
| .run         | The script Nextflow uses to stage/unstage files and queue processes based on given profile |
| .sh          | The script executed by bash for the process  |
| .trace       | The Nextflow [Trace](https://www.nextflow.io/docs/latest/tracing.html#trace-report) report for the process |
| versions.yml | A YAML formatted file with program versions |

#### Nextflow Reports

These Nextflow reports provide great a great summary of your run. These can be used to optimize
resource usage and estimate expected costs if using cloud platforms.

| Filename | Description |
|----------|-------------|
| bakta-dag.dot | The Nextflow [DAG visualisation](https://www.nextflow.io/docs/latest/tracing.html#dag-visualisation) |
| bakta-report.html | The Nextflow [Execution Report](https://www.nextflow.io/docs/latest/tracing.html#execution-report) |
| bakta-timeline.html | The Nextflow [Timeline Report](https://www.nextflow.io/docs/latest/tracing.html#timeline-report) |
| bakta-trace.txt | The Nextflow [Trace](https://www.nextflow.io/docs/latest/tracing.html#trace-report) report |


#### Program Versions

At the end of each run, each of the `versions.yml` files are merged into the files below.

| Filename                  | Description |
|---------------------------|-------------|
| software_versions.yml     | A complete list of programs and versions used by each process | 
| software_versions_mqc.yml | A complete list of programs and versions formatted for [MultiQC](https://multiqc.info/) |

## Parameters


### <i class="fa-xl fas fa-terminal"></i> Required Parameters
Define where the pipeline should find input data and save output data.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-bacterium"></i>` --bactopia` | The path to bactopia results to use as inputs <br/>**Type:** `string` |

### <i class="fa-xl fa-solid fa-filter"></i> Filtering Parameters
Use these parameters to specify which samples to include or exclude.

| Parameter | Description |
|:---|---|
| <i class="fa-lg far fa-square-plus"></i>` --include` | A text file containing sample names (one per line) to include from the analysis <br/>**Type:** `string` |
| <i class="fa-lg far fa-square-minus"></i>` --exclude` | A text file containing sample names (one per line) to exclude from the analysis <br/>**Type:** `string` |


### <i class="fa-xl fas fa-exclamation-circle"></i> Bakta Download Parameters


| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-file-alt"></i>` --bakta_db` | Tarball or path to the Bakta database <br/>**Type:** `string` |
| <i class="fa-lg fas fa-file-alt"></i>` --bakta_db_type` | Which Bakta DB to download 'full' (~30GB) or 'light' (~2GB) <br/>**Type:** `string`, **Default:** `full` |
| <i class="fa-lg fas fa-file-alt"></i>` --bakta_save_as_tarball` | Save the Bakta database as a tarball <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --download_bakta` | Download the Bakta database to the path given by --bakta_db <br/>**Type:** `boolean` |

### <i class="fa-xl fas fa-exclamation-circle"></i> Bakta Parameters


| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-file-alt"></i>` --proteins` | FASTA file of trusted proteins to first annotate from <br/>**Type:** `string` |
| <i class="fa-lg fas fa-file-alt"></i>` --prodigal_tf` | Training file to use for Prodigal <br/>**Type:** `string` |
| <i class="fa-lg fas fa-file-alt"></i>` --replicons` | Replicon information table (tsv/csv) <br/>**Type:** `string` |
| <i class="fa-lg fas fa-hashtag"></i>` --min_contig_length` | Minimum contig size to annotate <br/>**Type:** `integer`, **Default:** `1` |
| <i class="fa-lg fas fa-italic"></i>` --keep_contig_headers` | Keep original contig headers <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --compliant` | Force Genbank/ENA/DDJB compliance <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_trna` | Skip tRNA detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_tmrna` | Skip tmRNA detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_rrna` | Skip rRNA detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_ncrna` | Skip ncRNA detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_ncrna_region` | Skip ncRNA region detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_crispr` | Skip CRISPR array detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_cds` | Skip CDS detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_sorf` | Skip sORF detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_gap` | Skip gap detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --skip_ori` | Skip oriC/oriT detection & annotation <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-italic"></i>` --bakta_opts` | Extra Backa options in quotes. Example: '--gram +' <br/>**Type:** `string` |


### <i class="fa-xl fa-solid fa-gears"></i> Optional Parameters
These optional parameters can be useful in certain settings.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-folder"></i>` --outdir` | Base directory to write results to <br/>**Type:** `string`, **Default:** `./` |
| <i class="fa-lg fas fa-folder"></i>` --run_name` | Name of the directory to hold results <br/>**Type:** `string`, **Default:** `bactopia` |
| <i class="fa-lg fas fa-expand-arrows-alt"></i>` --skip_compression` | Ouput files will not be compressed <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-folder"></i>` --datasets` | The path to cache datasets to <br/>**Type:** `string` |
| <i class="fa-lg fas fa-trash-restore"></i>` --keep_all_files` | Keeps all analysis files created <br/>**Type:** `boolean` |

### <i class="fa-xl fa-solid fa-arrow-up-right-dots"></i> Max Job Request Parameters
Set the top limit for requested resources for any single job.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-redo"></i>` --max_retry` | Maximum times to retry a process before allowing it to fail. <br/>**Type:** `integer`, **Default:** `3` |
| <i class="fa-lg fas fa-microchip"></i>` --max_cpus` | Maximum number of CPUs that can be requested for any single job. <br/>**Type:** `integer`, **Default:** `4` |
| <i class="fa-lg fas fa-memory"></i>` --max_memory` | Maximum amount of memory (in GB) that can be requested for any single job. <br/>**Type:** `integer`, **Default:** `32` |
| <i class="fa-lg far fa-clock"></i>` --max_time` | Maximum amount of time (in minutes) that can be requested for any single job. <br/>**Type:** `integer`, **Default:** `120` |
| <i class="fa-lg fas fa-angle-double-up"></i>` --max_downloads` | Maximum number of samples to download at a time <br/>**Type:** `integer`, **Default:** `3` |

### <i class="fa-xl fa-solid fa-screwdriver-wrench"></i> Nextflow Configuration Parameters
Parameters to fine-tune your Nextflow setup.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-cog"></i>` --nfconfig` | A Nextflow compatible config file for custom profiles, loaded last and will overwrite existing variables if set. <br/>**Type:** `string` |
| <i class="fa-lg fas fa-copy"></i>` --publish_dir_mode` | Method used to save pipeline results to output directory. <br/>**Type:** `string`, **Default:** `copy` |
| <i class="fa-lg fas fa-cogs"></i>` --infodir` | Directory to keep pipeline Nextflow logs and reports. <br/>**Type:** `string`, **Default:** `${params.outdir}/pipeline_info` |
| <i class="fa-lg fas fa-recycle"></i>` --force` | Nextflow will overwrite existing output files. <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-trash-alt"></i>` --cleanup_workdir` | After Bactopia is successfully executed, the `work` directory will be deleted. <br/>**Type:** `boolean` |

### <i class="fa-xl fa-regular fa-address-card"></i> Nextflow Profile Parameters
Parameters to fine-tune your Nextflow setup.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-folder"></i>` --condadir` | Directory to Nextflow should use for Conda environments <br/>**Type:** `string` |
| <i class="fa-lg fas fa-box"></i>` --registry` | Docker registry to pull containers from. <br/>**Type:** `string`, **Default:** `dockerhub` |
| <i class="fa-lg fas fa-folder"></i>` --datasets_cache` | Directory where downloaded datasets should be stored. <br/>**Type:** `string`, **Default:** `<BACTOPIA_DIR>/data/datasets` |
| <i class="fa-lg fas fa-folder"></i>` --singularity_cache` | Directory where remote Singularity images are stored. <br/>**Type:** `string` |
| <i class="fa-lg fas fa-toolbox"></i>` --singularity_pull_docker_container` | Instead of directly downloading Singularity images for use with Singularity, force the workflow to pull and convert Docker containers instead. <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-recycle"></i>` --force_rebuild` | Force overwrite of existing pre-built environments. <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-clipboard-list"></i>` --queue` | Comma-separated name of the queue(s) to be used by a job scheduler (e.g. AWS Batch or SLURM) <br/>**Type:** `string`, **Default:** `general,high-memory` |
| <i class="fa-lg fas fa-clipboard-list"></i>` --cluster_opts` | Additional options to pass to the executor. (e.g. SLURM: '--account=my_acct_name' <br/>**Type:** `string` |
| <i class="fa-lg fas fa-toggle-off"></i>` --disable_scratch` | All intermediate files created on worker nodes of will be transferred to the head node. <br/>**Type:** `boolean` |

### <i class="fa-xl fa-solid fa-reply-all"></i> Helpful Parameters
Uncommonly used parameters that might be useful.

| Parameter | Description |
|:---|---|
| <i class="fa-lg fas fa-palette"></i>` --monochrome_logs` | Do not use coloured log outputs. <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-remove-format"></i>` --nfdir` | Print directory Nextflow has pulled Bactopia to <br/>**Type:** `boolean` |
| <i class="fa-lg far fa-clock"></i>` --sleep_time` | The amount of time (seconds) Nextflow will wait after setting up datasets before execution. <br/>**Type:** `integer`, **Default:** `5` |
| <i class="fa-lg fas fa-tasks"></i>` --validate_params` | Boolean whether to validate parameters against the schema at runtime <br/>**Type:** `boolean`, **Default:** `True` |
| <i class="fa-lg fas fa-question-circle"></i>` --help` | Display help text. <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-bacteria"></i>` --wf` | Specify which workflow or Bactopia Tool to execute <br/>**Type:** `string`, **Default:** `bactopia` |
| <i class="fa-lg fas fa-list"></i>` --list_wfs` | List the available workflows and Bactopia Tools to use with '--wf' <br/>**Type:** `boolean` |
| <i class="fa-lg far fa-eye"></i>` --show_hidden_params` | Show all params when using `--help` <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-question-circle"></i>` --help_all` | An alias for --help --show_hidden_params <br/>**Type:** `boolean` |
| <i class="fa-lg fas fa-info"></i>` --version` | Display version text. <br/>**Type:** `boolean` |

## Citations
If you use Bactopia and `bakta` in your analysis, please cite the following.

- [Bactopia](https://bactopia.github.io/)  
    Petit III RA, Read TD [Bactopia - a flexible pipeline for complete analysis of bacterial genomes.](https://doi.org/10.1128/mSystems.00190-20) _mSystems_ 5 (2020)
  

- [Bakta](https://github.com/oschwengers/bakta)  
    Schwengers O, Jelonek L, Dieckmann MA, Beyvers S, Blom J, Goesmann A [Bakta - rapid and standardized annotation of bacterial genomes via alignment-free sequence identification.](https://doi.org/10.1099/mgen.0.000685) _Microbial Genomics_ 7(11) (2021)
  
