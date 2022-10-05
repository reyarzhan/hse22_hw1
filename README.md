# hse22_hw1

1. Сначала сделаем символическую ссылку на каждый из файлов в своей папке \
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```

2. С помощью команды seqtk выбираем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs \
```
seqtk sample -s710 oil_R1.fastq 5000000 > sub_oil_R1.fastq
seqtk sample -s710 oil_R2.fastq 5000000 > sub_oil_R2.fastq
seqtk sample -s710 oilMP_S4_L001_R1_001.fastq 1500000 > sub_oilMP_S4_L001_R1_001.fastq
seqtk sample -s710 oilMP_S4_L001_R2_001.fastq 1500000 > sub_oilMP_S4_L001_R2_001.fastq
```

3. Оцениваю качество исходных чтений
```
mkdir fastqc
ls sub* | xargs -tI{} fastqc -o fastqc {}

mkdir multiqc
multiqc -o multiqc fastqc
```

