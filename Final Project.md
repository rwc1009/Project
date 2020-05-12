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

Build a blast database with the command:

```
makeblastdb -in bees.fa -out bees -dbtype nucl
```

Perform the blast with the command:

```
blast -db bees -max_target_seqs 1 -query bees.fa
```

Download MAFFT using the command:

```
conda install -c bioconda mafft
```
