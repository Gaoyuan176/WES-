# 一、下载数据
## 1.为每个步骤建立空文件夹
``` shell
mkdir 0.rawdata 0.ref 1.QC 2.clean_QC 3.bwa_index 4.align 5.stat 6.gatk 7.Germline_Mutations 8.Mutect2 tools
mkdir 1.QC/fast_QC  1.QC/multi_QC
mkdir 2.clean_QC/fast_QC  2.clean_QC/multi_QC  2.clean_QC/trim_galore_QC
mkdir 6.gatk/table
mkdir 7.Germline_Mutations/gvcf/
mkdir 8.Mutect2/annotation 8.Mutect2/maf
```
## 2.下载人类全基因组测序数据（版本GRCh37或GRCh38，根据自己测序数据版本选择，这里以GRCh37为例）
``` shell
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/latest/hg38.fa.gz #以GRCh38为例
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/latest/md5sum.txt #下载md5校验文件
md5sum hg38.fa.gz # 检查数据完整性
gunzip -c hg38.fa.gz # 解压参考基因组,这里使用-c参数，同时保留.gz文件和解压后的.fa文件

