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

With this tool, you can see the coverage along the genome, or the different chromosomes. There are reads corres

### 3. Identification of large structural aberrations
There are different methods that exist to find large structural aberrations, also known as SV for structural variant. 
Lack of time, I only look for software or tool which detects and identifies large SV. 
* qSV
* CREST
* GATK
* Belly



--------------------------------------------------------------------------------

Aide de 
* https://wurmlab.github.io/genomicscourse/2016-SIB/practicals/population_genetics/map_call
* https://espace.library.uq.edu.au/data/UQ_371011/s4112726_phd_submission.pdf?Expires=1549102895&Signature=GKGXS9BgDZag5b9CWFFBMc2umk2-rDSFXGNDSishFB~ev-8WzUJipNHiDxgT8wE4NlRhcTRF2LBkbS1RNRIoIEBdp22chRQGzrj0NrVb9abvsf5g9L~LggtH9n5OjyMrga4mbhTJEKi4Nud60LYvPFol8L-1C5soAR2Xp5EP-SQq5G6sp2CztwKvfvpTEyKwUE68N77GSn3nICx7zKFscnucFiF2MM0dTXBLe2vgtvyDPNZKQxMuJ5Ao7ypjx5P6el9vM0h2hfb1HyDz3dGa8pZrkfH5HQjy0k1Yvl3o0H49DaHZVsL-CQY-WLtCD3tQN2rIBwpoAN6GPqqr63pjjQ__&Key-Pair-Id=APKAJKNBJ4MJBJNC6NLQ
* https://software.broadinstitute.org/gatk/documentation/tooldocs/current/
* http://bioinformatics.org.au/ws14/wp-content/uploads/ws14/sites/5/2014/07/Ann-Marie-Patch_presentation.pdf


#### 10. outil pour retrouver les large structural aberrations

- https://sourceforge.net/p/adamajava/wiki/qsv/
