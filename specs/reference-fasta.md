---
layout: post
title: reference-fasta spec
version: v0.1.0
author: Chris Kent and Bede Constantinides  

---
spec version: `{{site.version}}`

---

### Summary 
A `reference.fasta` file contains the DNA sequences of all the primary-reference genomes, used in primerscheme generation. Its purpose is to provide a reference genome, and coordinate system to be used for referenced-based assembly and consensus generation.


# 1.1 Format overview  

`reference.fasta` files are typical `.fasta` [format files](https://en.wikipedia.org/wiki/FASTA_format), with text representing the nucleotide sequence of the reference. Each genome starts with a header line (starting with `>`) that denotes the id of the genome, followed by lines of nucleotide data. 

The id provided in the header line should match the chrom field of the corresponding primers in the `primer.bed` file.


# 1.2 Examples
Single fasta
```
>MN908947.3
ATTAAAGGTTTATACCTTCCCA...
```
> The corresponding bedfile should be `MN908947.3	47	78	...`

Multi fasta
```
>MN908947.3
ATTAAAGGTTTATACCTTCCCA...
>NC_006432.1
CGGACACACAAAAAGAAAGAAA...
``` 
> The corresponding bedfile should be `MN908947.3	47	78  ...` and `NC_006432.1	126	154	...`


# 1.3 Best practices 

As the `reference.fasta` is often used for referenced-based assembly, using high quality genome with minimal `Ns` or ambiguous bases is advisable. 

Using RNA sequences in the `reference.fasta` is not advice, as DNA is expected.