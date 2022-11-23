# **Variant-calling-assignment**
Here are main steps for the variant-calling assignment of the 'BVG-7003' course.  
  
## Prior required steps
### Installing required tools  
> Seven tools are requiered to perform variant-calling:  
> *Sabre*: https://github.com/najoshi/sabre  
> *Cutadapt*: https://cutadapt.readthedocs.io/en/stable/installation.html  
> *BWA*: https://github.com/lh3/bwa  
> *Samtools, BFCTools and htslib*: http://www.htslib.org/download/  
>  *GNU Parallel*: https://www.gnu.org/software/parallel/parallel_tutorial.html  

### Extract files from gzipped archive  
> Extract files from *GBS_data.tar.gz*, *refgenome.tar.gz*, *results.tar.gz* and *scripts.tar.gz* using:  
> `tar xzvf file_to_extract.tar.gz`   
> It is also required to gunzip the GBS data file named *FC20150701_1.fq.gz* which is in *GBS_data* directory using:  
> `gunzip FC20150701_1.fq.gz`  
> **NOTE**: only the first 150 lines have been kept in each data files of refgenome and results directory and for *FC20150701_1.fq.gz* file to allow the uploading on Github.  

## Variant calling steps  
### sabre.sh  
> The first step is to demultiplex GBS *.fastq* data file, which is to separate each individuals according to their specific barcode and generate individual *.fq* files.  
> The script also redirects all output messages to a *sabre.log* file and generates an output named *unk.fastq* containing records for which no barcode have been found.  
> An example of 48 resulting *.fq* files, the *unk.fastq* and the *sabre.log* file are in *results/out_sabre*.  

### cutad_for.sh and cutad_parallel.sh  
> Second step is to remove the adaptor *AGATCGGAA* from reads and discard short reads under 50bp length using *Cutadapt* tool. 
> *cutad_parallel.sh* does the same tasks as *cutad_for.sh* but in parallel using *GNU Parallel* tool.   
> The script also redirects all output messages to a *cutadap.log* file.  
> An example of 48 output *.fastq* files and the *cutadap.log* file are in *results/out_cutadapt*.  

### Read quality
> Read quality  using *FastQC* tool has not been done herein.

### mapping.sh  
> Third step is to align reads to the reference genome using the *bwa* tool.  
> Index of the reference genome have been previously done using *bwa index* and files are stored with the reference genome in the *refgenome* directory.  
> This script also uses *Parallel* and redirects all output messages to a *bwa.log* file.  
> An example of resulting *.sam* files and the *bwa.log* file are in *results/out_bwa*.  

### sam2bam.sh  
> The fourth step is to convert *.sam* format in *.bam* format and sort and index the resulting BAM files using *Samtools* tools.  
> The script uses 'parallel' to execute jobs in parallel.   
> The script also lists the BAM files in a *bamlist* file and redirects all output messages to a *convert.log* file.  
> **IMPORTANT NOTE** The *bamlist* file must be kept in the directory where it has been generated to ensure the next step running correctly.  
> An example of the output files are in the *results/out_sam2bam* directory.  

### samt_var.sh  
> The last step is to call variants.  
> *bcftools mpileup* is used to generate genotype likelihoods and *bcftools call* is use to call variants (in default mode and only outputting variant sites using -mv arguments).  
> The script redirects all output messages to a *samt_var.log* file.   
> An example of generated *.bcf* and *.vcf* files and the *.log* file are in *results/out_samt* directory.  

