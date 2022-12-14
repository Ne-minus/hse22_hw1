# hse22_hw1
# [ссылка на Jupyter notebook](https://github.com/Ne-minus/hse22_hw1/blob/main/src/python_code.ipynb)
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
mkdir multiqc
```
Получаю чтения через fastqc и затем получаю статистику через miltiqc  
```
ls sub* mps* | xargs -tI{} fastqc -o fastqc {}
multiqc -o multiqc fastqc
```
![Статистика до подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/multi1.png)
![Статистика до подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/multi2.png)
Подрезаю и удаляю адаптеры
```
platanus_trim sub*
platanus_internal_trim mps*
```
Удаляю исходники
```
rm sub1.fastq sub2.fastq mps1.fastq mps2.fastq
```
Статистика по новым подрезанным файлам  
```
mkdir fast_trim
ls sub* mps*| xargs -tI{} fastqc -o fast_trim {}
mkdir multi_trim
multiqc -o multi_trim fast_trim
```
![Статистика после подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/multi_trim1.png)
![Статистика после подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/multi_trim2.png)
**Удаляю все .fastq файлы**
```
rm *fastq
```
**Сборка контигов и скаффолдов**  
Собираю контиги
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```
Собираю скаффолды
```
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mps1.fastq.int_trimmed mps2.fastq.int_trimmed 2> scaffold.log
```
Уменьшаю количество гэпов
```
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mps1.fastq.int_trimmed mps2.fastq.int_trimmed 2> gapclose.log
```
# 2. Дополонительная часть  
Делаю все те же команды в терминали только при значениях 500000(pired-end) и 150000(mate-pairs):
```
seqtk sample -s713 oil_R1.fastq 500000 > sub3.fastq
seqtk sample -s713 oil_R2.fastq 500000 > sub4.fastq
seqtk sample -s713 oilMP_S4_L001_R1_001.fastq 150000 > mps3.fastq
seqtk sample -s713 oilMP_S4_L001_R2_001.fastq 150000 > mps4.fastq
mkdir fastqc1
ls sub* mps* | xargs -tI{} fastqc -o fastqc1 {}
mkdir multiqc2
multiqc -o multiqc2 fastqc1
platanus_trim sub*
platanus_internal_trim mps*
mkdir fast_trim1
ls sub* mps*| xargs -tI{} fastqc -o fast_trim1 {}
mkdir multi_trim1
multiqc -o multi_trim1 fast_trim1
time platanus assemble -o Poil -f sub3.fastq.trimmed sub4.fastq.trimmed 2> assemble.log
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub3.fastq.trimmed sub4.fastq.trimmed -OP2 mps3.fastq.int_trimmed mps4.fastq.int_trimmed 2> scaffold.log
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub3.fastq.trimmed sub4.fastq.trimmed -OP2 mps3.fastq.int_trimmed mps4.fastq.int_trimmed 2> gapclose.log
```
Статистика для неподрезанных:
![Статистика до подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/bonus1.png)
![Статистика до подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/bonus2.png)
После подрезания:
![Статистика после подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/bonus_trim1.png "Статистика после подрезания")
![Статистика после подрезания](https://github.com/Ne-minus/hse22_hw1/blob/main/pngs/bonus_trim2.png)
**Вывод**  
После того, как было изменено количество чтений, было найдено больше контигов и скаффолдов. Их получилось очень много (также выросло значение N50) --> качество выборки стало хуже.
