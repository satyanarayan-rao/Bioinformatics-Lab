# Bioinformatics-Lab

This documentation assumes that you have:

1. Installed Miniforge successfully. Meaning that `(base)` appears when you login to ParamGanga. 

2. You have created a virtual environment `bt_608_practical` (or any other name  I can't recall what we named during the class)

3. You have downloaded sequencing data shared on figshare (check email)

4. You have activated the virtual environment

First of all you should create the following directory structure:

Type the following command

```
cd 
mkdir -p workplace/projects/cfdna_analysis
cd workplace/projects/cfdna_analysis
mkdir data
mkdir bowtie2_indices

```

In the class, we made structures till `data` directory. And, most of you have downloaded indices. Please follow below. 

## Alignment of trimmed reads to human genome

### Downloading Bowtie2 indices for human genome
You have already downloaded bowtie2 indices. The next step is to align reads to the reference genome (Human in this case).

If you have not downloaded the indices, pleasr RIGHT CLICK on [this](https://genome-idx.s3.amazonaws.com/bt/GRCh38_noalt_as.zip) link, and copy link.

Run the following command to download the indices on ParamGanga (PG). 

```
cd bowtie2_indices 
curl -JLO https://genome-idx.s3.amazonaws.com/bt/GRCh38_noalt_as.zip 
```
See if download is happening fast or not. If it is not, then you can press `CRTL + c`. Delete the partially downloaded file. Use the following command to do that.

```
rm GRCh38_noalt_as.zip 
```    
NOTE: the whole idea is to keep downloaded index zip file in the `bowtie2_indices` directory. So, if you have downloaded it already, then simply move that zip file to `bowtie2_indices` directory.


### Running alignment command

```
mkdir mapped
bowtie2 --threads 2 -1 data/ih02_R1.fastq.gz -2 data/ih02_R2.fastq.gz -x bowtie2_indices/GRCh38_noalt_as/GRCh38_noalt_as --local --very-sensitive-local --no-unal --no-mixed --no-discordant -I 10 -X 700 | samtools view -h -bS  > mapped/demo_ih02.bam
```

See if samtools command is there (just type `samtools` and press enter). If not, then do the following:

```
mamba install -c bioconda samtools
```

If you already have samtools then run the following:

```
cd mapped
samtools sort -@4 demo_ih02.bam -o sorted_demo_ih02.bam
samtools index sorted_demo_ih02.bam
```

Now you will need to transfer `sorted_demo_ih02.bam` and `sorted_demo_ih02.bam.bai` to your local machine to visualize in IGV. 

#### Copying from ParamGanga

To copy from server you have to run the following command

Linux/Mac Users:

Open terminal and go to a desired directory. For example, Downloads. 

NOTE: please replace `<username>` with your ParamGanga username.
```
cd Downloads
scp -o StrictHostKeyChecking=no  -o UserKnownHostsFile=/dev/null <username>@paramganga.iitr.ac.in:~/workplace/projects/cfdna_analysis/mapped/sorted* . 
```

For Windows users. Open Powershell, and go to Downloads (or other directory you wish). Run the following command

```
scp <username>@paramganga.iitr.ac.in:~/workplace/projects/cfdna_analysis/mapped/sorted* .
```

## Install IGV

Visit [here](https://igv.org/doc/desktop/#DownloadPage/) and download accordingly to your operating system.

### Visualizing alignment on IGV

open IGV and select human (hg38) as genome. In File Menu, select `Load from file...`. Select the `bam` file. It will take some time to load.

Once loaded type `chr9:9,998,492-9,998,642` and press `Go`. Let it load. Visualize and explore yourself.
