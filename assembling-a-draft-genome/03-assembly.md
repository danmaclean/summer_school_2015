---
layout: page
title: Assembling a draft genome in the Galaxy Interface
subtitle: Assembly
minutes: 25
---

> ## Learning Objectives {.objectives}
> * Using Velvet's two step assembly process
> * Evaluating assembly quality
> * Changing parameters and estimating effect 
> 

Now that our reads are QC'd we can start to do an assembly. The reads we have prepared are selected from the Phytophthora infestans T30-4 genome. They cover a very small 170kb region on Super Contig 43 from 740-910Kb. This region contains some nice 'core orthologues' and the odd effector. The reads are sufficient to cover the region to 30x, which is an average amount of coverage for a first assembly with short reads.

## Assembly
Sequence assembly de novo seeks simply to form longer sequences from short ones, accurately and efficiently without the assistance of any reference sequence. The long sequences that are produced, contigs, should span an entire chromosome but this is not often achievable because most eukaryotic and prokaryotic genomes contain regions that cannot be sequenced or assembled, leaving most genomes assembled into a few (or many) large fragments. De-novo assembly is not a problem that has emerged with the next-generation sequencing, there are numerous well-established algorithms and tools for assembly of capillary sequence reads. These methods do not work with next-generation reads for several reasons: the distributions of sequencing errors are different making the prior error models inappropriate, they rely on overlaps between sequence reads that do not exist in short sequence reads and are unable to deal with the number of sequence reads.

Next Generation Sequence reads are typically assembled using de Bruijn graph assemblers, such as Velvet (Zerbino et al., 2008), ABYSS (Simpson et al., 2009), Euler (Chaisson et al., 2008) , SOAPdenovo (Li et al., 2010) and ALLPATHS (Butler et al., 2008). In the majority of these assemblers the de Bruijn graph is an internal data structure that represents a network in which the linked entities are (usually) the k-mers and links are made between k-mers that overlap by k-1 with a single nucleotide overhang. In perfect data, (i.e. a situation with no read errors and with k long enough to include the longest repeat in a single k-mer) then it is theoretically possible to reconstruct the genome by following from k-mer to k-mer along the edges, passing through each k-mer once. Since next-generation sequencing data do contain errors and genomic repeats can be very long (many times longer than read length, let alone the length of k used) then the read errors can cause dead ends in the path that terminate extension of the contig, repeats in the genome can cause cycles in the graph. The different ways in which the assembly programs get around these problems is what separates them. 

## Velvet
Velvet is one of the more popular and reliable de novo assemblers for short-read sequence. Velvet works by first breaking reads down into the constituent k-mers and forming the de Bruijn graph. The first part of using Velvet is to run `velveth` on the reads, this step makes the data structure.


> ## Challenge {.challenge}
> + In the tool list under 'NGS:Assembly', select `velveth`
> + Set the parameters as follows:
>   + Hash Length = 27
>	+ Click 'Add new input files' to load a read file, you'll need two for each assembly, as these are paired
>	+ 'fastq' format
>	+ Read type = 'shortPaired reads' for the first file and 'shortPaired2 reads' for the second file
>	+ Select the trimmed reads from the 500 bp insert library. Left reads go in first, right reads go in second.	
> + Click Execute, `velveth` will build the data structure. You can examine the output when it is done, but remember nothing is assembled yet.
>

The second step is to let the assembly algorithm `velvetg` work through the produced hash and create contigs.

> ## Challenge {.challenge}
> + In the tool list under 'NGS:Assembly', select `velvetg`
> + Set the parameters as follows:
>	+ Velvet Dataset = the velveth data you just created
>	+ Generate AMOS = No
>	+ Generate Unused Reads = Yes
>	+ Generate LastGraph = No
>	+ Coverage Cutoff = Automatic
>	+ Expected Coverage of Unique Region = Automatic
>	+ Minimum Length = No
>	+ Paired Reads = Yes
>	+ Expected Distance = 350
>	+ Velvet advanced = Defaults
> + Hit execute, the assembly will be placed in an item called 'Contigs'. You will also get 'Unused Reads' and 'Stats'. Examine all these files.

