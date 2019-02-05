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

--------------------------------------------------------------------------------

Aide de 
* https://wurmlab.github.io/genomicscourse/2016-SIB/practicals/population_genetics/map_call
* https://espace.library.uq.edu.au/data/UQ_371011/s4112726_phd_submission.pdf?Expires=1549102895&Signature=GKGXS9BgDZag5b9CWFFBMc2umk2-rDSFXGNDSishFB~ev-8WzUJipNHiDxgT8wE4NlRhcTRF2LBkbS1RNRIoIEBdp22chRQGzrj0NrVb9abvsf5g9L~LggtH9n5OjyMrga4mbhTJEKi4Nud60LYvPFol8L-1C5soAR2Xp5EP-SQq5G6sp2CztwKvfvpTEyKwUE68N77GSn3nICx7zKFscnucFiF2MM0dTXBLe2vgtvyDPNZKQxMuJ5Ao7ypjx5P6el9vM0h2hfb1HyDz3dGa8pZrkfH5HQjy0k1Yvl3o0H49DaHZVsL-CQY-WLtCD3tQN2rIBwpoAN6GPqqr63pjjQ__&Key-Pair-Id=APKAJKNBJ4MJBJNC6NLQ
* https://software.broadinstitute.org/gatk/documentation/tooldocs/current/
* http://bioinformatics.org.au/ws14/wp-content/uploads/ws14/sites/5/2014/07/Ann-Marie-Patch_presentation.pdf


#### 10. outil pour retrouver les large structural aberrations

- https://sourceforge.net/p/adamajava/wiki/qsv/
