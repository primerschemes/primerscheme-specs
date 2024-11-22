---
layout: post
title: primer-bedfile spec
version: v0.1.0
author: Chris Kent
---
{{page.version}}

---

### Summary 
The primer.bed file is the main file created by tooling for amplicon sequencing. Its purpose is to provide all the information required to reproduce the primer scheme. This includes the wet lab elements; primer sequences, which pool they belong to, and primer weights, and the analysis elements; which genomes the primers belong to, and their location in the genome. 




# 1.1 Brief description  

`primer.bed` files are whitespace-delimited file format. With each line in the file representing a primer. This work is heavily based off, and designed to be broadly compatible with the [.bed](https://samtools.github.io/hts-specs/BEDv1.pdf) file spec and associated tooling. 



| Col | Meaning       | Type            | Brief description             | Restrictions                                 |
| --- | ------------ | --------------- | ----------------------------- | -------------------------------------------- |
| 1   | chrom        | String          | Chromosome name               | `[A-Za-z0-9_]`                               |
| 2   | primerStart  | Int             | Primer start position         | `u64`                                        |
| 3   | primerEnd    | Int             | Primer end position           | `u64`                                        |
| 4   | primerName   | String          | Primer name                   | `[a-zA-Z0-9\-]+_[0-9]+_(LEFT\|RIGHT)_[0-9]+` |
| 5   | pool         | Int             | Primer pool                   | `u64`                                        |
| 6   | strand       | String          | Primer strand                 | `[-+.]`                                      |
| 7   | primerSeq    | String          | Primer Sequence in 5' to 3'   | `[ABCDGHKMNRSTUVWY]`                         |
| 8   | primerWeight | Optional(float) | Primer weight for rebalancing | `f64`                                        |

# 1.2 attributes 

1. `chrom`: The name of the Chromosome for the primer. This should match the ID located in the reference.fasta header.
2. `primerStart`: The start position of the primer on the `chrom`. 
3. `primerEnd`: The non-inclusive end position of the primer on the `chrom`. Must be greater than `primerStart`.
4. `primerName`: The name of the primer in the form `{runid}_{amplicon_number}_{direction}_{primer_number}`. 
    - `runid`: Must match regex `[a-zA-Z0-9\-]`.
    - `amplicon_number`: The number of the amplicon. Must be a positive integer. 
    - `direction`: The direction of the primers. Either `LEFT` or `RIGHT`.
    - `primer_number`: The number of the primer. Must be a positive integer.
5. `pool`: The PCR pool the primer belongs to. Must be a positive integer. **1-based**.
6. `strand`: The strand of the primer. Either `+` or `-`. Should match the `primerName:direction` (`LEFT`=`+`, `RIGHT`=`-`)
7. `primerSeq`: The sequence of the primer in the 5' to 3' direction. Restricted to DNA [IUPAC](https://academic.oup.com/nar/article/13/9/3021/2381659) codes.
8. `primerWeight`: The normalised weight for each primer for each pool. Can be left blank for Equimolar pools.


# 1.3 examples

7col file with no `primerWeight`
```
MN908947.3	47	78	SARS-CoV-2_1_LEFT_1	1	+	CTCTTGTAGATCTGTTCTCTAAACGAACTTT
MN908947.3	419	447	SARS-CoV-2_1_RIGHT_1	1	-	AAAACGCCTTTTTCAACTTCTACTAAGC
MN908947.3	344	366	SARS-CoV-2_2_LEFT_0	2	+	TCGTACGTGGCTTTGGAGACTC
MN908947.3	707	732	SARS-CoV-2_2_RIGHT_0	2	-	TCTTCATAAGGATCAGTGCCAAGCT
```

8col file with `primerWeight`
```
MN908947.3	47	78	SARS-CoV-2_1_LEFT_1	1	+	CTCTTGTAGATCTGTTCTCTAAACGAACTTT	1.4
MN908947.3	419	447	SARS-CoV-2_1_RIGHT_1	1	-	AAAACGCCTTTTTCAACTTCTACTAAGC	1.4
MN908947.3	344	366	SARS-CoV-2_2_LEFT_0	2	+	TCGTACGTGGCTTTGGAGACTC	1.6
MN908947.3	707	732	SARS-CoV-2_2_RIGHT_0	2	-	TCTTCATAAGGATCAGTGCCAAGCT	1.6
```

# 1.4 best practices 

