# Genomic analysis test

This report present the beginning of a genomic analysis of patient data. 
This Github link (https://github.com/ARTbio/genomic-analysis-test/blob/master/README.md) give access to the instructions for this genomic analysis. 

## Dependancies

* The work has done on ubuntu 14.04 LTS - 64 bits, from my /home/ repertory.
* It is necessary to install Miniconda. You can do this by following these few command lines on the terminal (from https://bioinfo-fr.net/conda-le-meilleur-ami-du-bioinformaticien). 
```
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
conda config --add channels bioconda
```
A repertory called miniconda3 is created. 
* Now, via Miniconda, you can install the different **tools** we need (**bowtie2** v2.3.4.3, **samtools** v1.9, **bcftools** v1.9, **gatk** v3.8.5, **belly** v0.7.7) by :
```
conda install {tool}
```
When you want to use an installed tool :
```
~/miniconda3/bin/tool {options}
```
* Integrative Genomics Viewer - IGV - is very useful to visualize the results. To install this tool :
```
#Download Java version 8
wget https://javadl.oracle.com/webapps/download/AutoDL?BundleId=236878_42970487e3af4f5aa5bca3f542482c60
#Decompression and extraction
tar zxvf jre-8u201-linux-x64.tar.gz
#Download of the IGV tool
wget http://data.broadinstitute.org/igv/projects/downloads/2.4/IGV_2.4.17.zip
#Launch IGV
~/jre1.8.0_201/bin/java -Xmx750m -jar ~/IGV_2.4.17/libigv.jar
```
* Install FastQC
```
#Download
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.8.zip
#Decompression and extraction
unzip fastqc_v0.11.8.zip
#Make the script executable
chmod +x FastQC/fastqc
```

## Input datasets

### 1. Patient data
Download patient data from the Github link :
```
wget https://storage.googleapis.com/artbio_genomic_analysis/jupyter_R1.fastq.gz
wget https://storage.googleapis.com/artbio_genomic_analysis/jupyter_R1.fastq.gz
wget https://storage.googleapis.com/artbio_genomic_analysis/jupyter_R1.fastq.gz
wget https://storage.googleapis.com/artbio_genomic_analysis/jupyter_R1.fastq.gz
```

### 2. Reference data
The reference human genome CRCh38/hg18 is in IGV. So, you can have this from IGV : Genomes > Load Genome From Server > Select Human hg18 and Download Sequence > OK. The reference will be download in fasta format in your ~/Downloads/ repertory. 

### 3. 
In order to classify the files, create repertories and move the different files in the corresponding repertory : 
```
mkdir Genomic Genomic/data Genomic/data/samples Genomic/data/ref
mv *fastq.gz Genomic/data/samples
mv hg18.fasta Genomic/data/ref
#Create repertories for further analysis
mkdir Genomic/results Genomic/results/alignments Genomic/results/variants
```
## Data quality
The first step of an analysis is to verify the quality of the sequenced data. FastQC is famous for that, mostly for Illumina sequencing. As these fastq.gz are from Illumina paired-end sequencing, FastQC is therefore suitable :
```
~/FastQC/fastqc Genomic/data/samples*fastq.gz
```
This command line lend to the creation of html files for each fastq.gz. This show that the data have a good quality - you can have a look on the screenshots of "Per base sequence quality" below - , and so, we can go further for the analysis. 

**JUPYTER R1**
![](https://raw.githubusercontent.com/SMORTAZA/Genomic_test/master/Screenshot_%20jupyter_R1%20fastq%20gz%20FastQC%20Report.png?token=Abv5cvqDESFGmQzvpX7LLyBwGMR1k6dxks5cWZMJwA%3D%3D)

**JUPYTER R2**
![](https://raw.githubusercontent.com/SMORTAZA/Genomic_test/master/Screenshot_jupyter_R2%20fastq%20gz%20FastQC%20Report.png?token=Abv5cpwh99_txt1NCe9H4hp0__SnSNqiks5cWYy0wA%3D%3D)

**URANUS R1**
![](https://raw.githubusercontent.com/SMORTAZA/Genomic_test/master/Screenshot_uranus_R1%20fastq%20gz%20FastQC%20Report.png?token=Abv5ckr4s5dKwQVFF5OxLnDX-MxURTp3ks5cWYzIwA%3D%3D)

**URANUS R2**
![](https://raw.githubusercontent.com/SMORTAZA/Genomic_test/master/Screenshot_uranus_R1%20fastq%20gz%20FastQC%20Report.png?token=Abv5ckr4s5dKwQVFF5OxLnDX-MxURTp3ks5cWYzIwA%3D%3D)

Out of curiosity, I directly started with the analysis and also because the data was globally correct. Indeed, what should be done is to start by processing the data by removing the low quality sequences ...

## Paired End alignment to the reference genome 

### 1. Indexation of the reference
The reference is the human genome hg18 in a fasta format file. In order to use it in an alignement, it should be indexed (creation of six files). 
```
#launch command lines from Genomic/ repertory
cd Genomic/
~/miniconda3/bin/bowtie2-build data/ref/hg18.fasta data/ref/hg18_indexed
```

### 2. Alignment of the paired reads data on the human reference

#### Mapping of the sequencing reads
```
##for Jupyter patient
~/miniconda3/bin/bowtie2 -x data/ref/hg18_indexed -1 data/samples/jupyter_R1.fastq.gz -2 data/samples/jupyter_R2.fastq.gz > results/alignments/jupyter.sam
#convert sam file on bam file
~/miniconda3/bin/samtools view -Sb results/alignments/jupyter.sam | ~/miniconda3/bin/samtools sort - > results/alignments/jupyter.bam
##for Uranus patient
~/miniconda3/bin/bowtie2 -x data/ref/hg18_indexed -1 data/samples/uranus_R1.fastq.gz -2 data/samples/uranus_R2.fastq.gz > results/alignments/uranus.sam
#convert sam file on bam file
~/miniconda3/bin/samtools view -Sb results/alignments/uranus.sam | ~/miniconda3/bin/samtools sort - > results/alignments/uranus.bam
```

For information :
```
##
#output obtained when Jupyter data were aligned on the reference genome
2186659 reads; of these:
  2186659 (100.00%) were paired; of these:
    537052 (24.56%) aligned concordantly 0 times
    1497886 (68.50%) aligned concordantly exactly 1 time
    151721 (6.94%) aligned concordantly >1 times
    ----
    537052 pairs aligned concordantly 0 times; of these:
      152157 (28.33%) aligned discordantly 1 time
    ----
    384895 pairs aligned 0 times concordantly or discordantly; of these:
      769790 mates make up the pairs; of these:
        383040 (49.76%) aligned 0 times
        273037 (35.47%) aligned exactly 1 time
        113713 (14.77%) aligned >1 times
91.24% overall alignment rate
#output obtained when Uranus data were aligned on the reference genome 
1178831 reads; of these:
  1178831 (100.00%) were paired; of these:
    184608 (15.66%) aligned concordantly 0 times
    891434 (75.62%) aligned concordantly exactly 1 time
    102789 (8.72%) aligned concordantly >1 times
    ----
    184608 pairs aligned concordantly 0 times; of these:
      100177 (54.26%) aligned discordantly 1 time
    ----
    84431 pairs aligned 0 times concordantly or discordantly; of these:
      168862 mates make up the pairs; of these:
        68674 (40.67%) aligned 0 times
        51698 (30.62%) aligned exactly 1 time
        48490 (28.72%) aligned >1 times
97.09% overall alignment rate
```
#### Covered genomic regions & Corresponding genes
In this part, IGV tool is used. To do this, bam files (binary files corresponding to the alignment of patient data on the human reference hg18) are indexed :
```
~/miniconda3/bin/samtools index results/alignments/jupyter.bam
~/miniconda3/bin/samtools index results/alignments/uranus.bam
```
When you have launched IGV tool, go to File > Load from File... > and open bam files. 
![](https://raw.githubusercontent.com/SMORTAZA/Genomic_test/master/chr22_IGV.png?token=Abv5chvSayQvYFd4F87GpzT7PjM9HFFYks5cWY2DwA%3D%3D)

With this tool, you can see the coverage along the genome, or the different chromosomes. As two regions of chromosomes 9 and 22 were enriched during the DNA preparation, before sequencing, we expect a big coverage on these regions. The gene names of the corresponding covered genomic regions are given by the RefSeq (at the bottom of the IGV window).

### 3. Identification of large structural aberrations
The previous steps are present in almost all omic data analysis (Quality checking, Preprocessing, Alignment to a reference). From now what follow is specific to this project.

There are different methods that exist to find large structural aberrations, also known as SV for structural variant. 
Lack of time, I only look for software or tool which detects and identifies large SV :

* **bcftools**
 - https://wurmlab.github.io/genomicscourse/2016-SIB/practicals/population_genetics/map_call
 - note : generalist tool. see the options to use as it is also used for other types of genomic variation. 

* **qSV**
  - https://sourceforge.net/p/adamajava/wiki/qsv/
  - note : Detects structural variants in whole genome paired end or mate pair sequencing.

* **CREST**
  - https://github.com/youngmook/CREST
  - *We developed CREST (Clipping REveals STructure), an algorithm that uses next-generation sequencing reads with partial alignments to a reference genome to directly map structural variations at the nucleotide level of resolution. Application of CREST to whole-genome sequencing data from five pediatric T-lineage acute lymphoblastic leukemias (T-ALLs) and a human melanoma cell line, COLO-829, identified 160 somatic structural variations. Experimental validation exceeded 80% demonstrating that CREST had a high predictive accuracy.* 
  (abstract from the article https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3527068/)
    - note : this tool might produce results close to what is requested in this project.

* **GATK**
  - https://software.broadinstitute.org/gatk/documentation/tooldocs/current/
  - note : used a lot. famous tool ?

* **Belly**
  - https://bpa-csiro-workshops.github.io/btp-manuals-md/modules/cancer-module-sv/sv_tut/
  - note : samplevscontrol. use two types of samples : sample (patient data) and control (?). 
  
Then, we should compare these tools and identify the one that corresponds the best to our expectations.


