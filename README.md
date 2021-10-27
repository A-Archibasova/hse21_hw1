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

sed -n '1,/^>/p' out_scaffold.fa | head -n -1 >longest.fa
sed -n '1,/^>/p' out_gapClosed.fa | head -n -1 >longest_gap.fa
```
## Статистика

### Исходные чтения
Файл: https://github.com/A-Archibasova/hse21_hw1/blob/main/multiqc/multiqc_report.html

<img width="1114" alt=" main" src="https://user-images.githubusercontent.com/71605966/139133527-ef063b98-f5d1-4081-8fe6-666aaf30a5f8.png">

### Подрезанные чтения
Файл: https://github.com/A-Archibasova/hse21_hw1/blob/main/multiqc/multiqc_report_transform.html

<img width="1121" alt="transform" src="https://user-images.githubusercontent.com/71605966/139133637-9f7d495c-7c8a-4640-ba82-b6f1f1ecc67f.png">

## Анализ

<img width="931" alt="Screenshot 2021-10-27 at 23 35 40" src="https://user-images.githubusercontent.com/71605966/139143087-d1b74fb3-6ce9-4a84-a15d-8cab7f3e4089.png">

<img width="862" alt="Screenshot 2021-10-27 at 23 42 58" src="https://user-images.githubusercontent.com/71605966/139143998-1cdfba37-1ed2-440f-b4b6-54a94021885b.png">


```
def statistic(file):
    lenghts = !grep '^>' $file 
    lens_statistic = []
    for i in range(len(lenghts)):
        lens = lenghts[i][(8+len(str(i+1))):]
        j = 0
        while lens[j].isdigit():
            j += 1
        lens_statistic.append(int(lens[:j]))
        
    all_lens = sorted((int(lens) for lens in lens_statistic), reverse=True)
    total = sum(all_lens)

    statistic = 0
    for lens in all_lens:
        statistic += lens
        if statistic >= total/2:
            N50 = lens
            break

    print('Общее количество контигов: ', len(all_lens))
    print('Общая длина: ', total)
    print('Максимальная длина: ', max(all_lens))
    print('N50: ', N50)
    
    
  statistic('~/data/out_contig.txt')
  statistic('~/data/out_scaffold.txt')
    
  def gaps(file):
    num = !grep -Ec 'N+' $file
    total_len = !grep -Eo 'N+' $file | tr -cd 'N' | wc -c
    print('Количество гэпов: ', num[0])
    print('Их общая длина: ', total_len[0])
    
gaps('~/data/longest.txt')
gaps('~/data/longest_gap.txt')
```


