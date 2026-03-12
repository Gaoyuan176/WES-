# 一、数据准备
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
cd 0.ref
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/latest/hg38.fa.gz #以GRCh38为例
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/latest/md5sum.txt #下载md5校验文件
md5sum hg38.fa.gz # 检查数据完整性
gunzip -c ./0.ref/hg38.fa.gz > ./0.ref/hg38.fa # 解压参考基因组,这里使用-c参数，同时保留.gz文件和解压后的.fa文件
```
## 3.下载人类外显子数据
在 UCSC table browser 中，选择要下载的数据。这里的选择包括：
Assembly = Feb. 2009 (GRCh37/hg19)：选择基因组的特定版本，即2009年2月的GRCh37/hg19版本。
Track = Ensembl Genes：选择要检索的基因组注释数据的特定跟踪（Ensembl基因）。
Output Format = BED：选择要将数据输出为BED格式，BED是一种常用的基因组坐标文件格式。
Output = Coding Exons：选择只输出编码外显子数据。

## 4.准备自己需要分析的WES数据
从 SRA 数据库检索到登录号是 SRP070662 的测序数据，选择 'Send results to Run selector' 按钮，进入该 id 的详细数据列表。再选择 Assay Type 是 WXS 的测序数据，一共显示有 30 个样本：6 个case，每个 case 有 5 个样本，分别是 Germline，3 个 Biological replicate：A、B、C，1 个 Technical replicate。

我们选择下面 1 套数据集（双端测序 PAIRED，Average Spot Length: 151）：

case：Germline（id是SRR3182419）、Biological replicate A （id是SRR3182436）
有很多工具或者方法可以下载，如 sratoolkit或 sra-explorer 提到的工具。我们已经将数据下载到目录中，读者无需下载（从下面 4 个链接可以看到每个样本对应 3 个文件，只需要下载*_1.fastq.gz和*_2.fastq.gz）。
https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR318/009/SRR3182419/
``` shell
gunzip -c ./0.rawdata/SRR3182419_2.fastq.gz > ./0.rawdata/SRR3182419_2.fastq #对.fastq.gz文件进行解压
```

# 二、质控


