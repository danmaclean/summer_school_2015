---
layout: page
title: Finding Causative Mutations With A Candidate SNP Approach
subtitle: Reference
---

## Glossary

Mutational Genomics
:	an experimental process that combines mutagenesis and genomics tools to help us discern the functions of genes.

Fastq
:	a text-based sequence file format that incorporates sequence and quality scores, see [https://en.wikipedia.org/wiki/FASTQ_format](https://en.wikipedia.org/wiki/FASTQ_format)

ASCII
:	An acronym from American Standard Code for Information Interchange. Does things like describe letter characters as numbers that the computer can understand and provides the basis of the Fastq quality encoding [https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart](https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart)

Phred
:	A score that describes the likelihood of error in a single base call from a sequencing machine [https://en.wikipedia.org/wiki/Phred_quality_score](https://en.wikipedia.org/wiki/Phred_quality_score)

FastQC
:	A useful and widely used sequence quality assessment program [http://www.bioinformatics.babraham.ac.uk/projects/fastqc/](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

Encoding
:	A confusion in exactly what symbol represents what number in Phred quality scores [https://en.wikipedia.org/wiki/FASTQ_format#Encoding](https://en.wikipedia.org/wiki/FASTQ_format#Encoding)

Trimmomatic
:	[http://www.usadellab.org/cms/?page=trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)

Kmer
:	A sequence of length `k`. Often used to describe the population of all `k`-length sub-sequences of a larger sequence or sequence set, such as a set of reads.

Whole Genome Shotgun
:	A sequencing method whereby the chromosomes are shattered into fragments and each sequenced individually.

Genetic Mapping
:	A method where molecular markers a ordered relative to each other using molecular genetic techniques [https://en.wikipedia.org/wiki/Gene_mapping](https://en.wikipedia.org/wiki/Gene_mapping)

Burrows Wheeler Transform
:	A method for creating an index of subsequences so a sequence space can be searched very quickly [http://en.wikipedia.org/wiki/Burrows–Wheeler_transform](http://en.wikipedia.org/wiki/Burrows–Wheeler_transform)

BWA
:	A fast, general purpose HTS read aligner that uses Burrows-Wheeler Transforms. [http://bio-bwa.sourceforge.net/](http://bio-bwa.sourceforge.net/)

SAM
:	An uncompressed, text- and line-based format for recording sequence alignments. [https://samtools.github.io/hts-specs/SAMv1.pdf](https://samtools.github.io/hts-specs/SAMv1.pdf)

Paired-end Reads
:	Some sequencing strategies sequence the two ends of a fragment of DNA of known length but not the middle bit, so that we end up with two reads where we know the distance between them. We can use the distance to better align each read by looking for alignments that fit with the real distance between.

PileUp
:	A text based format that describes in a base-by-base way, the alignments of nucleotides from reads over each position in the genome. [A guide to the format can be found here](http://samtools.sourceforge.net/pileup.shtml)

Synonymous
:	The evolutionary substitution of one base for another in an exon of a gene coding for a protein, such that the produced amino acid sequence is not modified. [https://en.wikipedia.org/wiki/Synonymous_substitution](https://en.wikipedia.org/wiki/Synonymous_substitution)

Non-synonymous
:	A nucleotide mutation that alters the amino acid sequence of a protein.[https://en.wikipedia.org/wiki/Nonsynonymous_substitution](https://en.wikipedia.org/wiki/Nonsynonymous_substitution)
