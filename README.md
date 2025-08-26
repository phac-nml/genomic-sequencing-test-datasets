## Title: genomic-sequencing-test-datasets
**Purpose:** This series of test datasets have been prepared emulating real Illumina and Nanopore sequencing data. These datasets are designed for use in:
    
    a) Evaluating software changes prior to implementation
    
    b) Proficiency testing of personnel performing NGS analysis of SARS-CoV-2

**Note:** These test datasets are focused solely on the analysis and interpretation of sequence data and excludes contextual data. 

For information and test datasets on training and quality control of contextual data, please see [here.](https://github.com/cidgoh/DataHarmonizer/wiki/CanCOGeN-Contextual-Data-Datasets-for-Harmonization-Training-and-Quality-Control )
 This resource contains "Gold Standard", "Common Errors", and a "Scenarios dataset".

**Resources:**

**Limitations:**

- The current comparison script will only work on SIGNAL Illumina data [run from here](https://github.com/phac-nml/covid-19-signal-nml?tab=readme-ov-file#sars-cov-2-illumina-genome-assembly-line-signal---nml-fork) as it requires the specific final output `qc.csv` file
- Dependencies must be handled [with `conda` or `mamba`](https://github.com/mamba-org/mamba) currently


*Sequencing Test Datasets*

#### SARS-CoV-2

- Location of in-house training set: [10.5281/zenodo.13748104](https://zenodo.org/) _soon-to-be-released_

    - Datasets included:
        - Illumina
            - Training Set A 
                - Contains dehosted Illumina paired-end sequencing data generated using the Freed primer scheme for 25 samples and 3 controls
            - Training Set B 
                - Contains dehosted Illumina paired-end sequencing data generated using the Freed primer scheme for 25 samples and 5 controls
            - Training Set C 
                - Contains dehosted Illumina paired-end sequencing data generated using the freed_V2_nml primer scheme for 92 samples and 5 controls
            - Training Set D 
                - Contains dehosted Illumina paired-end sequencing data generated using the freed_V2_nml primer scheme for 92 samples and 7 controls
            - Permission to use these datasets was provided by the Roy Romanow Provincial Laboratory (Saskatchewan). It should not be used for applications outside those outlined in this document without the express written consent of this group. 
            
                Authors: Anna Majer, Shari Tyson, Grace Seo, Philip Mabon, Elsie Grudeski, Rhiannon Huzarewich, Russell Mandes, Anneliese Landgraff, Jennifer Tanner, Natalie Knox, Morag Graham, Gary Van Domselaar, Ryan McDonald, Keith MacKenzie, Meredith Faires, Kara Loos, Stefani Kary, Rachel DePaulo, Alanna Senecal, Amanda Lang, Jessica Minion, Nathalie Bastien, Yan Li, Timothy Booth, Darian Hole, Madison Chapel, Kirsten Biggar, CanCOGeN's metadata curation team, Public Health Agency of Canada CanCOGeN team
        - Nanopore
            - Contains dehosted nanopore fastq files (and possibly fast5 files?) generated using the freed_V2_nml primer scheme for 93 samples and 3 controls

- CDC public dataset repo [git link](https://github.com/CDCgov/datasets-sars-cov-2) / [publication](https://peerj.com/articles/13821/)
    - What's included:
        - QC failures
        - host contamination
        - outbreak data
        - samples generated using different wet-lab approaches
        - datasets across different lineages 


<br>

**Recipes - how datasets are created**

- The Illumina Training Set A is composed of clinical samples from Saskatchewan sequenced by the National Microbiology Laboratory in April 2021 on a MiSeq instrument with the freed primer scheme. 

      - A run with clean negative controls was selected for use and a subset of samples representing various Ct values and final genome completeness scores.

      - Clinical positive control sample was also included
      
      - An artificial mixture sample was produced by combining reads from two existing samples to ensure that at least one sample would generate an EXCESS_AMBIGUITY flag

- Illumina Training Set B was created by combining Training Set A with a series of additional negative controls, some of which will fail QC checks.

- Illumina Training Set C is composed of clinical samples from Saskatchewan sequenced by the National Microbiology Laboratory in October 2022 on a MiSeq instrument with the freed_V2_nml primer scheme. This dataset also contains the same 2 negative contols found in Training Set A, as well as 3 additional controls: (i) a no templale control, (ii) a SARS-CoV-2 Alpha variant positive control, and (iii) a SARS-CoV-2 Delta variant positive control.

- Illumina Training Set D was created by combining the samples from Training Set C with the 4 negative contols found in Training Set B, as well as 3 additional controls: (i) a no templale control, (ii) a SARS-CoV-2 Alpha variant positive control, and (iii) a SARS-CoV-2 Delta variant positive control (these 3 additional controls are the same 3 additional controls fouund in Training set C).

- The Nanopore Training Set was generated by the Roy Romanow Provincial Laboratory in Saskatchewan on an Mk1c device in January 2022.
        - Run was selected to try and closely match the Illumina run in terms of composition, that is clean negative controls, a variety of Ct values, and final genome completeness scores.

Since Illumina Training Sets A and B were generated during major SARS-CoV-2 variant waves, the lineage composition of these datasets are homogeneous. The Nanopore dataset and Illumina Training Sets C and D contain samples with a variety of SARS-CoV-2 Omicron sublineages.

**Assessing operator proficiency:**

    - Data interpretation criteria must address the below considerations
        - Do Negative (negative-ctrl-#) controls pass specified internal QC metrics? If you detect warnings, what are the reasons and how will this impact the run?
        - Does the positive control (positive-ctrl-1) pass specified internal QC metrics?
        - Based on the above, is the run valid and can interpretations be made about the samples included? Provide rationale
        - How many samples meet minimum quality standards? List which do and do not.
        - Which (if any) samples display excess ambiguity? Should the data from these samples be excluded?
  
##  Guide for producing HTML output comparing QC results of Illumina runs

`SIGNAL_output_comparison_script.Rmd` can be used to compare [SIGNAL pipeline results](https://github.com/phac-nml/covid-19-signal-nml?tab=readme-ov-file#sars-cov-2-illumina-genome-assembly-line-signal---nml-fork) generated by two different users on the same test Illumina data set.

This script requires the following seven files (from both analyses) as input:

- SIGNAL results file:
  - `.qc.csv`
- Ncov-tools quality control files:
  - `_tree.nwk`
  - `_ambiguous_position_report.tsv`
  - `_mixture_report.tsv`
  - `_ncov_watch_variants.tsv`
  - `_negative_control_report.tsv`
  - `_summary_qc.tsv`

The current version of the script requires that the arrangement of these files adhere to a specific directory structure. For example:

```
Training_Set_A
├── User_1_SIGNAL_A
│   └── SIGNAL_analyses
│       ├── ncov-tools
│       │   ├── qc_analysis
│       │   │   └── nml_tree.nwk
│       │   └── qc_reports
│       │       ├── nml_ambiguous_position_report.tsv
│       │       ├── nml_mixture_report.tsv
│       │       ├── nml_ncov_watch_variants.tsv
│       │       ├── nml_negative_control_report.tsv
│       │       └── nml_summary_qc.tsv
│       └── nml.qc.csv
└── User_2_SIGNAL_A
    └── SIGNAL_analyses
        ├── ncov-tools
        │   ├── qc_analysis
        │   │   └── nml_tree.nwk
        │   └── qc_reports
        │       ├── nml_ambiguous_position_report.tsv
        │       ├── nml_mixture_report.tsv
        │       ├── nml_ncov_watch_variants.tsv
        │       ├── nml_negative_control_report.tsv
        │       └── nml_summary_qc.tsv
        └── nml.qc.csv
```

## Configuration file

A configuration (.yaml) file is used to identify the SIGNAL results directories that are to be compared, and to load the necessary R packages for the R markdown script. 

The configuration file requires the following:

`Base_path` - absolute path to the directory that contains the both the `Name_of_folder_one` and `Name_of_folder_two` per-user analyses folders (see below)

`Name_of_folder_one` - Parent directory for user 1 SIGNAL analyses on a particular Illumina dataset

`Name_of_folder_two` - Parent directory for user 2 SIGNAL analyses on a particular Illumina dataset

Using the directory structure example above, the configuration file entries would be as follows:

```
Base_path: <ABSOLUTE/PATH/TO/Training_Set_A> 
    - e.g. "C:/Zenodo_data/SARS_CoV_2/Illumina/Training_Set_A" 
Name_of_folder_one: "User_1_SIGNAL_A"
Name_of_folder_two: "User_2_SIGNAL_A"
```

The following R packages must also be listed in the configuration file as follows:

```
Packages:
  - dplyr
  - data.table
  - readxl
  - tidyverse
  - stringr
  - R.utils
  - writexl
  - lubridate
  - stringi
  - ggplot2
  - gridExtra
  - tinytex
  - kableExtra
  - DT
  - extraoperators
  - hablar
  - ape
  - phylogram
  - dendextend
  - data.tree
  - here
  - colorspace
```
## renv LOCK file usage

A LOCK file has been created to track R version and R package versions used with the script. To load the R package versions used during the creation of this script, run `renv::restore()`. 

## Script usage

Once the configuration file has been created, knit the script in RStudio to generate HTML output.  

## HTML Output

Contains a detailed comparison of per-user SIGNAL/ncov-tools output.

## CL Instructions

### Install
Install using conda/mamba with `renv restore`

```bash
conda create -n signal_compare_env -c conda-forge r-base=4.3.0 r-renv=1.0.11 r-rmarkdown=2.27 r-ggplot2=3.5.1 r-tidyverse=2.0.0 r-here=1.0.1
conda activate signal_compare_env
Rscript -e "renv::restore()"
```

Install using conda/mamba with `conda/mamba env create`. Not all dependencies were in conda-forge (some were only in the R channel) but running the SIGNAL script pulls the additional ones

```bash
conda env create -f environment.yml
conda activate signal_compare_env
```

If it fails to fully restore with `renv`, running the `SIGNAL_output_comparison_script.Rmd` script will install most of them as well

### Setup SIGNAL (only do the first time)

1. Follow the setup [instructions here](https://github.com/phac-nml/covid-19-signal-nml?tab=readme-ov-file#setup) which are basically:
    - Make sure you have conda installed
    - Clone the repo: `git clone https://github.com/phac-nml/covid-19-signal-nml.git`

2. Run the [setup script](https://github.com/phac-nml/covid-19-signal-nml?tab=readme-ov-file#1-get-dependencies) to get needed large files
    ```bash
    cd covid-19-signal-nml
    bash scripts/get_data_dependencies.sh -d data -a MN908947.3
    ```
    
    *Note:* It takes ~10GB storage and up to 35GB memory for temp downloads

### Run the Pipeline on a directory of `.fastq.gz` covid data

Help instructions can be [found here](https://github.com/phac-nml/covid-19-signal-nml?tab=readme-ov-file#help-command) or by running the `run_signal.sh` bash script with no args

```bash
bash /path/to/covid-19-signal-nml/run_signal.sh -d PATH_TO_PAIRED_FASTQ_DIR -p PRIMER_SCHEME <OPTIONAL FLAGS>
```

It will take additional time the first time it is run as it has to make the conda environments. Each subsequent use will reuse the made environments

To do a comparison, you will also have to run the data twice@

### Setup the Config and Rmarkdown file

1. Activate the `signal_compare_env` environment (or whatever you named it)

2. Copy and edit the [`config_file.yaml`](#configuration-file) to have the correct pathes to the data

3. Run the `SIGNAL_output_comparison_script.Rmd` script:

    ```bash
    Rscript -e "rmarkdown::render('SIGNAL_output_comparison_script.Rmd', params = list(config = '/path/to/config', outfile = paste0(Sys.Date(), '_report.html')))"
    ```
