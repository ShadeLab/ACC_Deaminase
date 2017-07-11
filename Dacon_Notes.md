# Jamell Dacon

File contains notes on ACCd Project: includes synopsis, preliminary data mining information, insight into programs and algorithms being used and scripts which are used in a linux based machine.

## Table of Contents

* [Background](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#background)
* [Commands](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#commands)
* [Sequencing Alignment Algorithms](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#sequencing-alignment-algorithms)
* [Scripts](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#scripts)
  * [Job Scripts](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#job-scripts)
  * [Phylogeny](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#phylogeny)
  * [Beta Diversity](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#beta-diversity)
* [Metadata](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#metadata)
* [R Studio Scripts](https://github.com/ShadeLab/ACC_Deaminase/blob/master/Dacon_Notes.md#r-studio-scripts)



## Background:
  
  + A plant is subject to a wide range of environmental stresses that can limit the growth and development of the plant. In this research, the study of distribution and diversity of the bacterial gene **_1-aminocyclopropane-1-carboxylate deaminase (ACCd)_** is examined. **_ACCd_** is an enzyme that promotes plant growth and development during stress by decreasing plant ethylene levels, especially following a variety of environmental stresses. Environmental stress consists of both biotic and abiotic stress. Biotic stress consists fungal and bacterial pathogens, plant viruses, insect predation, and nematode infections. Abiotic stress consists of temperature (high and low), drought, flooding, high salt concentrations (sea blast), organic/ non-organic contaminants and excessive radiation. The plant is influenced by these stressors to synthesize growth-inhibiting stress ethylene. As a result of these factors, stress ethylene, formally known as ethylene synthesis is produced thus inducing decreased root and shoot lengths, increased leaf abscission, decreased dry weight, decreased chlorophyll content and decreased protein synthesis.
  + There is a gray area called Dark Matter, this area consists of Diversity, Abundance, Phylogeny and Structure. However, this research will examine this area of Dark Matter in order to produce results which can be beneficial to the world's population by implementing and increasing this gene in the environment to encourage the increase food security. The distribution of **_ACCd_** is examined by retrieving its metadata, which includes _Gene ID, Locus Tag, Genome ID, Environmental data, Geographical location (Longitude and Latitude coordinates), Culture Type, Depth, Ecosystem Category, Ecosystem Subtype, Ecosystem Type, Habitat, Isolation Country, Study Name and Sequencing Method._ The genome sequencing is crucial for a phylogenic tree which depicts a diagrammatic hypothesis of the history of the evolutionary relationship of this this gene present in the environment and can be compared to engineered genes. Alpha diversity is examined as this is the diversity of each site, in other words a local species pool. Beta diversity is also examined afterwards since this consists of the ratio between local and regional species diversity for example: _Marine, Fresh water, Soil and Waste water microbial communities_. Consequently, Beta diversity is a measure of species turnover, which amplifies the role of rare species as the difference in species composition between two sites or communities. Another form of diversity that can be examined is Shannon diversity, entailing species richness which refers to the number of species and species eveness which refers to the homogeneity of the species.
  
  
## Commands:

### Gene Search:

* To search for ACC deaminase gene from [JGI IMG](http://jgi.doe.gov/), go to [Find Genes](https://img.jgi.doe.gov/cgi-bin/m/main.cgi?section=FindGenes&page=findGenes). Then set the Keyword to **1-aminocyclopropane-1-carboxylate deaminase**, set the **Filters** to Gene Product Name (inexact). 
  
* In regards to the **Sequencing Staus** click on All Finished, Permanent Draft and Draft. On the right of **Sequencing Status** you will see **Domain**, there are options such as Archaea, Bacteria, Eukaryota, Metagenome, All (Slow) and Gene Cart.
  
  **_Note_**: _You can search the three Kingdoms of Life (Archaea, Bacteria, Eukaryote), but the **_acdS_** gene is most prevelant in the Metagenome category_.
  
* Be sure that the **List** is highlighted, then click **Show**.
  
* A large list of studies will appear where the gene may or may not exist, select at most 50 at a time and add to **Selected Genomes**.
  
* After these steps are executed, click **Go**.
  
  
### Gene list and Gene Cart

* Now, the Gene ID, Locus Tag, Gene Product Name and Genomes will be displayed. There is an option called **All** to select all pages.
    
* Then click _Export_, this will now export an excel file named **genelist**. Now click on **Add Selected to Gene Cart**, scroll lower until the Table Configuration is visible in which the Gene Field, Scaffold/ Contig Field, Function Field and Project Metadata can all be selected with the option all. Then click **Display Genes Again**, thus the metadata can now be retrieved by clicking **Export**.
    
### Export Genes 

* After the metadata has been exported, there will be an option called **Upload & Export**, in the **Export Genes** click the *FASTA Nucleic Acid* format. Click **Show in Export Format** and the sequences will be displayed where it can be copied and pasted into in a .txt format.
  
  
## Sequencing Alignnment Algorithms 
  * Quantitative Insights Into Microbial Ecology [**QIIME**](http://qiime.org/), pronounced as _chime_. **QIIME** is an open-source bioinformatics pipeline for performing microbiome analysis from raw DNA sequencing data. This algorithm is designed to take raw sequencing data and generate it on the Illumina or other platforms through publication quality graphics and statistics. This includes demultiplexing and quality filtering, OTU picking, taxonomic assignment, and phylogenetic reconstruction, and diversity analyses and visualizations. 
  
  * MUltiple Sequence Comparison by Log- Expectation, [**MUSCLE**](http://www.ebi.ac.uk/Tools/msa/muscle/) is claimed to achieve both better average accuracy and better speed than ***ClustalW2*** or ***T-Coffee*** (other multiple sequencing alignment algorithms), depending on the chosen options.
 
 
## Scripts 

**Location of directory**:

* `cd /mnt/research/ShadeLab/WorkingSpace/Dacon` 

* Create a folder to upload environmental sequences `mkdir Seqs` and this folder will be very important as it will hold environmental, engineered and aligned sequences. In this Sequence directory, the environmental (uncultured) genes will be saved as `ACC_Seqs.txt` and the engineered (cultured) genes are saved as `acdS_Seqs.txt`.

To run QIIME, first download QIIME, it may a bit challenging to do, hence be patient. Fortunately, QIIME is pre-downloaded on the HPCC so it simply needs to be loaded. This algorithm uses python, thus functions must end in .py to be executed.

```
module spider QIIME
```
This code presents the latest version of QIIME, along with all previous version.
```
#module load QIIME
module load QIIME/1.8.0
```

### Job Scripts

Usually to identify a gene we used 97 percent sequence identity to see how similar they are, there is only a 3 percent difference. We need to make a plot to see how similar the 16s sequences are on the x-axis and how similar the ACC sequences are on the y axis and we would look the the distribution to look at the best cut off to cluster them into otus. We used 85 percent sequence identity to distinguish the diversity. To collapse the data and present species similarity with a 15 percentage difference ensure diversity. This may take a while, so create a job script in a .qsub file.
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
#PBS -N 85_ACC

### load necessary modules, e.g.
module load QIIME/1.8.0

### change to the working directory where your code is located
cd /mnt/research/ShadeLab/WorkingSpace/Dacon

### call your executable
pick_otus.py -s 0.85 -i ACC_Seqs.txt -o 85_otus
```
After receiving this folder, you can now attempt to acquire sequence specificity
```
#retrieves a sequence for each species
pick_rep_set.py -i 85_otus/ACC_Seqs_otus.txt -o rep_set.fna -f ACC_Seqs.txt

#creates a matrix of species by sample (number of times each species is in each sample) 
make_otu_table.py -i ACC_Seqs.txt -t acdS_Seqs.txt -o otu_table.biom
```
Now, that we have the above information, MUSCLE can be loaded to align sequences. This algorithm create for multiple sequences is also pre-downloaded on the HPCC. Be patient when downloaded these algorithms. 
```
module spider MUSCLE

#module load muscle
module load MUSCLE/3.8.31
```
To attempt multiple sequence alignment (compares sequences to each other)of both environmental and engineered genes, both `ACC_Seqs.txt` and `acdS_Seqs.txt` need to be catenated, forming a new .txt file called `ATCG_Seqs.txt`. This multiple sequence alignment may take a while, so create a job script in a .qsub file.
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
muscle -in ATCG_seqs.txt -out aligned_seqs -maxiters 2
```

### Phylogeny

A phylogeny or phylogenetic tree is an evolutionary relationships among various biological species and/ or other entities
based upon there similarities/ differences in their genetic characteristics. However, with the number of sequences, this make take some time so create a job script in a .qsub file.
* Firstly, one needs to align the sequences.
* Secondly, one needs filter the sequences.
* Lastly, with this retrieved data of the filtered sequences the phylogeny can now be made.
 ```
 #!/bin/bash -login

### define resources needed:
### walltime - how long you expect the job to run
#PBS -l walltime=24:00:00

### nodes:ppn - how many nodes & cores per node (ppn) that you require
#PBS -l nodes=1:ppn=1

### mem: amount of memory that the job will need
#PBS -l mem=24gb

##Setting the core wanted
#PBS -l feature=intel16

### you can give your job a name for easier identification
#PBS -N acc_cult_align

### load necessary modules, e.g.
module load MUSCLE/3.8.31

### change to the working directory where your code is located
cd /mnt/research/ShadeLab/WorkingSpace/Dacon/ACC

### call your executable
muscle -out aligned_rep_set_pfiltered.fasta -in cult_and_rep_set.fna -maxiters 1
```
Use the scripts as follows to create a phylogeny. This may take a while so create job scripts in a .qsub file.
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
#PBS -N jad_filt_ACC

### load necessary modules, e.g.
module load QIIME/1.8.0

### change to the working directory where your code is located
cd /mnt/research/ShadeLab/WorkingSpace/Dacon/ACC

### call your executable
make_phylogeny.py -i aligned_rep_set_pfiltered.fasta -o full_tree.tre
```
After retrieving the `full_tree.tre` files, copy this file to your desktop and go to [**ITOL**](http://itol.embl.de/upload.cgi). To find this file, click where its says **Browse**, located the file, select it and click **Upload** 
 
 
## Metadata

Metadata collected from JGI, was collated to used to for Abundance files where the ACC was matched with rplB (a single copy gene) data, Map files which contains GPS coordinates such as longitudinal and latitudinal points to identify the gene's location on a world map, and an Alpha diversity which identifies the the diversity of each site along with Shannon diversity which presents the number of species present and species evenness. Those files are as follows:
1. `acc_abun.txt`
2. `acc_map.txt`
3. `acc_alpha.txt`
4. `acc_alphaphied_map2.txt`


## R Studio Scripts 

R Studio is an IDE for R which includes a console, syntax-highlighting editor that supports direct code execution, as well as tools for plotting, history, debugging and workspace management.

Normalized ACC abundance (to rplB) script
```
#Remember to install packages in order to run the respective programs
install.packages("ggplot2")
install.packages("readr")
install.packages("plyr")
install.packages("rworldmap")

##### Normalized ACC abundance (to rplB)
library(readr)
setwd("/Users/icer4/Downloads/")
acc_abun <- read_delim("acc_abun.txt", "\t", escape_double = FALSE, trim_ws = TRUE)

library(ggplot2)
ggplot(acc_abun, aes(Collapsed_Ord, Normalized_ACC, fill=Collapsed_Ord))+
  geom_boxplot()+
  ylab("Normalized ACC Abundance")+
  xlab("")+
  theme_bw()+
  coord_cartesian(ylim=c(0,1))+
  coord_flip()
```
ACC Summary script
```
##### ACC summary  
library(plyr)
acc_sum<-ddply(acc_abun, c("Class"), summarise, N= length(Normalized_ACC), mean=mean(Normalized_ACC), sd=sd(Normalized_ACC), se=sd/sqrt(N))
str(acc_sum)
dodge <- position_dodge(width=.8)
ggplot(acc_sum, aes(Class, mean, fill=Class, colour=Class, cex=1.5))+
  geom_point()+
  theme_bw()+
  geom_errorbar(aes(ymax=mean+se, ymin=mean-se, width=0.2, cex=0.2), position=dodge)+ 
  coord_flip()
```
ACC Map Studies scripts 
```
###### Map studies

#read data
acc_map <- read_delim("acc_map.txt", "\t", escape_double = FALSE, trim_ws = TRUE)
str(acc_map)

#make columns of lat/long numeric
acc_map$lat<-as.numeric(acc_map$lat)
acc_map$long<-as.numeric(acc_map$long)


library(rworldmap)
library(ggplot2)


# Get world map from rworldmap
worldmap <- map_data("world")
str(worldmap)

# Plot entire world map with studies with ACC
  ggplot(data = worldmap, aes(x = long, y = lat, group = group)) +
  geom_polygon(colour = "grey", fill = "grey") +
  theme_bw() +
  geom_point(data = acc_map, aes(x = long, y = lat, fill=Collapsed_Eco, colour= Collapsed_Eco, group=Collapsed_Eco), size = 3)
```  
Alpha and Shannon Diversity
```
######Alpha diversity

library(readr)
acc_alphaphied_map2 <- read_delim("acc_alphaphied_map2.txt", "\t", escape_double = FALSE, trim_ws = TRUE)
library(ggplot2)
ggplot(acc_alphaphied_map2, aes(Collapsed_Eco, otus_obs, fill=Collapsed_Eco))+
  geom_boxplot()+
  ylab("Shannon")+
  xlab("")+
  coord_flip()+
  theme_bw()+
  scale_y_log10()

#lat/long correlation
plot(acc_alphaphied_map2$Latitude, acc_alphaphied_map2$shannon)
plot(acc_alphaphied_map2$Longitude, acc_alphaphied_map2$shannon)
plot(acc_alphaphied_map2$Latitude, acc_alphaphied_map2$otus_obs)
plot(acc_alphaphied_map2$Longitude, acc_alphaphied_map2$otus_obs)

ggplot(acc_alphaphied_map2, aes(Collapsed_Eco, shannon, fill=Collapsed_Eco))+
  geom_boxplot()+
  coord_flip()
```
Next we want to use [**BLASTn**](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome) to assign our OTUs taxonomy using the nucleotide database
```
#first, get the most up-to-date nt database
update_blastdb.plant

#unzip the files
tar -xzvf nt*

blastn -query rep_set.fna -max_target_seqs 1 -outfmt "6 qseqid sacc stitle pident evalue" -out acc_results_out -negative_gilist sequence.gi -db nt
```
 _Note: sequence.gi is a file of envrionmental sequences that was retrieved from NCBI Entrees._

This produces only a list of ascession numbers and the name that corresponds to the ascession number. We want to map this data back to a taxonomic assignment. 

## Beta Diversity

```
filter_samples_from_otu_table.py -i acc_otu_table.biom -o soil_plant_otu_table.biom -m acc_alphaphied_map2.txt -s 'Collapsed_Eco:Soil,Rhizosphere,Phylloplane,Plant,Rhizoplane'

beta_diversity.py -i soil_plant_otu_table.biom -o soil_plant_beta -m bray_curtis

principal_coordinates.py -o soil_plant_beta/coords.txt -i soil_plant_beta/bray_curtis_soil_plant_otu_table.txt

make_2d_plots.py -i soil_plant_beta/coords.txt -o soil_plant_beta/2d_plot -m acc_alphaphied_map2.txt

group_significance.py -i acc_otu_table.biom -o cat_significance.txt -m acc_alphaphied_map2.txt -c 'Collapsed_Eco' --biom_samples_are_superset

compare_categories.py -m acc_alphaphied_map2.txt --method=adonis -i soil_plant_beta/bray_curtis_soil_plant_otu_table.txt -c 'Collapsed_Eco' -o adonis_soil-plant
```
