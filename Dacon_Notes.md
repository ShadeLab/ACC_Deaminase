## Jamell Dacon

File contains notes on ACCd Project: includes synopsis, preliminary data mining information, insight into programs and algorithms being used and scripts.

## Table of Contents

* [Background](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#background)
* [Commands](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#commands)
* [Sequencing Alignment Algorithms](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#seq_al_algorithms)
* [Script](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#scripts)


## Background:
  
  + A plant is subject to a wide range of environmental stresses that can limit the growth and development of the plant. In this research, the study of distribution and diversity of the bacterial gene **_1-aminocyclopropane-1-carboxylate deaminase (ACCd)_** is examined. **_ACCd_** is an enzyme that promotes plant growth and development during stress by decreasing plant ethylene levels, especially following a variety of environmental stresses. Enviornmental stress consist of both bitoic and abiotic stress. Bitoic stress consist fungal and bacterial pathogens, plant viruses, insect predation, nematode infections. Abiotic stress consist of temperature (High and Low), drought, flooding, high salt concentrations (sea blast), organic/ non-organic contaminants and excessive radiation. The plant is influenced by theses stressors to synthesize growth-inhibiting stress ethylene. As a result of these factors stress ethylene, formally known as ethylene synthesis is produced thus inducing decreased root and shoot lengths, increase leaf abscission, decreased dry weight, decreased chlorophyll content and decreased protein synthesis.
  + There is a gray area called Dark Matter, this consist of Diversity, Abundance, Phylogeny and Structure. However, this research will examine this area of Dark Matter in order to produce results, which can be beneficial to the world's population by increase food security. The distribution of **_ACCd_** is examined by retrieving its metadata, which includes _Gene ID, Locus Tag, Genome ID, Environmental data (geographical location, GPS cords), Culture Type, Depth, Ecosystem Category, Ecosystem Subtype, Ecosystem Type, Habitat, Isolation Country, Study Name and Sequencing Method._ The genome sequencing is crucial for a phylogenic tree which depicts a diagrammatic hypothesis of the history of the evolutionary relationship of this this gene. Alpha diversity will be examined as this is the diversity of each site, in other words a local species pool). Beta diversity is examined afterwards since this consists of the ratio between local and regional species diversity for example: _Aquatic microbial communities, Forest soil and Fresh water microbial commmunities_. Consequently, Beta diversity is a measure of species turnover, which amplifies the role of rare species as the difference in species composition between two sites or communities. Another form of diversity that can be examined is Shannon diversity, entailing species richness which refers to the number of species and species eveness which refers to the homogeneity of the species.
  
  
## Commands:

### Gene Search:

* To search for ACC deaminase gene from JGI IMG, go to (https://img.jgi.doe.gov/cgi-bin/m/main.cgi?section=FindGenes&page=findGenes). Then set the Keyword to **1-aminocyclopropane-1-carboxylate deaminase**, set the **Filters** to Gene Product Name (inexact). 
  
* In regards to the **Sequencing Staus** click on All Finished, Permanent Draft and Draft. On the right of **Sequencing Status** you will see **Domain**, there are option such as Archaea, Bacteria, Eukaryota, Metagenome, All (Slow) and Gene Cart.
  
  **_Note_**: _You can search the three Kingdoms of Life (Archaea, Bacteria, Eukaryote), but the **_acdS_** gene is most prevelant in the Metagenome category_.
  
* Be sure that the **List** is highlighted, then click **Show**.
  
* A large list of studies will appear where the gene may or may not exist, select at most 50 at a time and add to **Selected Genomes**.
  
* After these steps are executed, click **Go**.
  
  
### Gene list and Gene Cart

* Now, the Gene ID, Locus Tag, Gene Product Name and Genomes will be displayed. There is an option called **All** to select all pages.
    
* Then click _Export_, this will now export an excel file named **genelist**. Now click on **Add Selected to Gene Cart**, scroll lower until the Table Configuration is visible in which the Gene Field, Scaffold/ Contig Field, Function Field and Project Metadata can all be selected with the option all. Then click **Display Genes Again**, thus the metadate can now be retrieved by clicking **Export**.
    
### Export Genes 

* After the metadata has been exported, there will be an option called **Upload & Export**, in the **Export Genes** click the *FASTA Nucleic Acid* format. Click **Show in Export Format** and the sequences will be exported in a .txt format.
  
  
## Sequencing Alignmment Algorithms 
  * Quantitative Insights Into Microbial Ecology [**QIIME **](http://qiime.org/), pronounced as _chime_. **QIIME** is an open-source bioinformatics pipeline for performing microbiome analysis from raw DNA sequencing data. QIIME is designed to take users from raw sequencing data generated on the Illumina or other platforms through publication quality graphics and statistics. This includes demultiplexing and quality filtering, OTU picking, taxonomic assignment, and phylogenetic reconstruction, and diversity analyses and visualizations. QIIME has been applied to studies based on billions of sequences from tens of thousands of samples. 
  
  * MUltiple Sequence Comparison by Log- Expectation, [**MUSCLE**](http://www.ebi.ac.uk/Tools/msa/muscle/) is claimed to achieve both better average accuracy and better speed than ***ClustalW2*** or ***T-Coffee*** (other multiple sequencing alignment algorithms), depending on the chosen options.
  
### Scripts 

**Location of directory**:

* `cd /mnt/research/ShadeLab/WorkingSpace/Dacon ` 
* Create a folder to upload environmental sequences `mkdir Seqs` and this folder will be very important as it will hold environmental, engineered and aligned sequences.

To run QIIME, first download QIIME, it may a bit challenging to do, hence be patient. Fortunately, QIIME is pre-downloaded on the HPCC so it simply needs to be loaded. This algorithm uses python, thus functions must end in .py to be executed.  
```
module spider QIIME
```
This code presents the latest version of QIIME, along with all previous version.
```
#module load QIIME
module load QIIME/1.8.0

#collapes data and presents species with a 3 percentage difference
pick_otus.py -i ACC_Seqs.txt -o 97_otus
```
This may take a while, so create a job script in a .qsub file.
```
#!/bin/bash -login

### define resources needed:
### walltime - how long you expect the job to run
#PBS -l walltime=24:00:00

### nodes:ppn - how many nodes & cores per node (ppn) that you require
#PBS -l nodes=1:ppn=1

### mem: amount of memory that the job will need
#PBS -l mem=64gb

##Setting the core wanted
#PBS -N ACC_qiime

### you can give your job a name for easier identification
#PBS -N 97_ACC

### load necessary modules, e.g.
module load QIIME/1.8.0

### change to the working directory where your code is located
cd /mnt/research/ShadeLab/WorkingSpace/Dacon

### call your executable
pick_otus.py -s 0.97 -i ACC_Seqs.txt -o 97_otus
```
After receiving this folder, you can now attempt to acquire sequence specificity
```
#retrieves a sequence for each species
pick_rep_set.py -i 97_otus/ACC_Seqs_otus.txt -o rep_set.fna -f ACC_Seqs.txt

#creates a matrix of species by sample (number of times each species is in each sample) 
make_otu_table.py -i ACC_Seqs.txt -t acdS.txt -o otu_table.biom
```
Now, that we have the above information, MUSCLE can be loaded to align sequences. This algorithm create for multiple sequences is also pre-downloaded on the HPCC. Be patient when downloaded these algorithms. 
```
module spider MUSCLE

#module load muscle
module load MUSCLE/3.8.31
```

To attempt multiple sequence alignment (compares sequences to each other) may take a while, so create a job script in a .qsub file.
```
#!/bin/bash -login

### define resources needed:
### walltime - how long you expect the job to run
#PBS -l walltime=24:00:00

### nodes:ppn - how many nodes & cores per node (ppn) that you require
#PBS -l nodes=1:ppn=1

### mem: amount of memory that the job will need
#PBS -l mem=64gb

##Setting the core wanted
#PBS -N ACC_muscle

### you can give your job a name for easier identification
#PBS -N jad_aligned

### load necessary modules, e.g.
module load MUSCLE/3.8.31

### change to the working directory where your code is located
cd /mnt/research/ShadeLab/WorkingSpace/Dacon

### call your executable
muscle -in rep_set.fna -out rep_aligned_seqs
```








