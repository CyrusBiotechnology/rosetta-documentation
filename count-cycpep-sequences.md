# Count Cyclic Peptide Sequences (count_cycpep_sequences) Application

Back to [[Application Documentation]].

Created 16 August 2019 by Vikram K. Mulligan, Flatiron Institute (vmulligan@flatironinstitute.org).  Approach derived by Todd Yeates, University of California Los Angeles (yeates@mbi.ucla.edu).<br/><br/>
<b><i>If you use this application, please cite:</i><br/>
Mulligan VK, Kang CS, Sawaya MR, Rettie S, Li X, Antselovich I, Craven T, Watkins A, Labonte JW, DiMaio F, Yeates TO, and Baker D. (2019).  Computational design of internally-symmetric peptide macrocycles with switchable folds.  Manuscript in prepration.</b><br/>

[[_TOC_]]

## Description

It is often useful to be able to count the number of possible sequences for a given design problem or chemical synthesis problem.  Given an **n**-residue linear peptide with **p** possibilities at each position, there are **p<sup>n</sup>** possible unique sequences.  For an N-to-C cyclic peptide, however, it becomes trickier to count the number of unique sequences.  A first approximation would be to divide by sequence length (**p<sup>n</sup>/n**), but this would still count sequences with internal symmetry more than once.  More challenging still is counting in the cases in which one is limiting oneself to sequences with internal symmetry, such as cyclic (C2, C3, C4...) or improper rotational (S2, S4, S6...) symmetry.

This application provides a convenient means of computing the number of possible sequences given sequence length **n**, possibilities at each position **p**, and symmetry type.

## Algorithms

### Analytic algorithm for cyclic symmetries (including the asymmetric case)

Given a peptide with CN symmetry (where **N** = 1 in the asymmetric case), the number of possible sequences is given by the following expression, derived from application of [Burnside's lemma](https://en.wikipedia.org/wiki/Burnside%27s_lemma):

![Expression for number of unique CN-symmetric sequences, U, given an n-residue macrocycle with p possibilities per position.](CN_expression.png)

In the above, **U** is the number of unique CN-symmetric sequences (where cyclic permutations of a sequence are considered to be the same sequence), **N** is the fold symmetry, **n** is the number of residues in the peptide, and **p** is the number of possibilities at each position.  Note that the expression is ultimately a function of **n/N**, the number of residues in the asymmetric unit, meaning that a C2-symmetric 10-mer (with 5 residues in the asymmetric unit) has the same number of possibilities as a C3-symmetric 15-mer (also with 5 residues in the asymmetric unit).  The expression **gcf(j,N/n)** represents the greatest common factor of **j** and **N/n**.

### Analytic algorithm for improper rotational symmetries:

Given a peptide with SN symmetry (where **N** = 2, 4, 6...) and a sequence that can be divided into symmetric unit "lobes" that repeat with sequence inversion (so that L-amino acids become D-amino acids from one lobe to the next and vice versa), the number of possible sequences is given by the following expression:

![Expression for number of unique SN-symmetric sequences, U, given an n-residue macrocycle with p possibilities per position.](SN_expression.png)

In the above, **U** is the number of unique SN-symmetric sequences (where once again cyclic permutations of a sequence are considered to be the same sequence), xxxxxCONTINUE HERExxxxx

## See also

* [Burnside's lemma](https://en.wikipedia.org/wiki/Burnside%27s_lemma) on Wikipedia.
* [Burnside's lemma](http://mathworld.wolfram.com/Cauchy-FrobeniusLemma.html) (a.k.a. the Cauchy-Frobenius lemma) on Wolfram Mathworld.
* [[Simple cyclic peptide structure prediction|simple_cycpep_predict]].