# Vcftools使用说明
此vcftools使用说明为自己部分翻译的版本，原文请参考[vcftools官方使用说明](http://vcftools.sourceforge.net/man_latest.html)
```
vcftools [--vcf file | --gzvcf file | --bcf file] [filtering options] [--record] [--out] [output file]
```
* ```vcftools```：使用vcftools需先输入以调用
* ```[--vcf file | --gzvcf file | --bcf file]```：输入文件。vcftools可输入的文件类型包括'.vcf'、'.vcf.gz'、'.bcf'
* ```[filtering options]```：过滤选项。需要对输入文件进行的过滤选项
* ```[--record]```：表示输出筛选的内容，当有过滤操作时，都需要加上```--record```
	* ```--recode-INFO-all```：表示保留所有INFO fields的内容
* ```[--out]```：输出前缀
* ```[output file]```：输出的文件

示例：从输入的vcf文件中删除所有的InDel位点，输出为新的文件
```
vcftools --vcf input_file.vcf --remove-indels --recode --recode-INFO-all --out SNPs_only
```



## 1.基础命令（Basic Options）
### 1.1 输入文件命令（Input File Options）
* **```--vcf <input_file>```**
此项用于处理.vcf文件
* **```--gzvcf <input_file>```**
此项用于处理.vcf.gz文件
* ```--bcf <input_file>```
此项用于处理.bcf文件
### 1.2 输出文件命令（Output File Options）
* ```--out <output_prefix>```
输出前缀，用于定义vcftools生成的所有输出文件的文件名前缀。如果该参数设置为```--out output```，则所有的输出文件的文件名均为```output.****.```。此选项可以被省略，省略后所有的输出文件的前缀为```out```
* ```stdout```
* ```-c```
将vcftools的输出结果输出为为标准输出，以便可输入到另一个程序或写入所选的文件名。在写为标准输出时，有少数输出功能无法被写入
* **```--temp <temporary_directory>```**
指定输出结果的输出目录



## 2.过滤命令（Filtering Options）
这些命令用于对输入文件的位点进行过滤（保留或者排除）
### 2.1 位置过滤（Position Filtering）
* ```--chr <chromosome>```
* ```--not-chr <chromosome>```
按染色体来过滤位点，可“选择该染色体”或“排除该染色体”。此命令可以多次使用以选择或排除一条以上的染色体
示例：
	```
	vcftools --vcf input_file.vcf --chr chr1 --recode --out C1
	```
* ```--from-bp <integer> --to-bp <integer>```
按碱基对位置来过滤位点，通过指定位点的下限和上限进行位点过滤。此命令可以和```--chr <chromosome>```一起使用
示例：
	```
	vcftools --vcf input_file.vcf --chr chr1 --from-bp 33000000 --to-bp 33781370 --recode --out C1
	```
* ```--positions <filename>```
* ```--exclude-positions <filename>```
按位置信息来过滤位点，通过在输入文件（指的是上述的```<filename>```，而非输入的vcf文件）中的位置信息列表来过滤位点。使用此命令要求在输入文件的每一行都包含有以制表符分隔的染色体和位置信息。此输入文件可以包含以“＃”开头的注释行，它们将被忽略
* ```--positions-overlap <filename>```
* ```--exclude-positions-overlap <filename>```
按位置信息来过滤位点，通过在输入文件（指的是上述的```<filename>```，而非输入的vcf文件）中的位置信息列表来过滤位点，与上一种方法不同的是，此类过滤所依据的信息是文件中参考等位基因与位置列表的重叠（the reference allele overlapping with a list of positions in a file）。使用此命令要求在输入文件的每一行都包含有以制表符分隔的染色体和位置信息。此输入文件可以包含以“＃”开头的注释行，它们将被忽略
* ```--bed <filename>```
* ```--exclude-bed <filename>```
依据输入的bed文件信息来过滤位点。参考的信息为bed文件的前三列（chrom、chromStart、chromEnd），此外bed文件还应包括标题行。如果某个位点的任一等位基因的任一部分在bed文件的条目范围内，其都将被作为Match对象被包括或排除
* ```--thin <integer>```
精简位点，以保证不会有两个位点位于指定的距离之内
### 2.2 位点ID过滤（Site ID Filtering）
* ```--snp <string>```
按SNP位点的ID（如dbSNP的rsID）来过滤位点，通过输入SNP位点的ID以保留该SNP的信息。此命令可以多次使用以包含多个SNP位点
* ```--snps <filename>```
* ```--exclude <filename>```
按输入的txt文件中包含的SNP位点ID来过滤位点。该输入文件（指的是上述的```<filename>```，而非输入的vcf文件）应包含SNP的ID列表（如dbSNP的rsID），要求每行只有一个ID，没有标题行
示例：
	```
	vcftools --vcf input_file.vcf --snps snp.list.txt --recode --recode-INFO-all --out subsetsnp
	```
### 2.3 变异类型过滤（Variant Type Filtering）
* ```--keep-only-indels```
* **```--remove-indels```**
按“SNP”或“InDel”来过滤位点，通过指定需要保留的是SNP还是InDel来进行过滤。```--keep-only-indels```将只保留InDel位点，```--remove-indels```将只保留SNP位点
示例：
	```
	vcftools --vcf input_file.vcf --keep-only-indels --recode --recode-INFO-all --out onlyindel
	vcftools --vcf input_file.vcf --remove-indels --recode --recode-INFO-all --out onlysnp
	```
### 2.4 过滤标志过滤（Filter Flag Filtering）
* ```--remove-filtered-all```
Removes all sites with a FILTER flag other than PASS
* ```--keep-filtered <string>```
* ```--remove-filtered <string>```
Includes or excludes all sites marked with a specific FILTER flag. These options may be used more than once to specify multiple FILTER flags
### 2.5 信息字段过滤（Info Field Filtering）
* ```--keep-INFO <string>```
* ```--remove-INFO <string>```
Includes or excludes all sites with a specific INFO flag. These options only filter on the presence of the flag and not its value. These options can be used multiple times to specify multiple INFO flags
### 2.6 等位基因过滤（Allele Filtering）
* **```--min-alleles <integer>```**
* **```--max-alleles <integer>```**
按照等位基因数量来过滤，仅包含等位基因数量大于等于```--min-alleles <integer>```值并小于等于```--max-alleles <integer>```值的位点。该命令可以和其他命令一起使用
示例：仅筛选出二等位基因位点
	
	```
	vcftools --vcf file1.vcf --min-alleles 2 --max-alleles 2  # 保留二等位基因的位点
	```
* **```--maf <float>```**
* ```--max-maf <float>```
按最小等位基因频率大小来过滤，```--maf <float>```将保留最小等位基因频率大于等于float值的位点，```--max-maf <float>```将保留最小等位基因频率小于等于float值的位点。该命令可以和其他命令一起使用
示例：
	```
	vcftools --vcf input_file.vcf  --maf 0.01 --min-alleles 2 --max-alleles 2 --recode --recode-INFO-all --out maf0.01  # 保留最小等位基因大于等于0.01的位点
	```
* ```--mac <integer>```
* ```--max-mac <integer>```
按最小等位基因计数大小来过滤，```--mac <integer>```将保留最小等位基因计数大于等于integer值的位点，```--max-mac <integer>```将保留最小等位基因计数小于等于integer值的位点。该命令可以和其他命令一起使用
（Allele Count指的是在所有样本中每个位点的各个等位基因的计数情况，大多数情况下，计数为1。Minor Allele Count，最小等位基因计数，指在样品的所有染色体上观察到的该位点的最小等位基因的数量）
* ```--non-ref-af <float>```
* ```--max-non-ref-af <float>```
* ```--non-ref-ac <integer>```
* ```--max-non-ref-ac <integer>```
* ```--non-ref-af-any <float>```
* ```--max-non-ref-af-any <float>```
* ```--non-ref-ac-any <integer>```
* ```--max-non-ref-ac-any <integer>```
Include only sites with all Non-Reference (ALT) Allele Frequencies (af) or Counts (ac) within the range specified, and including the specified value. The default options require all alleles to meet the specified criteria, whereas the options appended with "any" require only one allele to meet the criteria
### 2.7 基因型相关参数过滤（Genotype Value Filtering）
* ```--min-meanDP <float>```
* ```--max-meanDP <float>```
按测序深度来过滤位点，仅包含平均深度值（在所有包含的个体中）大于等于```--min-meanDP <float>```值且小于等于```--max-meanDP <float>```值的位点。使用该命令要求每个位点都有“DP” FORMAT tag。该命令可以和其他命令一起使用
示例：
	```
	vcftools --vcf input_file.vcf --minDP 3 --maxDP 100 --min-meanDP 3 --recode-INFO-all --recode --out filter_DP
	```
* **```--hwe <float>```**
按哈温平衡来过滤位点，仅包含哈温平衡p值大于或等于```--hwe <float>```值的位点
示例：
	```
	vcftools --vcf input_file.vcf --hwe 0.05 --recode-INFO-all --recode --out filter_DP  # 过滤得到哈温平衡p值大于等于0.05的位点
	```
* **```--max-missing <float>```**
按信息的完整度来过滤位点，此处的```<float>```值定义为（0，1），其中0代表允许保留信息完全丢失的位点，1代表不允许任何位点有信息丢失。该命令可理解为：能分型的样本占总样本的比例至少为多少
* **```--max-missing-count <integer>```**
按缺失基因型的数目来过滤位点，此处将排除在所有个体中缺失基因型数目超过```<integer>```值数目的位点
* ```--phased```
Excludes all sites that contain unphased genotypes
### 2.8 杂项过滤（Miscellaneous Filtering）
* ```--minQ <float>```
Includes only sites with Quality value above this threshold
### 2.9 综合命令示例
```
vcftools --vcf input_file.vcf 
	--remove-indels  # 仅保留SNP
	--max-missing 0.8  # 允许位点信息丢失20%
	--maf 0.05  # 保留最小等位基因频率大于0.05的位点
	--min-alleles 2 --max-alleles 2  # 仅保留二等位基因位点
	--hwe 0.05  # 仅保留哈温平衡p值大于0.05的位点
	--recode --recode-INFO-all  # 保留所有INFO fields的内容
	--out snp.hwe0.01
```



## 3.个体过滤命令（Individual Filtering Options）
这些命令用于保留或排除某些个体
* ```--indv <string>```
* ```--remove-indv <string>```
按指定保留或排除的个体来过滤个体。此命令可多次使用以指定或排除多个个体。如果两个命令需要同时使用，则需先使用```--indv <string>```，后使用```--remove-indv <string>```
* ```--keep <filename>```
* ```--remove <filename>```
按指定保留或排除个体的文件来过滤个体。使用此命令要求在输入文件的每一行只有一个个体的ID（使用VCF headerline所定义的个体ID），文件无需标题行。当提供了多个文件时，将保留所有保留文件的个体总和，排除所有排除文件的个体综合。如果两个命令需要同时使用，则需先使用```--keep <filename>```，后使用```--remove <filename>```
* ```--max-indv <integer>```
随机排除若干个个体，仅保留```<integer>```设置的个体数



## 4.基因型过滤命令（Genotype Filtering Options）
这些命令用于排除基因型。如果基因型被排除，则视排除的值为丢失值
* ```--remove-filtered-geno-all```
排除所有包含非“.”（缺失值）或“PASS”的FILTER tag的基因型（Excludes all genotypes with a FILTER flag not equal to "." (a missing value) or PASS）
* ```--remove-filtered-geno <string>```
排除有特殊FILTER tag（```<string>```中的）的基因型
* ```--minGQ <float>```
Exclude all genotypes with a quality below the threshold specified. This option requires that the "GQ" FORMAT tag is specified for all sites
* ```--minDP <float>```
* ```--maxDP <float>```
按测序深度来过滤基因型，仅包含大于等于```--minDP <float>```值且小于等于```--maxDP <float>```值的位点。使用该命令要求每个位点都有“DP” FORMAT tag



## 5.输出命令（Output Options）
这些命令用于对vcftools输出的结果指定执行一些分析或转换。即输出一些经过分析和转换后的结果，而非单纯过滤后的结果
### 5.1 等位基因统计分析结果输出（Output Allele Statistics）
* **```--freq```**
* ```--freq2```
输出每个位点的等位基因频率信息，保存在后缀为“.frq”的文件中。```--freq2```命令用于禁止输出有关等位基因的任何信息。该命令在调用时，位于```[filtering options]```处
示例：
	```
	vcftools --vcf input_file.vcf --freq --out input_filefreq
	vcftools --vcf input_file.vcf --freq --chr chr1 --out input_filefreq  # 可指定染色体
	```
* ```--counts```
* ```--counts2```
输出每个位点的原始等位基因计数，保存在后缀为“.frq.count”的文件中。```--counts2```命令用于禁止输出有关等位基因的任何信息
* ```--derived```
重新排列输出文件列的顺序，使ancestral allele在前面出现。该命令依赖于vcf文件中INFO字段中AA标签所指定的ancestral allele信息。该命令仅可在前面四个命令使用时使用
### 5.2 “深度”统计分析结果输出（Output Depth Statistics）
* ```--depth```
Generates a file containing the mean depth per individual. This file has the suffix ".idepth"
* ```--site-depth```
Generates a file containing the depth per site summed across all individuals. This output file has the suffix ".ldepth"
* ```--site-mean-depth```
Generates a file containing the mean depth per site averaged across all individuals. This output file has the suffix ".ldepth.mean"
* ```--geno-depth```
Generates a (possibly very large) file containing the depth for each genotype in the VCF file. Missing entries are given the value -1. The file has the suffix ".gdepth"
### 5.3 连锁不平衡统计分析结果输出（Output LD Statistics）
* ```--hap-r2```
以phased haplotypes数据分析，输出r2，D和D’值统计信息，保存在后缀为“.hap.ld”的文件中。使用此命令时，vcf输入文件中需包含有phased haplotypes信息
* ```--geno-r2```
以编码为0，1，2（代表每个个体中非参考等位基因的数量）的基因型数据分析，输出r2值统计信息（与Plink进行LD分析的输出度量值相同），保存在后缀为“ .geno.ld”的文件中。使用phased genotypes数据时，还可得到D和D’值统计信息
* ```--geno-chisq```
在数据中，单个位点包含两个以上等位基因时，可使用此命令通过卡方统计量测试基因型的独立性，输出信息保存在后缀为“ .geno.chisq”的文件中
* ```--hap-r2-positions <positions list file>```
* ```--geno-r2-positions <positions list file>```
统计输入文件中包含的位点与其他所有位点的r2值统计信息，输出信息保存在后缀为“.list.hap.ld”或“.list.geno.ld”的文件中
Outputs a file reporting the r2 statistics of the sites contained in the provided file verses all other sites. The output files have the suffix ".list.hap.ld" or ".list.geno.ld", depending on which option is used
* ```--ld-window <integer>```
* ```--ld-window-min <integer>```
此二参数为可选参数，用于定义使用```--hap-r2```、```--geno-r2```、```--geno-chisq```功能进行连锁不平衡计算时使用的SNP数目的窗口：最大数目（```--ld-window <integer>```）和最小数目（```--ld-window-min <integer>```）
* ```--ld-window-bp <integer>```
* ```--ld-window-bp-min <integer>```
此二参数为可选参数，用于定义使用```--hap-r2```、```--geno-r2```、```--geno-chisq```功能进行连锁不平衡计算时，SNP之间物理碱基数的窗口：最大物理碱基数（```--ld-window-bp <integer>```）和最小物理碱基数（```--ld-window-bp-min <integer>```）
* ```--min-r2 <float>```
可选参数，设定r2值的下限，低于该下限的```--hap-r2```、```--geno-r2```、```--geno-chisq```结果将不会输出
* ```--interchrom-hap-r2```
* ```--interchrom-geno-r2```
统计不同染色体上位点的r2值统计信息，输出信息保存在后缀为“ .interchrom.hap.ld”或“.interchrom.geno.ld”的文件中
### 5.4 转换、颠换统计分析结果输出（Output Transition / Transversion Statistics）
* ```--TsTv <integer>```
Calculates the Transition / Transversion ratio in bins of size defined by this option. Only uses bi-allelic SNPs. The resulting output file has the suffix ".TsTv"
* ```--TsTv-summary```
Calculates a simple summary of all Transitions and Transversions. The output file has the suffix ".TsTv.summary"
* ```--TsTv-by-count```
Calculates the Transition / Transversion ratio as a function of alternative allele count. Only uses bi-allelic SNPs. The resulting output file has the suffix ".TsTv.count"
* ```--TsTv-by-qual```
Calculates the Transition / Transversion ratio as a function of SNP quality threshold. Only uses bi-allelic SNPs. The resulting output file has the suffix ".TsTv.qual"
* ```--FILTER-summary```
Generates a summary of the number of SNPs and Ts/Tv ratio for each FILTER category. The output file has the suffix ".FILTER.summary"
### 5.5 核苷酸多样性统计分析结果输出（Output Nucleotide Divergence Statistics）
* ```--site-pi```
基于每个位点分析核苷酸多样性，输出信息保存在后缀为“.sites.pi”的文件中
* ```--window-pi <integer>```
* ```--window-pi-step <integer>```
基于窗口分析核苷酸多样性，以```--window-pi <integer>```提供的数字作为窗口的大小，输出信息保存在后缀为“.windowed.pi”的文件中。```--window-pi-step <integer>```为可选参数，用于指定各个窗口之间的步长
### 5.6 Fst值统计分析结果输出（Output Fst Statistics）
* **```--weir-fst-pop <filename>```**
基于vcf文件中的每个位点，按提供的文件内容，依据Weir和Cockerham的理论计算Fst值。此命令的输入文件中必须包含vcf文件中与一个人群相对应的个体列表（每行仅一个个体），一次需传入两个文件。此命令一次可以计算两个人群之间的Fst值，可通过多次调用该命令来计算两个以上人群的Fst值。默认情况下计算基于vcf中的所有位点。输出信息保存在后缀为“.weir.fst”的文件中
示例：
	```
	vcftools --vcf input_file.vcf --weir-fst-pop population1.txt --weir-fst-pop population2.txt --out
	# 传入包含个体列表的population1.txt和population2.txt，计算两个人群之间位点的Fst值
	```
* ```--fst-window-size <integer>```
* ```--fst-window-step <integer>```
可选参数，与```--weir-fst-pop <filename>```一起使用，以便在设定窗口的基础上进行Fst值的计算。```--fst-window-size <integer>```设定窗口的大小，```--fst-window-step <integer>```设定所需步骤
### 5.7 其他统计分析结果输出（Output Other Statistics）
* ```--het```
基于个体计算个体的近交系数F（杂合度计算）
Calculates a measure of heterozygosity on a per-individual basis. Specfically, the inbreeding coefficient, F, is estimated for each individual using a method of moments. The resulting file has the suffix ".het".
* **```--hardy```**
输出通过哈温平衡测试的每个位点的p值，此外还包含“纯合子”和“杂合子”的“观察数”以及HWE下的相应“预期数”。输出信息保存在后缀为”.hwe”的文件中
* ```--TajimaD <integer>```
Outputs Tajima’s D statistic in bins with size of the specified number. The output file has the suffix ".Tajima.D"
* ```--indv-freq-burden```
This option calculates the number of variants within each individual of a specific frequency. The resulting file has the suffix ".ifreqburden"
* ```--LROH```
This option will identify and output Long Runs of Homozygosity. The output file has the suffix ".LROH". This function is experimental, and will use a lot of memory if applied to large datasets
* ```--relatedness```
This option is used to calculate and output a relatedness statistic based on the method of Yang et al, Nature Genetics 2010 (doi:10.1038/ng.608). Specifically, calculate the unadjusted Ajk statistic. Expectation of Ajk is zero for individuals within a populations, and one for an individual with themselves. The output file has the suffix ".relatedness"
* ```--relatedness2```
This option is used to calculate and output a relatedness statistic based on the method of Manichaikul et al., BIOINFORMATICS 2010 (doi:10.1093/bioinformatics/btq559). The output file has the suffix ".relatedness2"
* ```--site-quality```
输出包含每个位点的SNP质量值，其在vcf文件的QUAL列中显示。输出信息保存在后缀为“.lqual”的文件中
* **```--missing-indv```**
输出每个个体的缺失情况，输出信息保存在后缀为".imiss"的文件中
* **```--missing-site```**
输出每个位点的缺失情况，输出信息保存在后缀为".lmiss"的文件中
* ```--SNPdensity <integer>```
Calculates the number and density of SNPs in bins of size defined by this option. The resulting output file has the suffix ".snpden"
* **```--kept-sites```**
输出经过滤后所保留的所有位点的信息，输出信息保存在后缀为".kept.sites"的文件中
* ```--removed-sites```
输出经过过滤后排除的所有位点的信息，输出信息保存在后缀为 ".removed.sites"的文件中
* ```--singletons```
This option will generate a file detailing the location of singletons, and the individual they occur in. The file reports both true singletons, and private doubletons (i.e. SNPs where the minor allele only occurs in a single individual and that individual is homozygotic for that allele). The output file has the suffix ".singletons"
* ```--hist-indel-len```
生成所有InDel（包含SNP）长度的直方图文件
This option will generate a histogram file of the length of all indels (including SNPs). It shows both the count and the percentage of all indels for indel lengths that occur at least once in the input file. SNPs are considered indels with length zero. The output file has the suffix ".indel.hist"
* ```--hapcount <BED file>```
This option will output the number of unique haplotypes within user specified bins, as defined by the BED file. The output file has the suffix ".hapcount"
* ```--mendel <PED file>```
This option is use to report mendel errors identified in trios. The command requires a PLINK-style PED file, with the first four columns specifying a family ID, the child ID, the father ID, and the mother ID. The output of this command has the suffix ".mendel"
* ```--extract-FORMAT-info <string>```
从vcf文件中提取与指定的FORMAT标识符有关的基因型字段信息，输出信息保存在后缀为".<FORMAT_ID>.FORMAT"的文件中
示例：
	```
	vcftools --vcf input_file1.vcf --extract-FORMAT-info GT
	# 提取所有包含有“GT”（基因型）的条目
	```
* ```--get-INFO <string>```
从vcf的INFO字段中提取信息。```<string>```用于指定要提前的INFO tag，此命令可以多次使用以提取多个INFO条目。输出信息保存在后缀为".INFO"的文件中，该文件中的INFO信息由制表符分隔
示例：
	```
	vcftools --vcf input_file1.vcf --get-INFO NS --get-INFO DB
	# 提取NS和DB flags
	```
### 5.8 vcf格式输出（Output Vcf Format）
* **```--recode```**
* ```--recode-bcf```
在对输入的'.vcf'、'.vcf.gz'、'.bcf'文件进行过滤后（前述的各个命令），输出一个结果文件（```--recode```输出vcf文件，```--recode-bcf```输出bcf文件），该结果文件的后缀为“.recode.vcf”或“.recode.bcf”。单纯使用该命令时，结果文件中的INFO字段将被删除
* ```--recode-INFO <string>```
* **```--recode-INFO-all```**
此二命令与上述命令一同使用，用以保存INFO字段。```--recode-INFO <string>```可依据输入的字符串保留相应的INFO字段，该命令可多次调用以保留若干INFO字段，```--recode-INFO-all```可保留原始文件中的所有INFO字段
* ```--contigs <string>```
原始文件中不含有任何“contig declarations”时，与```--recode-bcf```命令一同使用
This option can be used in conjuction with the --recode-bcf when the input file does not have any contig declarations. This option expects a file name with one contig header per line. These lines are included in the output file
### 5.9 其他格式输出（Output Other Format）
* ```--012```
将结果输出为大的矩阵，输出结果包含三个文件：
	* ".012"：主文件，每个个体的位点基因型信息以0，1，2的形式按行保存。0，1，2表示在该个体基因型中非参考等位基因的数量，缺失的基因型用-1表示
	* ".012.indv"：对主文件包含的个体进行详细说明
	* ".012.pos"：对主文件包含的位点信息进行详细说明
* ```--IMPUTE```
This option outputs phased haplotypes in IMPUTE reference-panel format. As IMPUTE requires phased data, using this option also implies --phased. Unphased individuals and genotypes are therefore excluded. Only bi-allelic sites are included in the output. Using this option generates three files. The IMPUTE haplotype file has the suffix ".impute.hap", and the IMPUTE legend file has the suffix ".impute.hap.legend". The third file, with suffix ".impute.hap.indv", details the individuals included in the haplotype file, although this file is not needed by IMPUTE
* ```--ldhat```
* ```--ldhat-geno```
These options output data in LDhat format. This option requires the "--chr" filter option to also be used. The first option outputs phased data only, and therefore also implies "--phased" be used, leading to unphased individuals and genotypes being excluded. The second option treats all of the data as unphased, and therefore outputs LDhat files in genotype/unphased format. Two output files are generated with the suffixes ".ldhat.sites" and ".ldhat.locs", which correspond to the LDhat "sites" and "locs" input files respectively
* ```--BEAGLE-GL```
* ```--BEAGLE-PL```
These options output genotype likelihood information for input into the BEAGLE program. The VCF file is required to contain FORMAT fields with "GL" or "PL" tags, which can generally be output by SNP callers such as the GATK. Use of this option requires a chromosome to be specified via the "--chr" option. The resulting output file has the suffix ".BEAGLE.GL" or ".BEAGLE.PL" and contains genotype likelihoods for biallelic sites. This file is suitable for input into BEAGLE via the "like=" argument
* **```--plink```**
将结果按Plink的格式输出，生成 ".ped"和".map"两个文件。只有二等位基因才可被输出。对于大型数据集可能会非常慢， 建议使用--chr选项来划分数据集
* ```--plink-tped```
将结果按Plink转置格式输出后缀为“ .tped”和“ .tfam”的文件，相较```--plink```，输出结果较快
* ```--chrom-map```
For usage with variant sites in species other than humans, the --chrom-map option may be used to specify a file name that has a tab-delimited mapping of chromosome name to a desired integer value with one line per chromosome. This file must contain a mapping for every chromosome value found in the file



## 6.比较命令（Comparison Options）
这些命令用于比较两个variant file并输出比较结果。所有的比较命令都要求两个文件都包含相同的染色体，并且文件必须以相同的顺序排序。 如果其中一个文件包含另一个文件不包含的染色体，请使用--not-chr过滤器将其从分析中删除
## 6.1 DIFF vcf文件（DIFF Vcf File）
* ```--diff <filename>```
此项用于处理.vcf文件
* ```--gzdiff <filename>```
此项用于处理.vcf.gz文件
* ```--diff-bcf <filename>```
此项用于处理.bcf文件
## 6.2 DIFF命令（DIFF Options）
* ```--diff-site```
输出两个文件共有/独有的位点，输出信息保存在后缀为".diff.sites_in_files"的文件中
* ```--diff-indv```
输出两个文件共有/独有的个体，输出信息保存在后缀为".diff.indv_in_files"的文件中
* ```--diff-site-discordance```
This option calculates discordance on a site by site basis. The resulting output file has the suffix ".diff.sites"
* ```--diff-indv-discordance```
This option calculates discordance on a per-individual basis. The resulting output file has the suffix ".diff.indv"
* ```--diff-indv-map <filename>```
This option allows the user to specify a mapping of individual IDs in the second file to those in the first file. The program expects the file to contain a tab-delimited line containing an individual’s name in file one followed by that same individual’s name in file two with one mapping per line
* ```--diff-discordance-matrix```
This option calculates a discordance matrix. This option only works with bi-allelic loci with matching alleles that are present in both files. The resulting output file has the suffix ".diff.discordance.matrix"
* ```--diff-switch-error```
This option calculates phasing errors (specifically "switch errors"). This option creates an output file describing switch errors found between sites, with suffix ".diff.switch"



## 7.Authors
Adam Auton (adam.auton@einstein.yu.edu)
Anthony Marcketta (anthony.marcketta@einstein.yu.edu)