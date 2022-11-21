# iGTEx
This project describes an automated pipeline to quantify isoform expressions for the GTEx V8 RNA sequencing data. The protocol uses [XAEM](https://github.com/WenjiangDeng/XAEM), a powerful method for isoform expression estimation across multiple samples. You also can find the detailed information about XAEM [website](https://www.meb.ki.se/sites/biostatwiki/xaem) and in the published paper in [Bioinformatics](https://academic.oup.com/bioinformatics/article/36/3/805/5545974).

## Prerequisites
```
R (recommended version >= 3.5.1)
Python (recommended version >= 3.7)
```


## Installation

#### Step 1: Setup XAEM
Install the XAEM tool for this protocol via the bash command:
```
git https://github.com/ZhengCQ/iGTEx.git
```
or 
```
wget https://github.com/ZhengCQ/iGTEx/archive/refs/tags/iGTEx_XAEM_v0.1.1.zip
unzip iGTEx_XAEM_v0.1.1.zip
ln -fs iGTEx-iGTEx_XAEM_v0.1.1 iGTEx_XAEM
```

#### Step 2: Setup R dependencies
In R, install the R dependencies via:
```
install.packages("foreach")
install.packages("doParallel")
```

#### Step 3: Download the annotation reference
Run the following commands to download the reference annotating the transcripts:
```
cd /path/to/iGTEx_XAEM
python down_ref.py
```

## Example
An example is prepared in the project **Example** folder, executable as
```
cd /path/to/iGTEx_XAEM/Example
sh run_example.sh 
```

## Isoform Estimation using GTEx data

XAEM performs better when multiple samples of similar data type are considered. In the GTEx V8 data, for **each tissue**, we create a project directory:
```
mkdir -p /path/to/Tissue1
cd /path/to/Tissue1
```
#### Input files
In `/path/to/Tissue1`, create a file `/path/to/Tissue1/infastq_lst.tsv` listing the FASTQ input files. The file is a tab-delimited text file with 4 columns: `Sample name`, `Source name`, `FASTQ file name for paired-end read 1`, and `FASTQ file name for paired-end read 2`. `Source name` indicates the batch or sequencing library of the sample, so that the same sample may correspond to more than one sources. A standard example, where each sample has only a single batch, is given as `/path/to/iGTEx_XAEM/Example/infastq_lst.tsv`:
```
sample1 S0001   S0001_1.fg.gz   S0001_2.fg.gz
sample1 S0002   S0002_1.fg.gz   S0002_2.fg.gz
sample2 S0003   S0003_1.fg.gz   S0003_2.fg.gz
sample2 S0004   S0004_1.fg.gz   S0004_2.fg.gz
sample3 S0005   S0005_1.fg.gz   S0005_2.fg.gz
sample3 S0006   S0006_1.fg.gz   S0006_2.fg.gz
sample4 S0007   S0007_1.fg.gz   S0007_2.fg.gz
sample5 S0008   S0008_1.fg.gz   S0008_2.fg.gz
```

#### Run XAEM 
XAEM can be easily run with:
```
python /path/to/iGTEx_XAEM/run_xaem.py -i /path/to/Tissue1/infastq_lst.tsv
```
(Optional) To specify a particular output directory, use:
```
-o /path/to/Tissue1_output_directory
```
(Optional) Further customized configuration of XAEM can be setup by:
```
-c /path/to/Tissue1_config.ini
```
An example of the `config.ini` file can be found in `/path/to/iGTEx_XAEM/`.

