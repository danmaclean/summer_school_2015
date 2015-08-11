---
layout: page
title: Finding Effector Genes and Proteins in DNA Sequence
subtitle: Motif finding with Regular Expressions
minutes: 25
---

> ## Learning Objectives {.objectives}
> * Understanding what a regular expression is?
> * Know how to apply a regular expression search to some text


Many classes of effector have characteristic sequence motifs (discrete shared patterns of amino acids), including the famous *RXLR* effectors that have an aRginine, anything, Leucine, aRginine motif. Computationally, motifs are easy to find with a 'regular expression', essentially a pattern matching process. However this approach is not terribly robust and only ever enables us to a find a pre-defined sequence. Let's quickly use regular expressions to find motifs.

> ## 'Not Terribly Robust' {.callout}
> English, when spoken by the English can have a lovely, gentle, understated quality. Just so you know I, the author of this document, am English. These are both important facts to consider when evaluating my assessment of regular expressions as 'not terribly robust' - I mean they're crap for this job. But they are quick and easy to implement - don't base your science on searches with regular expressions alone but do use them to get a feel for whether a week of analysis might even be worth starting.  

Regular expressions are actually a small but powerful computer scientist's way of encoding patterns of text, with the idea that it makes flexible text searching possible. If you've ever used the `*` wildcard in Word or a web-browser, then you've dabbled with regular expressions. The expression for RXLR is very easy, we literally want R, then a wildcard (which is usually `.` in regular expressions) and then LR. So our full expression will be `R.LR`. Let's use the `Regular Expression` tool in Galaxy to examine an effector set to see which have an RXLR.

> ## {.challenge}
>First, we need to convert the FASTA to Tabular format to get all the data on a single line per sequence
> + Select FASTA-to-Tabular in FASTA Manipulation tool and select your `avr_proteins.fa` file
> + Select 1 as the number of columns to divide title into, click Execute.
> + When the job is run, use the output in the `Filter and sort` `Select lines that match an expression` tool
> + Select the `avr_proteins.fa` file as the input, type `R.LR` into the pattern box and select `Execute`
> + Hopefully, _ALL_ the sequences should have been retained.
> + Repeat the exercise with the `unclassified.fa` sequences, how many of the `unclassified` proteins have full RXLRs?
