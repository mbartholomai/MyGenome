# MyGenome
## Sequence Quality Assessment
- To get the total number of reads in remaining paired sequences, run: grep -c @A00261 forward_paired.fastq
- To get the total number of bases in remaining paired sequences, run: cat forward_paired.fastq reverse_paired.fastq | awk 'NR%4==2 {total += length($0)} END {print total}'
## Random Help
### X11 Forwarding
- Run: export _JAVA_OPTIONS='-Dsun.java2d.xrender=false'
- Then, run: fastqc &
