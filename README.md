# MyGenome
Analyses for ABT480/CS485G genome assembly

## 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```bash
ssh -Y mdba238@mdba238.cs.uky.edu
cd MyGenome
export _JAVA_OPTIONS='-Dsun.java2d.xrender=false'
fastqc &
```
Load F1 and R1 datasets into GUI interface. 
Take screenshots of output files
Forward Paired:
![F1screenshot.png](/F1_screenshot.png)
Reverse Paired:
![R1screenshot.png](/R1_screenshot.png)

## 2. Ran trimmomatic
```bash
java -jar ~/sequences/trimmomatic-0.38.jar PE -threads 16 -phred33 -trimlog file.txt
input_forward.fq input_reverse.fq
forward_paired.fastq forward_unpaired.fastq
reverse_paired.fastq reverse_unpaired.fastq
ILLUMINACLIP:adaptors.fasta:2:30:10 SLIDINGWINDOW:20:20 MINLEN:100
```

## 3. Count the number of forward reads remaining
```bash
grep -c @A00261 forward_paired.fastq 
cat forward_paired.fastq reverse_paired.fastq | awk 'NR%4==2 {total += length($0)} END {print total}'
```

## 4. Velvet optimizer
```bash
sbatch velvetoptimiser_noclean.sh UFVPY113 61 131 10
```
Then, I found that the hash size was 110 so I ran:
```bash
sbatch velvetoptimiser_noclean.sh UFVPY113 91 111 2
```

