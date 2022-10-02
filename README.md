# hse22_hw1
# 1. Основная часть 
**Ссылки**
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```
**Random seed** (выбираю рандомные чтения)  
Seed = 713 (07.01.2003)  
```
seqtk sample -s713 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s713 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s713 oilMP_S4_L001_R1_001.fastq 1500000 > mps1.fastq
seqtk sample -s713 oilMP_S4_L001_R2_001.fastq 1500000 > mps2.fastq
```
**Статистика по исходникам**  
Создаю директории для анализа:  
```
mkdir fastqc
mkdir miltiqc
```
