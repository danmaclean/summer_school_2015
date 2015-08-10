---
layout: page
title: Finding Effector Genes and Proteins in DNA Sequence
subtitle: Classifying with BLAST
minutes: 25
---

> ## Learning Objectives {.objectives}
> * Use BLAST to define a sensible cut-off
> * Use a sensible cut-off to classify sequences into groups

BLAST is the canonical sequence comparison tool and a very heavily used tool. Sometimes it is used too much and without proper attention to the weakness of the method. BLAST functions by creating an indexed database of segments of all the sequences provided to it, called 'words'. The 'words' of the query sequence are then compared to the index and 'hits' found. These small hits are extended to find all the regions of identity. The regions of matches are called High Scoring Pairs (HSPs).

In this part of the practical we will use BlastP (the protein sequence optimised variant of BLAST) to circumspectly examine sequence similarity between the known proteins and the candidates to identify potential matches. The basic principle is to compare all the known Avr proteins to each other using BLAST and examine some of the statistics, including the E-Score and the percent identity, to find a figure for the groups internal similarity. We can then use this threshold to classify our unknown proteins.

Let's use the version in Galaxy to

> ## Use BLAST to {.challenge}
> + Load in the file 'hpa_effectors.fa' with the 'Get Data' tool, this is a file of sequences from Hyaloperonospora arabidopsidis (Hpa).
> + Select the 'NCBI BLAST+ blastp' tool in the 'NCBI BLAST+' category
> + Use 'unclassified.fa' as the Query sequences
> + Select 'FASTA file' for 'Subject database/sequences:'
> + Select 'hpa_effectors.fa' for 'Protein FASTA file to use as database:'
> + Set 'Set expectation value cutoff:' to 1 (A deliberately high value to make sure we catch very distant similarity).
> + Leave 'Type of BLAST' as 'blastp' and click 'Execute'.

When the job has run you will have a table of BLAST results of unclassified proteins against Hpa, the table columns contain information as follows:

<table border="1" class="docutils">
<colgroup>
<col width="10%" />
<col width="15%" />
<col width="75%" />
</colgroup>
<tbody valign="top">
<tr><td>Column</td>
<td>NCBI name</td>
<td>Description</td>
</tr>
<tr><td>1</td>
<td>qseqid</td>
<td>Query Seq-id (ID of your sequence)</td>
</tr>
<tr><td>2</td>
<td>sseqid</td>
<td>Subject Seq-id (ID of the database hit)</td>
</tr>
<tr><td>3</td>
<td>pident</td>
<td>Percentage of identical matches</td>
</tr>
<tr><td>4</td>
<td>length</td>
<td>Alignment length</td>
</tr>
<tr><td>5</td>
<td>mismatch</td>
<td>Number of mismatches</td>
</tr>
<tr><td>6</td>
<td>gapopen</td>
<td>Number of gap openings</td>
</tr>
<tr><td>7</td>
<td>qstart</td>
<td>Start of alignment in query</td>
</tr>
<tr><td>8</td>
<td>qend</td>
<td>End of alignment in query</td>
</tr>
<tr><td>9</td>
<td>sstart</td>
<td>Start of alignment in subject (database hit)</td>
</tr>
<tr><td>10</td>
<td>send</td>
<td>End of alignment in subject (database hit)</td>
</tr>
<tr><td>11</td>
<td>evalue</td>
<td>Expectation value (E-value)</td>
</tr>
<tr><td>12</td>
<td>bitscore</td>
<td>Bit score</td>
</tr>
</tbody>
</table>

We will now examine the internal similarity of Hpa effectors and use this to come up with a threshold for our classifier.

> ## {.challenge}
> Re-run the BLASTP with `Hpa` against itself
>

The important values will be such things as `E-value`, `hit length`, `percent identical`. Examine each protein's hits and (excluding self hits) see what you think is a reasonable set of figures for similarity. The figures decided upon will vary quite widely for every dataset you ever try. Compare the figures that you get here with the ones you got for the comparison of unclassified proteins against hpa. You'll see that this sort of analysis can be very plastic, different values are needed every time, dependent on what the current control set is. Does BLAST similarity alone seem like a good way to classify proteins?

> ## {.challenge}
> With the results of the previous BLAST file, use Galaxy's `Text Manipulation` and `Filter` tools to remove low similarity matches.
>

With BLAST approaches we can compare candidates from a pathogenic species with proteins from a related non-pathogenic species using the principle that any proteins in the pathogenic species without matches in the non-pathogenic relative are likely to be related to pathogenesis.


> ## {.challenge}
> Lets see whether we can use a negative control set to classify potential pathogenesis related proteins. We have two new files, this time from bacteria, one a pathogen of tomato (_Pseudomonas syringae pv tomato DC3000_) and a non-pathogenic relative (_Pseudomonas putida F1_).
>
> + Set up the BLASTP using the files `p_syringae_dc3000.fa` and `p_putida_f1.fa`, as you did above. Remember to use P.putida as the database!
> + Use the `Text Manipulation` tool-set in Galaxy to find any genes in _P.syringae DC3000_ that don't have matches in _P.putida_.
