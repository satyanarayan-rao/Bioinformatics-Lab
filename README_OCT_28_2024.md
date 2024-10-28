## Knowing the where part

As we discussed in the class, sequecing enables where things are happening.
Having the reference genome helped us to map inserts to human genome. The BAM
file has a lot of information. But, manier times, we are just interested in the
location. To achieve that, we will extract the mapping information from the BAM
file you have generated from the first documentation [part](./README.md).

### Installing bedtools

First, login to ParamGanga, and activate the virtual environment you have been working on (`bt_608_practical`).
```
mamba install -c bioconda bedtools

```

Once `bedtools` is installed, you can run the following:

```
cd mapped

samtools view -bf 0x2 demo_ih02.bam | bedtools bamtobed -i stdin -bedpe -mate1 |  awk '{if ($NF == "-"){print $1"\t"$2"\t"$6"\t"$7"\t"$8"\t+"}else {print $1"\t"$5"\t"$3"\t"$7"\t"$8"\t+"}}' | sort -k1,1 -k2,2n -k3,3n  | gzip - > demo_ih02.bed.gz  
```

