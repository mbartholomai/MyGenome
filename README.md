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

## 5. Rename sequence headers to a standard format
```bash
perl /project/farman_s24cs485g/SCRIPTs/SimpleFastaHeaders.pl UFVPY113.fasta
```

## 6. Check Genome completeness using BUSCO
```bash
sbatch BuscoSingularity.sh UFVPY113/velvet_UFVPY113_91_111_2_noclean/UFVPY113_nh.fasta
```

## 7. Remove contigs less than 200 base pairs long
```bash
perl CullShortContigs.pl UFVPY113_nh.fasta
```

## 8. BLAST the MoMitochondrion.fasta sequence against the final genome assembly
```bash
blastn -query MoMitochondrion.fasta -subject UFVPY113_nh.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid slen length qstart qend sstart send btop' -out MoMitochondrion.UFVPY113.BLAST
```

## 9. Export a list of contigs that mostly comprise mitochondrial sequences
```bash
awk '$4/$3 > 0.9 {print $2 ",mitochondrion"}' MoMitochondrion.UFVPY113.BLAST > UFVPY113_mitochondrion.csv
```

## 10. BLAST the genome assembly against a repeat-masked version of the B71 reference genome
```bash
blastn -query B71v2sh_masked.fasta -subject UFVPY113_final.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid qstart qend sstart send btop' -out B71v2sh.UFVPY113.BLAST
```

## 11. Identify genetic variants between the B71v2sh genome and the genome assembly
```bash
sbatch CallVariants.sh UFVPY113_BLASTS
```


