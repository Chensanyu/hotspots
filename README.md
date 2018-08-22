
[![Build Status](https://travis-ci.org/taylor-lab/hotspots.svg?branch=add_tests)](https://travis-ci.org/taylor-lab/hotspots)
[![codecov.io](https://img.shields.io/codecov/c/github/taylor-lab/hotspots.svg)](https://codecov.io/github/taylor-lab/hotspots?branch=add_tests)


## Identifying recurrent mutations in cancer

[<img target="_blank" align="right" border="0" alt="" src="http://www.cbioportal.org/images/cbioportal_logo.png" width="125" height="30">](http://www.cbioportal.org/) 
<img align="right" border="0" alt="" src="http://www.cbioportal.org/images/oncokb-flame.svg" width="30" height="30">(http://oncokb.org/)


### Description

This is a method to identify population-scale recurrent mutations in cancer based on a binomial
statisical model that incoporates underlying mutational processes including nucleotide context
mutability, gene-specific mutation rates, and major expected patterns of hotspot mutation emergence

### Dependencies

Need R Version 3.0.2 or higher

Dependencies:
* data.table
* IRanges
* BSgenome.Hsapiens.UCSC.hg19

```
install.packages("data.table")
source("http://bioconductor.org/biocLite.R")
biocLite("IRanges","BSgenome.Hsapiens.UCSC.hg19")
```

### Usage

```
./hotspot_algo.R
    --input-maf=[REQUIRED: mutation file]
    --rdata=[REQUIRED: Rdata object with necessary files for algorithm]
    --output-file=[REQUIRED: output file to print statistically significant hotspots]
    --gene-query=[OPTIONAL (default=all genes in mutation file): List of Hugo Symbol in which to query for hotspots]
    --homopolymer=[OPTIONAL (default=TRUE): TRUE|FALSE filter hotspot mutations in homopolymer regions]
    --filter-centerbias=[OPTIONAL (default=FALSE): TRUE|FALSE to identify false positive filtering based on mutation calling center bias]
    --align100mer=[OPTIONAL: BED file of hg19 UCSC alignability track for 100-mer length sequences for false positive filtering]
    --align24mer=[OPTIONAL: BED file of hg19 UCSC alignability track for 24-mer length sequences for false positive filtering]
```
Command to run hotspot algorithm on genes listed in file genes_of_interest.txt:
```
./hotspot_algo.R \
	--input-maf=pancancer_unfiltered.maf \
	--rdata=hotspot_algo.Rdata \
	--gene-query=genes_of_interest.txt \
	--output-file=sig_hotspots.txt
```

### Contents

`hotspot_algo.R` - R script to execute hotspot detection algorithm

`hotspot_algo.Rdata` - Rdata object with necessary files for algorithm (mutability, expression filters, etc)

`funcs.R` - R script of functions necessary for proper execution of hotspot_algo.R

`genes_of_interest.txt` - Sample list of genes for hotspot detection

`minimalist_test_maf.txt` - minimalist MAF needed from maf2maf. [mskcc/maf2maf](https://github.com/mskcc/vcf2maf)

### Notes

`--align100mer` and `--align24mer` are optional filters based on how uniquely k-mer sequences align to a region of the hg19 genome. Note, both filters were used as part of this analysis. See more information at [ENCODE Mapability](http://genome.ucsc.edu/cgi-bin/hgFileUi?db=hg19&g=wgEncodeMapability).

The use of these filters will require downloading the 100-mer and 24-mer alignability tracks from UCSC that are not included here:
	[ENCODE CRG Alignability 100-mer](http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeMapability/wgEncodeCrgMapabilityAlign100mer.bigWig)
	[ENCODE CRG Alignability 24-mer](http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeMapability/wgEncodeCrgMapabilityAlign24mer.bigWig)

Convert these downloaded bigWig to bedgraph format, following instructions here: [UCSC BigWig](http://genome.ucsc.edu/goldenpath/help/bigWig.html)

### Citation

Chang MT et al, 2015. 'Identifying recurrent mutations in cancer reveals widespread lineage diversity and mutational specificity.' Nat Biotechnol. 2016 Feb;34(2):155-63. doi: 10.1038/nbt.3391. Epub 2015 Nov 30. (Nature Biotechnology)[https://www.nature.com/articles/nbt.3391 ] (PubMed)[https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4744099/ ]

### R Package Contributors

* Michael Chang
* Evan Biederstedt 
* JJ Gao
* Christopher Harris
* Philip Jonsson 
* Cyriac Kandoth
* Alex Penson
* Barry Taylor
