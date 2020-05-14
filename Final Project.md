Download anaconda with the command:

```
mkdir anaconda
cd anaconda
curl -LO https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh
bash Anaconda3-5.1.0-Linux-x86_64.sh -b -p $HOME/anaconda/install/
echo ". $HOME/anaconda/install/etc/profile.d/conda.sh" >> ~/.bashrc
source ~/.bashrc
conda update -n base conda
```

Download tmux with the command:

```
conda install -c conda-forge tmux
```

Create tmux window with the command:

```
tmux new -s project
```

Download the first FASTQ file from https://www.ebi.ac.uk/ena/data/view/SRX3444921 using the command:

```
curl -LO ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR634/001/SRR6347831/SRR6347831_1.fastq.gz
```

Download the second FASTQ file from https://www.ebi.ac.uk/ena/data/view/SRX3444921 using the command:

```
curl -LO ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR634/001/SRR6347831/SRR6347831_2.fastq.gz
```

Unzip the files using the commands:

```
gzip -d SRR6347831_1.fastq.gz

gzip -d SRR6347831_2.fastq.gz
```

Combine the files into a single file using the command:

```
cat SRR6347831_1.fastq SRR6347831_2.fastq > bees.fastq
```

Convert the fastq files to fasta files using the command:

```
paste - - - - < bees.fastq | cut -f 1,2 | sed 's/^@/>/' | tr "\t" "\n" > bees.fa
```

Download the microsatellite marker Am0283=AT020, which is within the sting-1 QTL (sting-1 is a region of DNA associated with aggressive behavior) from https://www.ncbi.nlm.nih.gov/nuccore/AJ509514 with the command:

```
esearch -db nucleotide -query "AJ509514" | efetch -format fasta > gene.fasta
```

Build a blast database with the command:

```
makeblastdb -in bees.fa -out bees -dbtype nucl
```

Perform the blast with the command:

```
blastn -db bees -max_target_seqs 1 -query gene.fasta -outfmt '6 qseqid qlen length pident gaps evalue stitle' -evalue 1e-10 -num_threads 10 -out blast.out
```

Remove the sequence of interest (SRR6347831.14905832) after looking in the blast.out file with the command:

```
grep -i -A 1 "SRR6347831.14905832" bees.fa > bee_genes.fa
```

Combine both genes of interest with the command:

```
cat bee_genes.fa gene.fasta > final.fa
```

Download MAFFT using the command:

```
conda install -c bioconda mafft
```

Transform the final.fa file into a file that can be viewed in a multiple sequence alignment viewer using the command:

```
mafft --reorder --localpair --thread 10 final.fa > final.aln
```

Look at the file with

```
cat final.aln
```

Copy the file and go to the website https://www.ebi.ac.uk/Tools/msa/mview/

Paste the data into the text box and set the input to DNA. Click submit and view the results

The results should look like this:
