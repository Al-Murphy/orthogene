`orthogene`: Interspecies gene mapping
================
<img src='https://github.com/neurogenomics/orthogene/raw/main/inst/hex/hex.png' title='Hex sticker for orthogene' height='300'><br>
[![](https://img.shields.io/badge/release%20version-1.8.0-green.svg)](https://www.bioconductor.org/packages/orthogene)
[![](https://img.shields.io/badge/download-340/month-green.svg)](https://bioconductor.org/packages/stats/bioc/orthogene)
[![](https://img.shields.io/badge/download-4739/total-green.svg)](https://bioconductor.org/packages/stats/bioc/orthogene)
[![download](http://www.bioconductor.org/shields/downloads/release/orthogene.svg)](https://bioconductor.org/packages/stats/bioc/orthogene)
[![License:
GPL-3](https://img.shields.io/badge/license-GPL--3-blue.svg)](https://cran.r-project.org/web/licenses/GPL-3)
<br>
[![](https://img.shields.io/badge/devel%20version-1.9.1-black.svg)](https://github.com/neurogenomics/orthogene)
[![](https://img.shields.io/github/languages/code-size/neurogenomics/orthogene.svg)](https://github.com/neurogenomics/orthogene)
[![](https://img.shields.io/github/last-commit/neurogenomics/orthogene.svg)](https://github.com/neurogenomics/orthogene/commits/main)
<br> [![R build
status](https://github.com/neurogenomics/orthogene/workflows/rworkflows/badge.svg)](https://github.com/neurogenomics/orthogene/actions)
[![](https://codecov.io/gh/neurogenomics/orthogene/branch/main/graph/badge.svg)](https://app.codecov.io/gh/neurogenomics/orthogene)
<br>
<a href='https://app.codecov.io/gh/neurogenomics/orthogene/tree/main' target='_blank'><img src='https://codecov.io/gh/neurogenomics/orthogene/branch/main/graphs/icicle.svg' title='Codecov icicle graph' width='200' height='50' style='vertical-align: top;'></a>  
<h4>  
Authors: <i>Brian Schilder</i>  
</h4>
<h4>  
README updated: <i>Dec-21-2023</i>  
</h4>

# Intro

`orthogene` is an R package for easy mapping of orthologous genes across
hundreds of species. It pulls up-to-date gene ortholog mappings across
**700+ organisms**. It also provides various utility functions to
aggregate/expand common objects (e.g. data.frames, gene expression
matrices, lists) using **1:1**, **many:1**, **1:many** or **many:many**
gene mappings, both within- and between-species.

In brief, `orthogene` lets you easily:

- [**`convert_orthologs`** between any two
  species.](https://neurogenomics.github.io/orthogene/articles/orthogene#convert-orthologs)
- [**`map_species`** names onto standard taxonomic
  ontologies.](https://neurogenomics.github.io/orthogene/articles/orthogene#map-species)  
- [**`report_orthologs`** between any two
  species.](https://neurogenomics.github.io/orthogene/articles/orthogene#report-orthologs)
- [**`map_genes`** onto standard
  ontologies](https://neurogenomics.github.io/orthogene/articles/orthogene#map-genes)
- [**`aggregate_mapped_genes`** in a
  matrix.](https://neurogenomics.github.io/orthogene/articles/orthogene#aggregate-mapped-genes)  
- [get **`all_genes`** from any
  species.](https://neurogenomics.github.io/orthogene/articles/orthogene#get-all-genes)
- [**`infer_species`** from gene
  names.](https://neurogenomics.github.io/orthogene/articles/infer_species.html)  
- [**`create_background`** gene lists based one, two, or more
  species.](https://neurogenomics.github.io/orthogene/reference/create_background.html)  
- [**`get_silhouettes`** of each species from
  phylopic.](https://neurogenomics.github.io/orthogene/reference/get_silhouettes.html)  
- [**`prepare_tree`** with evolutionary divergence times across
  \>147,000
  species.](https://neurogenomics.github.io/orthogene/reference/prepare_tree.html)

## Citation

If you use `orthogene`, please cite:

<!-- Modify this by editing the file: inst/CITATION  -->

> Brian M. Schilder, Nathan G. Skene (2022). orthogene: Interspecies
> gene mapping. R package version 1.4.0,
> <https://doi.org/doi:10.18129/B9.bioc.orthogene>

## [Documentation website](https://neurogenomics.github.io/orthogene/)

## [PDF manual](https://github.com/neurogenomics/orthogene/blob/main/inst/orthogene_1.5.1.pdf)

# Installation

``` r
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
# orthogene is only available on Bioconductor>=3.14
if(BiocManager::version()<"3.14") BiocManager::install(update = TRUE, ask = FALSE)

BiocManager::install("orthogene")
```

## Docker

`orthogene` can also be installed via a
[Docker](https://hub.docker.com/repository/docker/neurogenomicslab/orthogene)
or
[Singularity](https://sylabs.io/guides/2.6/user-guide/singularity_and_docker.html)
container with Rstudio pre-installed. Further [instructions provided
here](https://neurogenomics.github.io/orthogene/articles/docker).

# Methods

``` r
library(orthogene)

data("exp_mouse")
# Setting to "homologene" for the purposes of quick demonstration.
# We generally recommend using method="gprofiler" (default).
method <- "homologene"  
```

For most functions, `orthogene` lets users choose between different
methods, each with complementary strengths and weaknesses:
`"gprofiler"`, `"homologene"`, and `"babelgene"`

In general, we recommend you use `"gprofiler"` when possible, as it
tends to be more comprehensive.

While `"babelgene"` contains less species, it queries a wide variety of
orthology databases and can return a column “support_n” that tells you
how many databases support each ortholog gene mapping. This can be
helpful when you need a semi-quantitative measure of mapping quality.

It’s also worth noting that for smaller gene sets, the speed difference
between these methods becomes negligible.

|                     | gprofiler                     | homologene         | babelgene                                                                                                                                                                                                                 |
|:--------------------|:------------------------------|:-------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Reference organisms | 700+                          | 20+                | 19 (but cannot convert between pairs of non-human species)                                                                                                                                                                |
| Gene mappings       | More comprehensive            | Less comprehensive | More comprehensive                                                                                                                                                                                                        |
| Updates             | Frequent                      | Less frequent      | Less frequent                                                                                                                                                                                                             |
| Orthology databases | Ensembl, HomoloGene, WormBase | HomoloGene         | HGNC Comparison of Orthology Predictions (HCOP), which includes predictions from eggNOG, Ensembl Compara, HGNC, HomoloGene, Inparanoid, NCBI Gene Orthology, OMA, OrthoDB, OrthoMCL, Panther, PhylomeDB, TreeFam and ZFIN |
| Data location       | Remote                        | Local              | Local                                                                                                                                                                                                                     |
| Internet connection | Required                      | Not required       | Not required                                                                                                                                                                                                              |
| Speed               | Slower                        | Faster             | Medium                                                                                                                                                                                                                    |

# Quick example

## Convert orthologs

[`convert_orthologs`](https://neurogenomics.github.io/orthogene/reference/convert_orthologs.html)
is very flexible with what users can supply as `gene_df`, and can take a
`data.frame`/`data.table`/`tibble`, (sparse) `matrix`, or
`list`/`vector` containing genes.

Genes, transcripts, proteins, SNPs, or genomic ranges will be recognised
in most formats (HGNC, Ensembl, RefSeq, UniProt, etc.) and can even be a
mixture of different formats.

All genes will be mapped to gene symbols, unless specified otherwise
with the `...` arguments (see `?orthogene::convert_orthologs` or
[here](https://neurogenomics.github.io/orthogene/reference/convert_orthologs.html)
for details).

### Note on non-1:1 orthologs

A key feature of
[`convert_orthologs`](https://neurogenomics.github.io/orthogene/reference/convert_orthologs.html)
is that it handles the issue of genes with many-to-many mappings across
species. This can occur due to evolutionary divergence, and the function
of these genes tend to be less conserved and less translatable. Users
can address this using different strategies via `non121_strategy=`.

``` r
gene_df <- orthogene::convert_orthologs(gene_df = exp_mouse,
                                        gene_input = "rownames", 
                                        gene_output = "rownames", 
                                        input_species = "mouse",
                                        output_species = "human",
                                        non121_strategy = "drop_both_species",
                                        method = method) 
```

    ## Preparing gene_df.

    ## sparseMatrix format detected.

    ## Extracting genes from rownames.

    ## 15,259 genes extracted.

    ## Converting mouse ==> human orthologs using: homologene

    ## Retrieving all organisms available in homologene.

    ## Mapping species name: mouse

    ## Common name mapping found for mouse

    ## 1 organism identified from search: 10090

    ## Retrieving all organisms available in homologene.

    ## Mapping species name: human

    ## Common name mapping found for human

    ## 1 organism identified from search: 9606

    ## Checking for genes without orthologs in human.

    ## Extracting genes from input_gene.

    ## 13,416 genes extracted.

    ## Extracting genes from ortholog_gene.

    ## 13,416 genes extracted.

    ## Checking for genes without 1:1 orthologs.

    ## Dropping 46 genes that have multiple input_gene per ortholog_gene (many:1).

    ## Dropping 56 genes that have multiple ortholog_gene per input_gene (1:many).

    ## Filtering gene_df with gene_map

    ## Setting ortholog_gene to rownames.

    ## 
    ## =========== REPORT SUMMARY ===========

    ## Total genes dropped after convert_orthologs :
    ##    2,016 / 15,259 (13%)

    ## Total genes remaining after convert_orthologs :
    ##    13,243 / 15,259 (87%)

``` r
knitr::kable(as.matrix(head(gene_df)))
```

|          | astrocytes_ependymal | endothelial-mural | interneurons | microglia | oligodendrocytes | pyramidal CA1 | pyramidal SS |
|:---------|---------------------:|------------------:|-------------:|----------:|-----------------:|--------------:|-------------:|
| TSPAN12  |            0.3303571 |         0.5872340 |    0.6413793 | 0.1428571 |        0.1207317 |     0.2864750 |    0.1453634 |
| TSHZ1    |            0.4285714 |         0.4468085 |    1.1551724 | 0.4387755 |        0.3621951 |     0.0692226 |    0.8320802 |
| ADAMTS15 |            0.0089286 |         0.0978723 |    0.2206897 | 0.0000000 |        0.0231707 |     0.0117146 |    0.0375940 |
| CLDN12   |            0.2232143 |         0.1148936 |    0.5517241 | 0.0510204 |        0.2609756 |     0.4376997 |    0.6842105 |
| RXFP1    |            0.0000000 |         0.0127660 |    0.2551724 | 0.0000000 |        0.0158537 |     0.0511182 |    0.0751880 |
| SEMA3C   |            0.1964286 |         0.9957447 |    8.6379310 | 0.2040816 |        0.1853659 |     0.1608094 |    0.2280702 |

`convert_orthologs` is just one of the many useful functions in
`orthogene`. Please see the [documentation
website](https://neurogenomics.github.io/orthogene/articles/orthogene)
for the full vignette.

# Additional resources

## [Hex sticker creation](https://github.com/neurogenomics/orthogene/blob/main/inst/hex/hexSticker.Rmd)

## [Benchmarking methods](https://github.com/neurogenomics/orthogene/blob/main/inst/benchmark/benchmarks.Rmd)

# Session Info

<details>

``` r
utils::sessionInfo()
```

    ## R version 4.3.1 (2023-06-16)
    ## Platform: aarch64-apple-darwin20 (64-bit)
    ## Running under: macOS Sonoma 14.2
    ## 
    ## Matrix products: default
    ## BLAS:   /Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/lib/libRblas.0.dylib 
    ## LAPACK: /Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/lib/libRlapack.dylib;  LAPACK version 3.11.0
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## time zone: America/New_York
    ## tzcode source: internal
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] orthogene_1.8.0
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] gtable_0.3.4              babelgene_22.9           
    ##  [3] xfun_0.41                 ggplot2_3.4.4            
    ##  [5] htmlwidgets_1.6.4         rstatix_0.7.2            
    ##  [7] lattice_0.22-5            vctrs_0.6.5              
    ##  [9] tools_4.3.1               generics_0.1.3           
    ## [11] yulab.utils_0.1.1         parallel_4.3.1           
    ## [13] tibble_3.2.1              fansi_1.0.6              
    ## [15] pkgconfig_2.0.3           Matrix_1.6-4             
    ## [17] ggplotify_0.1.2           data.table_1.14.10       
    ## [19] homologene_1.4.68.19.3.27 RColorBrewer_1.1-3       
    ## [21] desc_1.4.3                lifecycle_1.0.4          
    ## [23] compiler_4.3.1            treeio_1.26.0            
    ## [25] dlstats_0.1.7             munsell_0.5.0            
    ## [27] carData_3.0-5             ggtree_3.10.0            
    ## [29] gprofiler2_0.2.2          ggfun_0.1.3              
    ## [31] htmltools_0.5.7           yaml_2.3.8               
    ## [33] lazyeval_0.2.2            plotly_4.10.3            
    ## [35] pillar_1.9.0              car_3.1-2                
    ## [37] ggpubr_0.6.0              tidyr_1.3.0              
    ## [39] cachem_1.0.8              grr_0.9.5                
    ## [41] abind_1.4-5               nlme_3.1-164             
    ## [43] tidyselect_1.2.0          aplot_0.2.2              
    ## [45] digest_0.6.33             dplyr_1.1.4              
    ## [47] purrr_1.0.2               rprojroot_2.0.4          
    ## [49] fastmap_1.1.1             grid_4.3.1               
    ## [51] here_1.0.1                colorspace_2.1-0         
    ## [53] cli_3.6.2                 magrittr_2.0.3           
    ## [55] patchwork_1.1.3           utf8_1.2.4               
    ## [57] broom_1.0.5               ape_5.7-1                
    ## [59] withr_2.5.2               scales_1.3.0             
    ## [61] backports_1.4.1           httr_1.4.7               
    ## [63] rmarkdown_2.25            rvcheck_0.2.1            
    ## [65] ggsignif_0.6.4            memoise_2.0.1.9000       
    ## [67] evaluate_0.23             knitr_1.45               
    ## [69] rworkflows_1.0.1          viridisLite_0.4.2        
    ## [71] gridGraphics_0.5-1        rlang_1.1.2              
    ## [73] Rcpp_1.0.11               glue_1.6.2               
    ## [75] tidytree_0.4.6            BiocManager_1.30.22      
    ## [77] renv_1.0.3                rstudioapi_0.15.0        
    ## [79] jsonlite_1.8.8            R6_2.5.1                 
    ## [81] badger_0.2.3              fs_1.6.3

</details>

# Related projects

## Tools

- [`gprofiler2`](https://cran.r-project.org/web/packages/gprofiler2/vignettes/gprofiler2.html):
  `orthogene` uses this package. `gprofiler2::gorth()` pulls from [many
  orthology mapping
  databases](https://biit.cs.ut.ee/gprofiler/page/organism-list).

- [`homologene`](https://github.com/oganm/homologene): `orthogene` uses
  this package. Provides API access to NCBI
  [HomoloGene](https://www.ncbi.nlm.nih.gov/homologene) database.

- [`babelgene`](https://cran.r-project.org/web/packages/babelgene/vignettes/babelgene-intro.html):
  `orthogene` uses this package. `babelgene::orthologs()` pulls from
  [many orthology mapping
  databases](https://cran.r-project.org/web/packages/babelgene/vignettes/babelgene-intro.html).

- [`annotationTools`](https://www.bioconductor.org/packages/release/bioc/html/annotationTools.html):
  For interspecies microarray data.

- [`orthology`](https://www.leibniz-hki.de/en/orthology-r-package.html):
  R package for ortholog mapping (deprecated?).

- [`hpgltools::load_biomart_orthologs()`](https://rdrr.io/github/elsayed-lab/hpgltools/man/load_biomart_orthologs.html):
  Helper function to get orthologs from biomart.

- [`JustOrthologs`](https://github.com/ridgelab/JustOrthologs/):
  Ortholog inference from multi-species genomic sequences.

- [`orthologr`](https://github.com/drostlab/orthologr): Ortholog
  inference from multi-species genomic sequences.

- [`OrthoFinder`](https://github.com/davidemms/OrthoFinder): Gene
  duplication event inference from multi-species genomics.

## Databases

- [HomoloGene](https://www.ncbi.nlm.nih.gov/homologene): NCBI database
  that the R package [homologene](https://github.com/oganm/homologene)
  pulls from.

- [gProfiler](https://biit.cs.ut.ee/gprofiler): Web server for
  functional enrichment analysis and conversions of gene lists.

- [OrtholoGene](http://orthologene.org/resources.html): Compiled list of
  gene orthology resources.

## Contact

### [Neurogenomics Lab](https://www.neurogenomics.co.uk/)

UK Dementia Research Institute  
Department of Brain Sciences  
Faculty of Medicine  
Imperial College London  
[GitHub](https://github.com/neurogenomics)  
[DockerHub](https://hub.docker.com/orgs/neurogenomicslab)

<br>
