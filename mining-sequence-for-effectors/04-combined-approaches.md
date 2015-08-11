---
layout: page
title: Finding Effector Genes and Proteins in DNA Sequence
subtitle: Combination Approaches
minutes: 25
---

> ## Learning Objectives {.objectives}
> * Run and compare `published` effector finding pipelines
>

The methods we've looked at so far give us a range of tools to classify the unknown proteins into potential effectors. Each method has its own strength and weaknesses. The next approach will be to use complex ones that combine two or more of the approaches above into a new method, the three that we will try are all extracted from key papers. The method 'Win et al' was devised in Sophien Kamoun's group and is one of their approaches for detecting effector candidates from oomycetes. In this section we'll use these methods. These are all optimised on oomycete sequences, so we'll test them on some unclassified fungal sequences, our known _P.infestans_ effectors and our Hpa sequences. By comparing the output we will see the importance of being circumspect when applying one method to a new species!

> ## Run the `Black Box Methods`{.challenge}
> + Select the `Protein Analysis` `RXLR Motifs` tool
> + Select `unclassified.fa` as the input
> + Select `Win et al` as the RXLR model and click `Execute`
> + Repeat for the `Whisson et al` and `Bhattarcharjee et al` RXLR models
> + Repeat for the `avr_proteins.fa` and `hpa_effectors.fa`
>
> How applicable are the individual methods for detecting effectors in the different sets? How similar are the results within a set? How definitive do you find the results to be from any of these data and tools?
