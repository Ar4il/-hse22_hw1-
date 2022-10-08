# -hse22_hw1-
minor hw1
Сборка генома бактерии, выделенной из воды с нефтью, на основании парно-концевых (paired-end, PE) и чтений типа mate-pairs (MP).
Чепель Артем группа 2
Работа с Putty
1 Создание символических ссылок
$ ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {} 
2 Выбор случайных чтений с помощью SeqTK (Random seed = 928)
$ seqtk sample -s 928 oil_R2.fastq 5000000 > sub2.fastq
$ seqtk sample -s 928 oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
$ seqtk sample -s 928 oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
3 Оценка качества исходных чтений с помощью FastQC и создание отчёта с помощью MultiQC
$ mkdir fastqc
$ ls sub* matep* | xargs -tI{} fastqc -o fastqc {}
$ mkdir multiqc
$ multiqc -o multiqc fastqc
4 Урезание размера исходных чтений через Platanus, оценка и создание отчёта MultiQC:
$ platanus_trim sub*
$ platanus_internal_trim matep*
$ mkdir fastqc_trimmed
$ ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
$ mkdir multiqc_trimmed
$ multiqc -o multiqc_trimmed fastqc_trimmed
5 Сбор контиг и скаффолдов
$ platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed
$ platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed
$ platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed
Отчет для неурезанных чтений
![image](https://user-images.githubusercontent.com/84396301/194705983-952d7024-49cd-4149-bc51-0af944cb8378.png)
![image](https://user-images.githubusercontent.com/84396301/194705992-0c36de0c-4d6c-4ce1-acad-9b0627affe18.png)
![image](https://user-images.githubusercontent.com/84396301/194706000-722c8c1a-3c48-4f78-b3b9-e7296ad14092.png)
![image](https://user-images.githubusercontent.com/84396301/194706025-21a943c8-6dd5-42a3-9dc8-f721d17fa230.png)
