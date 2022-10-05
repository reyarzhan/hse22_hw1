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

4. platanus_trim и platanus_internal_trim и их оценка через fastqc и multiqc. Удаляем ненужные файлы
```
platanus_trim sub_oil_*
platanus_internal_trim sub_oilMP*
mkdir trim
mv  -v *trimmed trim

mkdir fastqc_trim
ls *trim | xargs -tI{} fastqc -o fastqc_trim {}

rm sub*
```

5.platanus assemble, platanus scaffold,platanus gap_close.
```
cd trim
time platanus assemble -o Poil -t 8 -m 28 -f sub_oil_R1.fastq.trimmed sub_oil_R2.fastq.trimmed
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub_oil_R1.fastq.trimmed sub_oil_R2.fastq.trimmed -OP2 sub_oilMP_S4_L001_R1_001.fastq.int_trimmed sub_oilMP_S4_L001_R2_001.fastq.int_trimmed
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub_oil_R1.fastq.trimmed sub_oil_R2.fastq.trimmed -OP2 sub_oilMP_S4_L001_R1_001.fastq.int_trimmed sub_oilMP_S4_L001_R2_001.fastq.int_trimmed
```


Notebook - https://colab.research.google.com/drive/10BIKVMK_n4MeSJ8zvVOPJwQ3-k-eaZIH?usp=sharing

Результаты пункта 3

<img width="1472" alt="Screen Shot 2022-10-05 at 19 31 55" src="https://user-images.githubusercontent.com/93340670/194142695-90a57627-a18e-4472-864b-cc0aad1b1af5.png">

Результаты пункта 4

<img width="1476" alt="Screen Shot 2022-10-05 at 19 33 31" src="https://user-images.githubusercontent.com/93340670/194142853-33a786b0-5b39-4af0-8e70-d16b03f97028.png">

