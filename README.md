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
> Extract files from *GBS_data.tar.gz*, *refgenome.tar.gz*, and *scripts.tar.gz* using:  
> 'tar xzvf file.tar.gz'.    
> It is also required to gunzip the GBS data file named *FC20150701_1.fq.gz* which is in *GBS_data* directory using:  
> 'gunzip FC20150701_1.fq.gz'.  

## Variant calling steps  
### sabre.sh  
> The script also redirects all output messages to a *sabre.log* file and and output named *unk.fastq* containing records on which no barcode have been found.  
> An example of 48 resulting *.fq* files, the *unk.fastq* and the *sabre.log* file are in *out_sabre*.  

### cutad_for.sh and cutad_parallel.sh  
> Second step is to remove the adaptor *AGATCGGAA* from reads and discard short reads under 50bp length using *Cutadapt* tool. 
> *cutad_parallel.sh* do the same tasks than *cutad_for.sh* but in parallel using *GNU Parallel* tool.   
> The script also redirects all output messages to a *cutadap.log* file.  
> An example of 48 output *.fastq* files and the *cutadap.log* file are in *out_cutadapt*.  

### Read quality
> Read quality  using *FastQC* tool has not been done herein.

### mapping.sh  
> Third step is to align reads to the reference genome using the *bwa* tool.  
> Index of the reference genome have been previously done using *bwa index* and files are store with the reference genome in refgenome directory.  
> This script also uses *Parallel* and redirects all output messages to a *bwa.log* file.  
> An example of resulting *.sam* files and the *bwa.log* file are in *out_bwa*.  

### sam2bam.sh  
> The fourth step is to convert *.sam* format in *.bam* format and sort and index the resulting BAM files using *Samtools* tools. 
> The script uses 'parallel'.   
> The script also list the BAM files in a bamlist file and redirects all output messages to a *convert.log* file. 
> **IMPORTANT NOTE** The bamlist must be kept in the directory where it has been generated to ensure *samt_var.sh* running correctly. 
> An example of the output files are in the out_sam2bam directory.  

### samt_var.sh  
> The last step is to call variants.  
> *bcftools mpileup* is used to generate genotype likelihoods and *bcftools call* is use to call variant (in default mode and only outputting variant sites using -mv arguments.  
> The script redirects all output messages to a *samt_var.log* file.  
> **IMPORTANT NOTE** The bamlist generated previously must be kept in the directory where it has been generated to ensure *samt_var.sh* running correctly.  
> An example of generated *.bcf* and *.vcf* files and the *.log* file are in out_samt directory.  

