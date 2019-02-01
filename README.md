# Genomic_test

Aide de 
* https://wurmlab.github.io/genomicscourse/2016-SIB/practicals/population_genetics/map_call
* https://espace.library.uq.edu.au/data/UQ_371011/s4112726_phd_submission.pdf?Expires=1549102895&Signature=GKGXS9BgDZag5b9CWFFBMc2umk2-rDSFXGNDSishFB~ev-8WzUJipNHiDxgT8wE4NlRhcTRF2LBkbS1RNRIoIEBdp22chRQGzrj0NrVb9abvsf5g9L~LggtH9n5OjyMrga4mbhTJEKi4Nud60LYvPFol8L-1C5soAR2Xp5EP-SQq5G6sp2CztwKvfvpTEyKwUE68N77GSn3nICx7zKFscnucFiF2MM0dTXBLe2vgtvyDPNZKQxMuJ5Ao7ypjx5P6el9vM0h2hfb1HyDz3dGa8pZrkfH5HQjy0k1Yvl3o0H49DaHZVsL-CQY-WLtCD3tQN2rIBwpoAN6GPqqr63pjjQ__&Key-Pair-Id=APKAJKNBJ4MJBJNC6NLQ
* https://software.broadinstitute.org/gatk/documentation/tooldocs/current/
* http://bioinformatics.org.au/ws14/wp-content/uploads/ws14/sites/5/2014/07/Ann-Marie-Patch_presentation.pdf

---------------------------------------------------------------------
1- Téléchargement des données
wget adresse des liens des 4 fastq.gz

2- Téléchargement de la référence human genome CRCh38/hg18 - version nucléotidique
"""{}
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/GCA_000001405.15_GRCh38_genomic.fna.gz
gunzip pour décompresser
"""{}

3- Téléchargement de l'annotation de hg18
----------------------------------------------
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/GCA_000001405.15_GRCh38_genomic.gff.gz
----------------------------------------------
gunzip pour décompresser
 
4- téléchargement du script d’installation de Miniconda : wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh puis suivre les indications pour terminer l'installation

5- Installation bowtie2 2.3.4.3 avec 
* conda search bowtie2
* conda install bowtie2
rq : utilisation des lignes de commandes à partir du répertoire /miniconda3/bin/

6- 
