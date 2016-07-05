# Installation of some bioinformatics tools

**bedtools**
`sudo apt-get install bedtools`

**bcbio-nextgen**
```bash
wget https://raw.github.com/chapmanb/bcbio-nextgen/master/scripts/bcbio_nextgen_install.py
sudo python bcbio_nextgen_install.py /usr/local/share/bcbio --tooldir=/usr/local --genomes GRCh37 --aligners bwa --aligners bowtie2

sudo apt-get install libgsl2
sudo apt-get install gsl-bin
sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update
sudo apt-get install openjdk-7-jdk
sudo apt-get install openjdk-7-jre
```

**homer**
```bash
wget http://homer.salk.edu/homer/configureHomer.pl
perl configureHomer.pl -install
perl configureHomer.pl -install hg19
```