## Evaluating the assembly
Once we have contigs we can start to examine them. One obvious metric is the number and length of the contigs. the longer the better. The assembly statistics tool gives us a summary of distribution of contig size. A statistic called N50, which is the contig length such that using equal or longer contigs produces half the bases of the genome, is a commonly used one as it encompasses both length and contig number. It does rely on knowing an approximate value for the genome you are trying to assemble.

> ## Challenge {.challenge}
> + Select the 'assemblystats' tool in 'NGS: Assembly' section. 
> + Choose 'contig' for 'Type of Read' and your Velvet contigs for the 'Source file in FASTA format'.
> + Click execute, when its done you should get a file of statistics and a histogram. Examine these and see what you think about the assembly you have produced. Is it any good? 
>

##Altering the k-mer size and merge assemblies
An additional complication is that the performance of each of the assemblers is sensitive to input parameter values, it is not usually possible to determine a priori the best parameter values (indeed, the ‘best’ parameter values may depend entirely on the final downstream application). To get round this we carry out assemblies at a wide-range of parameter values (usually we check a wide range of k values) and compare the resulting contig sets with a view to processing the set of sequences downstream. This can take the form of clustering and removing identical redundant sequences to make a maximal set or using a set of control sequences such as commonly occurring orthologue sets, to aid assessment of the reconstruction of these core genes as an indicator of assembly success.

###Velvet Optimiser
Velvet Optimiser is a useful tool that carries out many runs of Velvet at different values of k, assesses them using a particular user-specified measure and keeps only the best one according to that measure. Run it on your trimmed read data.

> ## Challenge {.challenge}
> + Select the 'Velvet Optimiser' tool in 'NGS: Assembly' section. 
> + Set the parameters as follows:
>	+ Start Hash Length = 41
>	+ End Hash Length = 51
>	+ Number of threads = 1
>	+ Kmer optimisations metric = N50
>	+ Coverage optimisation metric = Total number of bp in contigs > 1kb
>	+ Leave 'Advanced options' empty
>	+ Click 'Add new input files' to load a read file, you'll need two for each assembly, as these are paired
>	+ 'fastq' format
>	+ Read type = 'shortPaired reads' for the first file and 'shortPaired3 reads' for the second file
>	+ Select the trimmed reads from the 500 bp insert library. Left reads go in first, right reads go in second.
> + CLick Execute. When the job is done, you should have the 'best' assembly and the output stats.
> + Examine the Stats Output file
> + Run the `assembly stats` tool on the Contigs from this assembly, as before.
> + Compare the results of this best assembly, with the previous assembly. Is it any better?
> + Now repeat the `Velvet Optimiser` run with the trimmed reads from the 2000 bp insert library. Examine that and compare it to the assembly from the 500 bp library.

## Using Minimus to scaffold the assembly
Once we have our contigs from our two separate insert size libraries, we can merge them to remove redundancy and find points of overlap and combine overlapping contigs into a super-assembly. The Minimus 2 scaffolder, part of the AMOS package, takes longer sequences and compares them to each other, merges sequence with overlaps and outputs the super-contigs. The Galaxy implementation of this program is very simple and has very few options. The command-line has many more options.

> ## Challenge {.challenge}
> + Select the 'Minimus 2' tool in 'NGS: Assembly' section. 
> + Select the Contigs from the Velvet Optimiser of the 500 bp library as 'Contig Sequences 1'
> + Select the Contigs from the Velvet Optimiser of the 2000 bp library as 'Contig Sequences 2'
> + Select 'yes, add prefix' for both files
> + Hit Execute.

When the program finishes, then you'll get two files. One is of combined contigs, one of contigs that weren't merged. Both files constitute the assembly. Run the `assemblystats` tool on the Contigs file. Has this step improved our assembly at all?

Congratulations! You've put together a rough draft genome assembly.
