# Lab_5_genome_annotation_part2

&ensp;
**OUTLINE**

[Step 1 - Lab setup](#step-1---lab-setup)

[Step 2 - Functional Annotation](#step-2---functional-annotation)

[Lab Question 1](#lq-1)

[Step 3 - Generate protein sequences](#step-3---generate-protein-sequences)

[Lab Question 2](#lq-2)

[Step 4 - Upload files to GenomeQC](#step-4---upload-files-to-genomeqc)

[Step 5 - Submit your job](#step-5---submit-your-job)

[Step 6 - Record your Annotation Metrics results](#step-6---record-your-annotation-metrics-results)



[Step 7 - Generate your DNA coding sequences](#step-7---generate-your-dna-coding-sequences)

[Lab Question 2](#lq-2)

[Step 8 - Filter your CDS file](#step-8---filter-your-cds-file)

[Lab Question 3](#lq-3)

[Step 9 - Generate your protein-coding sequences](#step-9---generate-your-protein-coding-sequences)

[Lab Question 4](#lq-4)

[Step 10 - BUSCO results](#step-10---busco-results)

[LAb Question 5](#lq-5)


We have now annotated our genomes. This means we have identified locations in the genome where we believe genes are located. 
This week in the lab, we will be analyzing our genome annotations. 

Specifically, we will be doing a **functional annotation.**

This will tell us **what** our genes are, not just just _where_.

&ensp;
## Step 1 - Lab setup

We will be using several files from lab 4 this week. 

&ensp;
&ensp;

### Step 1a
First, create a new folder in your home directory for this week's lab. 

```bash
#go to home directory
cd

#make a new directory
mkdir lab_5
```
&ensp;
&ensp;

### Step 1b

From your home directory (before moving into lab_5), you can copy your files using a command below, replacing the XXXX with your SRR number.

```bash
cp lab_4/SRRXXXXXXX/predict_results/SRRXXXXXXX.cds-transcripts.fa lab_5/.
cp lab_4/SRRXXXXXXX/predict_results/SRRXXXXXXX.gff3 lab_5/.
cp lab_4/SRRXXXXXXX/predict_results/SRRXXXXXXX.scaffolds.fa lab_5/.
```

&ensp;
&ensp;
&ensp;

## Step 2 - Functional Annotation

We will use funannotate again. This time, we will conduct a functional annotation.

## Step 2a - Copy and edit slurm script

Copy the annotation slurm file from the project space

```bash
cp /projects/class/binf3101_001/lab_5/annotate.slurm .
```

Now you will need to edit the `annotate.slurm` file to replace SRRXXXXX with your SRR number. 

### Step 2b - Submit the job and ensure it is running

Submit the script
```bash
sbatch annotate.slurm
```

Check to see if it is running

```bash
squeue -u username
```

This should take ~10 minutes. While you wait, answer the question below. 

&ensp;
&ensp;

# LQ 1
The funannotate command we used was
`funannotate annotate --gff SRRXXXXXX.gff3 --fasta SRRXXXXXX.scaffolds.fa -s SRRXXXXXX -o SRRXXXXXX --cpus 4`

As input, it took two files:
1 - Genome annotation `SRRXXXXXX.scaffolds.fa`
2 - GFF3 file `SRRXXXXXX.gff3`

Read about the GFF3 file here: https://useast.ensembl.org/info/website/upload/gff3.html 

**Question** - Select the **types** found in our GFF3 files

&ensp;
&ensp;

## Step 3 - Generate protein sequences

If you look at the `SRRXXXXXXX.cds-transcripts.fa` file you will see that it is DNA. We will need to translate this DNA sequence into protein sequences for the steps next week. 

To translate our DNA into Protein we use the codon table. This will predict the protein-coding sequences for all of our genes. 

In class we discussed that the genetic code (the codon table) is _nearly_ universal. In the budding yeast, there are **three** different genetic codes in the nuclear DNA. 

&ensp;
&ensp;

## Step 3a - Determine your genetic code

Go to this table and determine which Order your species belongs to 

https://docs.google.com/spreadsheets/d/1EuW_gBTT3Epl0tYhHk68UUEkpcYa0N2Q1w90fKzuhr8/edit?usp=sharing

Based on the order your species is in select the correct genetic code
|Order	|Codon Table|
|------|------|
|Alaninales|	12|
|Dipodascales| 1|
|Lipomycetales| 1|
|Phaffomycetales|	1|
|Pichiales|	1|
|Saccharomycetales|	1|
|Saccharomycodales|	1|
|Serinales|	12|
|Sporopachydermiales|	1|


A description of each code can be found here: https://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi

&ensp;

# LQ 2
What is the **name** of your genetic code listed on the NCBI website?

&ensp;
&ensp;

## Step 3b - Translate your DNA to Proteins 

We will use this next week! 

We will use transeq to translate our DNA into Protein 

Use the command below but replace
INPUT = SRRXXXXXXX.cds-transcripts.fa
OUTPUT = SRRXXXXXXX.prot.fa
CODON_TABLE = the **number** of your codon table above

```
module purge
module load emboss
transeq -sequence INPUT -outseq OUTPUT -table CODON_TABLE
```

&ensp;
&ensp;

## Step 4 - Analyze your annotation results

Once the annotation process is done you will be able to find a file `SRRXXXXXX/annotate_results/SRRXXXXXXX.annotations.txt

We will be analyzing that file so move to that folder. 

### Step 4a - Number of genes annotated

The "name" of our gene will be in the 8th column of the annotation.txt file. 

To calculate this, we will need to string together several commands. 

**Command 1** - get only the 8th column
`cut -f8 SRRXXXXX.annotations.txt`

**Command 2** - sort the 8th column
`sort`

**Command 3** - get only the unique values 

Now let's string them all together using the pipe `|` command so that the results of one command get sent to the next 

`cut -f8 SRRXXXXX.annotations.txt | sort | uniq`

**Command 4** - now we want to count the number of genes using `wc -l` which counts the number of lines 

`cut -f8 SRRXXXXX.annotations.txt | sort | uniq | wc -l`

&ensp;

# LQ 3

How many genes were functionally annotated in your genome?







