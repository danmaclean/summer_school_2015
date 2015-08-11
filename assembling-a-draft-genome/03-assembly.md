---
layout: page
title: Assembling a draft genome in the Galaxy Interface
subtitle: Assembling and assessing assemblies
minutes: 25
---

> ## Learning Objectives {.objectives}
> * Using Velvet`s two step assembly process
> * Evaluating assembly quality
> * Changing parameters and estimating effect

Now that our reads are QC'd we can start to do an assembly. The reads we have prepared are selected from the _Phytophthora infestans T30-4 genome_. They cover a very small 170kb region on SuperContig 43 from 740-910Kb. This region contains some nice `core orthologues` and the odd effector. The reads are sufficient to cover the region to 30x, which is an average amount of coverage for a first assembly with short reads.

> ## x Coverage {.callout}
> When we talk about coverage of the genome we`re really talking about the average number of times we expect each base of the genome to have been sequenced. Sequencing is `shotgun` that is to say we smash up the genome and sample bits of it at random - so if we have a 1 Mb genome and we collect 30 Mb of sequence data at random, each bit should appear 30 times.  

Sequence assembly _de novo_ seeks simply to form longer sequences from short ones, accurately and efficiently without the assistance of any reference sequence. The long sequences that are produced are called contigs and should span an entire chromosome but this is not often achievable because most eukaryotic and prokaryotic genomes contain regions that cannot be sequenced or assembled, leaving most genomes assembled into a few (or many) large fragments - this is a draft genome.


> ## Assembler history {.callout}
> _de novo_ assembly is not a problem that has emerged with the next-generation sequencing, there are numerous well-established algorithms and tools for assembly of capillary sequence reads. These methods do not work with next-generation reads for several reasons: the distributions of sequencing errors are different making the prior error models inappropriate, they rely on overlaps between sequence reads that do not exist in short sequence reads and are unable to deal with the number of sequence reads.

Next Generation Sequence reads are typically assembled using de Bruijn graph assemblers, such as Velvet (Zerbino et al., 2008), ABYSS (Simpson et al., 2009), Euler (Chaisson et al., 2008), SOAPdenovo (Li et al., 2010) and ALLPATHS (Butler et al., 2008).

> ## How Assemblers work {.callout}
> In the majority of these assemblers the de Bruijn graph is an internal data structure that represents a network in which the linked entities are (usually) k-mers (short fragments of the reads) and links are made between k-mers that overlap by k-1 with a single nucleotide overhang. In perfect data, (i.e. a situation with no read errors and with k long enough to include the longest repeat in a single k-mer) then it is theoretically possible to reconstruct the genome by following from k-mer to k-mer along the edges, passing through each k-mer once. Since next-generation sequencing data do contain errors and genomic repeats can be very long (many times longer than read length, let alone the length of k used) then the read errors can cause dead ends in the path that terminate extension of the contig, repeats in the genome can cause cycles in the graph. The different ways in which the assembly programs get around these problems is what separates them.

Velvet is one of the more popular and reliable _de novo_ assemblers for short-read sequence. Velvet works by first breaking reads down into the constituent k-mers and forming the de Bruijn graph. The first part of using Velvet is to run `velveth` on the reads, this step makes the data structure which is used later.

> ## Interlacing {.callout}
> Velvet requires it's input files to be interlaced, that is the left and right reads should be in the same file one after the other, in the `velveth` tool Galaxy will do this for you behind the scenes. We will do it manually so we don't need to do it for a later step.

> ## Interlacing {.challenge}
> Under `NGS: QC and manipulation` select `FASTQ interlacer on paired end reads`.
> Select the left and right files to interlace. You may need to set the attributes!
>

> ## Challenge {.challenge}
> Note the interface can be picky about the exact datatype so you _may_ need to change the read format in the attributes box - again. Also the interface sometimes just refuses to show the items. Try forcing it to look again by resetting the format between `fasta` and `fastq` in the `velveth` window. Apologies for this - it is just a weird bug...
>
> + In the tool list under `NGS:Assembly`, select `velveth`
> + Set the parameters as follows:
> + Hash Length = 27
>	+ Click `insert files` to load a read file, you`ll need the interlaced file for the assembly, as these are `paired`
>	+ `FASTQ` format
>	+ `Set Short Library Type reads` for the first set (interlaced 500) and don't use the insert 2000 sets for this step.
> + Click Execute, `velveth` will build the data structure. You can examine the output when it is done, but remember nothing is assembled yet.
>

The second step is to let the assembly algorithm `velvetg` work through the produced data structure and create contigs.

> ## Challenge {.challenge}
> + In the tool list under `NGS:Assembly`, select `velvetg`
> + Examine all the parameters and convince yourself you are happy with how they are set.
> + Hit execute, the assembly will be placed in an item called `Contigs`. You will also get `Unused Reads` and `Stats`. Examine all these files.

Once we have contigs we can start to examine them. One obvious metric is the number and length of the contigs. the longer the better. The assembly statistics tool gives us a summary of distribution of contig size. A statistic called N50, which is the contig length such that using equal or longer contigs produces half the bases of the genome, is a commonly used one as it encompasses both length and contig number. It does rely on knowing an approximate value for the genome you are trying to assemble.

> ## Challenge {.challenge}
> + Select the `assemblystats` tool in `NGS: Assembly` section.
> + Choose `contig` for `Type of Read` and your Velvet contigs for the `Source file in FASTA format`.
> + Click execute, when its done you should get a file of statistics and a histogram. Examine these and see what you think about the assembly you have produced. Is it any good?
> + What do you think about the quality of the assembly you produced? How big were the reads to start with? How big are the contigs you made? Is the assembly any good?

>## Finding the best assembly {.callout}
An additional complication is that the performance of each of the assemblers is sensitive to input parameter values, it is not usually possible to determine a priori the best parameter values (indeed, the ‘best’ parameter values may depend entirely on the final downstream application). To get round this we carry out assemblies at a wide-range of parameter values (usually we check a wide range of k values) and compare the resulting contig sets with a view to processing the set of sequences downstream. This can take the form of clustering and removing identical redundant sequences to make a maximal set or using a set of control sequences such as commonly occurring orthologue sets, to aid assessment of the reconstruction of these core genes as an indicator of assembly success.

Congratulations! You`ve put together a rough draft genome assembly.
