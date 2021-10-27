# hse21_hw1

## Команды, которые были выполнены на сервере

```
ln -s /usr/share/data-minor-bioinf/assembly/oil* .

seqtk sample -s127 oil_R1.fastq 5000000 > pair_end_1.fastq
seqtk sample -s127 oil_R2.fastq 5000000 > pair_end_2.fastq
seqtk sample -s127 oilMP_S4_L001_R1_001.fastq 1500000 > mp_1.fastq
seqtk sample -s127 oilMP_S4_L001_R2_001.fastq 1500000 > mp_2.fastq

mkdir fastqc multiqc
fastqc pair*.fastq mp*.fastq -o fastqc
multiqc fastqc -o multiqc

platanus_trim pair*.fastq
platanus_internal_trim mp*.fastq

rm pair*.fastq mp*.fastq

fastqc *trimmed -o fastqc
multiqc fastqc -o multiqc

platanus assemble -f *.trimmed
platanus scaffold -c out_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed
platanus gap_close -c out_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed
```
## Статистика

### Исходные чтения
Файл: https://github.com/A-Archibasova/hse21_hw1/blob/main/multiqc/multiqc_report.html

<img width="1114" alt=" main" src="https://user-images.githubusercontent.com/71605966/139133527-ef063b98-f5d1-4081-8fe6-666aaf30a5f8.png">

### Подрезанные чтения
Файл: https://github.com/A-Archibasova/hse21_hw1/blob/main/multiqc/multiqc_report_transform.html

<img width="1121" alt="transform" src="https://user-images.githubusercontent.com/71605966/139133637-9f7d495c-7c8a-4640-ba82-b6f1f1ecc67f.png">

## Анализ


