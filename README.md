### Motivation
My laptop can't cope with the huge Parse datasets, had tried installing Seurat 5 on Eddie worksapce but it kept failing installing some of the dependencies (probably because of some conflict arising from using conda gcc and unable to create a shared object?). Donald suggested using conda environment that worked very well. Took a while to install all the packages (about an hour) with lots of erros on screen but very pleased to see it has managed to install Seurat v5!!  

***


### Installation

Request a node for installation
```
qlogin -l h_vmem=10G
```

Load the latest/most recent version of anaconda. By default when you log into a node, a very dated version of conda module is loaded (v4.7) that doesn't have r-base >4.0 on it and just wouldn't let me install anything at all.  
```
module load anaconda/2024.02
```
Search r-base versions available, I went ahead with the latest version of R, why not!!
```
conda search r-base
#r-base                         4.3.3      he2d9a6e_9  conda-forge
#r-base                         4.3.3      hf0d99cb_1  conda-forge
#r-base                         4.4.0      h019f4a6_1  conda-forge
#r-base                         4.4.1      h0be853a_1  conda-forge
```

Now, create a conda environment (I named it seurat5 but feel free to whatever pleases you). Not I have instructed conda to create this environment with the latest R version 4.4.1.  
```
conda create --name seurat5 -c conda-forge  r-base=4.4.1

#environment location: /exports/cmvm/eddie/eb/groups/macqueen_lab/Pooran/anaconda/envs/seurat5
#added / updated specs:
#   - r-base=4.4.1

```

Activate the newly-created conda env and install Seurat package in R. By default, R will install the most recent version of Seurat, which is v5.1.0 as of today, i.e., 19 August 2024.

```
conda activate seurat5

R

install.packages('Seurat', repos='http://cran.us.r-project.org')

packageVersion("Seurat")
#‘5.1.0’

install.packages('tidyverse', repos='http://cran.us.r-project.org')

```

### Analysis

Requesting slightly higher memory as I know this dataset requires about 30Gb of RAM.  
```
qlogin -l h_vmem=30G

module load anaconda/2024.02

conda activate seurat5
```
#### R Seurat analysis
```
R

library("Seurat")
library("Matrix")
library("tidyverse")

setwd("/exports/eddie/scratch/pdewari/seurat5_analysis/DGE_unfiltered/")

#based on tutorial here https://support.parsebiosciences.com/hc/en-us/articles/360053078092-Seurat-Tutorial-65k-PBMCs

mat_path <- "/exports/eddie/scratch/pdewari/seurat5_analysis/DGE_unfiltered/"
mat <- ReadParseBio(mat_path)
table(rownames(mat) == "")
cell_meta <- read.csv(paste0(mat_path, "/cell_metadata.csv"), row.names = 1)
seu_obj <- CreateSeuratObject(mat, meta.data = cell_meta)
seu_obj
```
