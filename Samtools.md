# Samtools

## Why do I need to know about this tool?

Alignment tool (bowwtie2, tophat etc.) align the reads and the output is stored in SAM files. SAM (Sequence Alignment/Map) is a format for storing read alignments against the reference genome. To get counts for transcripts / do any other kind of downstream analysis, you need to use Samtools.

Fun fact: original [paper](https://academic.oup.com/bioinformatics/article/25/16/2078/204688) has >24000 citations. Tons of people are using this. It is probably a gold standard. (_Is it?_)

## Okay, I'm convinced I need to know this. What's the bare minimum I can run with?

To begin with, 3 main functions: `view`, `sort` and `index`. 

---

### `view` is for converting SAM to BAM. 

What is BAM? - BAM is a binary (machine-readable, not human readable) version of SAM. Makes life easier (_how?_).

`samtools view -S -b -o sample.bam sample.sam`

`view`: convert sam to bam

`-S`: input is a sam (default is bam)

`-b`: output is a bam

`-o`: output file name

---

### `sort` is for placing alignments in genomic order

Alignments are initially in a random order, based on where the reads were in the input `fastq` file. Sorting is useful: for visualizations, for calling variants (_other, better, examples?_). 

`samtools sort sample.bam -o sample.sorted.bam`

`-o`: output file name

After you've run the above command, you can check:

`samtools view sample.sorted.bam | head`

---

### `index` is for indexing files for fast random access

_Is this similar to what a dict is like?_ 

From the [doc](https://www.htslib.org/doc/samtools-index.html): 'This index is needed when region arguments are used to limit samtools view and similar commands to particular regions of interest.'

`samtools index sample.sorted.bam`

The above command creates an index file: `sample.sorted.bam.bai`. 

According to the tutorial, we can use the index to extract alignments from genome regions of interest. E.g. the 33rd megabase of chromosome 1: 

`samtools view sample.sorted.bam 1:33000000-34000000`

To count the number of alignments in this region:

`samtools view sample.sorted.bam 1:33000000-34000000 | wc -l`

_Got weirdness when I tried this._


---

## Ooh what other juicy details do you have?

### `flagstat` is for getting alignment details

`samtools flagstat SRR1916544.bam`

It gives you the number of reads that passed and failed quality control. From the [doc](https://www.htslib.org/doc/samtools-flagstat.html), here's an example output: `122 + 28 in total (QC-passed reads + QC-failed reads)`. This means you had 150 reads in total. Of these 122 passed the quality control. Has option to write output to file.




# Resources

1. Short, sweet getting started tutorial [here](http://quinlanlab.org/tutorials/samtools/samtools.html). 

1. There are a ton of other options: Read the [doc](https://www.htslib.org/doc/samtools.html) for more. (_Someday_)