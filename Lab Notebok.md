# Lab Notebook - Dominic Carignan

PS1='$ '
# setting a variable to get rid of the text on the command line

cd
# change directory

ls
# this lists all of the files in my current working directory

pwd
# this stands for print working directory 

ctrl-c
# cancel current cammand line and get a new one

clear
# clears all code in the terminal

User Name/Password
# dec1040
# dec1040zfG1

ssh username@servername.edu
# connects terminal to teaching cluster RON



## Lab 2 2/10/23



mkdir directoryname
# creates directory

cp /
# copies a file

tar -zxf shell_data.tar
# sets specific settings for the file

head filename
# opens fastq files

ls -F
# gives more info about directories

ls -lrth
# shows who has access to directories

grep '@SRR097977' SRR097977.fastq
# acts as a "Word Finder" for code

cd ~
# brings back to home directory

rm
# removes a file


## Lab 3 2/17/23

cd ../
# go back a directory

cd /
# starts an absolute path

cd /home/users/dec1040/gen711
# will bring you to your folder from anywhere

Excercise 3a.
1. cd ~
2. cd ../../../
3. cd /home/users/dec1040

Excercise 3b.
Hidden File
# .hidden/youfoundit.txt
# "Here I am"

cat filename filename filename
# opens entire file into terminal
# can also string files together

*
# "wildcard" will list files with anything in place of the "*"

Excercise 3c.
1. Start with C = 90
2. Start with A = 45
3. Start with O = 22

echo
# repeats back whatever you say
# can interpret "*"

> 
# creats a new file

>> 
# adds to an existing file

ctrl R
# search through command history

history
# shows cammand history

history | grep 'cammand'
# search for a specific cammand


## Lab 4 2/24/23

realpath 'directory'
# shows absolute path to a directory

mdir -p
# creats new directory 

gunzip
# unzips .gz files

less -S 'filename'
# shows files in more organized way

ls -lh
# shows the total size and individual sizes of all files in the directory

conda activate genomics
# opens an "environment"

fastqc
# summarizes quality of files

ctrl z
# stops a process

bg
# runs a process in the background

unzip 'filename'
# unzips .zip files


## Lab 5 3/3/23

sftp dec1040@ron.sr.unh.edu
# "secure file transfer protocol" (must be done in a new terminal)

get 'filename.png'
# opens an image file in VSCODE to be able to view it

trimmomatic PE -threads 4 SRR_1056_1.fastq SRR_1056_2.fastq \
SRR_1056_1.trimmed.fastq.gz SRR_1056_1un.trimmed.fastq.gz \
SRR_1056_2.trimmed.fastq.gz SRR_1056_2un.trimmed.fastq.gz \
ILLUMINACLIP:SRR_adapters.fa SLIDINGWINDOW:4:20
# used for trimming paired-end reads

Excerise 
1. make a folder called ‘trimmed_fastq’ in data
    
    # mkdir -p trimmed_fastq

2. move all files that we made (hint: .trim and the move command, and the destination directory will work)

    # cp /tmp/trimmed/*.gz ~/dc_workshop/data/trimmed_fastq/

3. Re-run fastqc on the trimmed fastqs in the trimmed_fastq folder.

    # fastqc *fastq.gz


## Lab 6 3/24/23

curl -L -o 
# downloads a file from an html page

mv
# moves files between directories

tar xvf 'filename'
# saves files to a different file

bwa index
# speeds up alignments

bwa mem
# aligns sequence reads

samtools view -S -b 'path'
# creates a bam file from a sam file

samtools sort 
# sorts bam files

bcftools mpileup -Ob -o 
# compares bam file to the reference genome and looks for differences


## Lab 7 3/31/23

Absolute paths for download files
# /home/users/dec1040/dc_workshop/data/ref_genome/ecoli_rel606.fasta*

# /home/users/dec1040/dc_workshop/results/bam/SRR2584866.aligned.sorted.bam*

# /home/users/dec1040/dc_workshop/results/vcf/SRR2584866_final_variants.vcf*

Excercise:
# Variant at position 4,377,265 is a mutation from an A in the reference genome to a G in the variant.



## Lab 8 4/14/23

https://github.com/jthmiller/gen711-811-example

# Working on Cyanobacteria project

## Lab 9 4/21/23

Conda activate genomics

cp /tmp/gen711_project_data/fastp.sh Gen-711-Final-Project/fastp.sh
# copies fastp.sh into final project directory (shell script)

chmod +x Gen-711-Final-Project/fastp.sh

Gen-711-Final-Project/fastp.sh 150 /tmp/gen711_project_data/cyano/fastqs  trimmed_fastq
# runs the shell script with 150 poly g length and redirecting the data to trimmed_fastq

conda activate qiime2-2022.8

qiime tools import --type SampleData[PairedEndSequencesWithQuality] --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path trimmed_fastq --output-path Gen_711-Final-Project/qiime_file.qza
# this works

qiime cutadapt trim-paired \
    --i-demultiplexed-sequences qiime_file.qza \
    --p-cores 4 \
    --p-front-f GTGYCAGCMGCCGCGGTAA \
    --p-front-r CCGYCAATTYMTTTRAGTTT \
    --p-discard-untrimmed \
    --p-match-adapter-wildcards \
    --verbose \
    --o-trimmed-sequences Gen-711-final-Project/qiime_trimmed.qza

qiime demux summarize \
--i-data qiime_trimmed.qza \
--o-visualization  Gen-711-Final-Project/qiime_visualization.qzv

Denoising

qiime dada2 denoise-paired \
    --i-demultiplexed-seqs qiime_out/${run}_demux_cutadapt.qza  \
    --p-trunc-len-f ${trunclenf} \
    --p-trunc-len-r ${trunclenr} \
    --p-trim-left-f 0 \
    --p-trim-left-r 0 \
    --p-n-threads 4 \
    --o-denoising-stats denoising/denoising-stats.qza \
    --o-table denoising/feature_table.qza \
    --o-representative-sequences denoining/rep-seqs.qza

qiime metadata tabulate \
    --m-input-file denoising/denoising-stats.qza \
    --o-visualization denoising/denoising-stats.qzv

qiime feature-table tabulate-seqs \
        --i-data denoising/rep-seqs.qza \
        --o-visualization denoising/rep-seqs.qzv
        
Extra commands 
/tmp/gen711_project_data/cyano/fastqs
# pulls the cyanobacteria data from online

git version
# gets you into git hub commands

git copy https://github.com/dec1040/Gen-711-Final-Project.git
# copies my github into ron

