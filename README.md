# MyGenome
Analyses for ABT480/CS485G genome assembly

## 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```
ssh -Y mdba238@mdba238.cs.uky.edu
cd MyGenome
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
java -jar...
```

## 3. Count the number of forward reads remaining
```bash
grep -c @A00261 forward_paired.fastq 
awk 'NR%4==2 {total += length($0)} END {print total}' forward_paired.fastq 
awk 'NR%4==2 {total += length($0)} END {print total}' reverse_paired.fastq
```
