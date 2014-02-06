#RosettaAntibody3: Protocol Workflow

Metadata
========

Author: Jianqing Xu (xubest@gmail.com), Daisuke Kuroda (dkuroda1981@gmail.com), Oana Lungu (olungu@utexas.edu), Jeffrey Gray (jgray@jhu.edu)

Last edited 4/25/2013. Corresponding PI Jeffrey Gray (jgray@jhu.edu).

References
==========

We recommend the following articles for further studies of RosettaDock methodology and applications:

-   J. Xu, D. Kuroda & J. J. Gray, “RosettaAntibody3: Object-Oriented Designed Protocol and Improved Antibody Homology Modeling.” (2013) in preparation
-   A. Sivasubramanian,\* A. Sircar,\* S. Chaudhury & J. J. Gray, "Toward high-resolution homology modeling of antibody Fv regions and application to antibody-antigen docking," Proteins 74(2), 497-514 (2009)

Overview
========

**Please realize this the overview is to speed you up to run the protocol asap with minimum knowledge. For details of each steps, please check**

* [[RosettaAntibody3 application: the Python Pre-Processing Script|antibody-python-script]]
* [[RosettaAntibody3 application: Antibody CDR Grafting Protocol|antibody-assemble-CDRs]]
* [[RosettaAntibody3 application: Antibody Modeler Protocol (Loop H3 and VL-VH)|antibody-model-CDR-H3]]

To build an antibody model from sequences of its light chain and heavy chain, you need

1.  your input Fv sequences
2.  antibody.py (Downloading antibody.py from developer-only repository: [https://svn.rosettacommons.org/source/trunk/antibody/scripts.v2/](https://svn.rosettacommons.org/source/trunk/antibody/scripts.v2/) )
3.  ProFit (Installing ProFit3.1: [http://www.bioinf.org.uk/software/profit/](http://www.bioinf.org.uk/software/profit/) )
4.  BLAST (C++ version) (Installing BLAST: [http://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE\_TYPE=BlastDocs&DOC\_TYPE=Download](http://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download) )
5.  Rosetta

Currently, antibody homology modelling is a 3 step process:

1.  Template selections by BLAST
2.  Grafting of CDR templates onto a FR template and Fv refinement
3.  Intensive H3 modeling and VL/VH refinement

Usage for a Production Run
==================

**Steps 1 and 2:**

```
./antibody.py --light-chain <input_l.fasta> --heavy-chain <input_h.fasta> --profit=<ProFit> --blast=<blast> --blast-database=<blast_database> --antibody-database=<antibody_database> --rosetta-bin=<rosetta/rosetta_sourse/bin> --rosetta-database=<rosetta_database>
```

\<input\_l.fasta\> and \<input\_h.fasta\> are the files having sequences of the light and heavy chains, respectively, which you want to model.

Inputs:

1.  Sequence of the light chain Fv in FASTA format
2.  Sequence of the heavy chain Fv in FASTA format

Outputs:

1.  Sequence-grafted and refined Fv pdb: grafted.pdb, grafted.relaxed.pdb
2.  Constraints file for optional use in Step 3: cter\_constraint

The script calls two Rosetta executable (relax and antibody\_assemble\_CDRs) for grafting and refinement, respectively, by specifying “–rosetta-bin” option. You can see other options by typing: ./antibody.py –help

**Step 3:**

```
[path to executable] /antibody_model_CDR_H3.[platform|linux/mac][compile|gcc/ ixx]release –database [path to database] @options
```

Sample options for a production run may look like: (this is an example, see details in [[RosettaAntibody3 application: Antibody Modeler Protocol (Loop H3 and VL-VH)|antibody-model-CDR-H3]] .

Flags starting from "-kic\_bump\_overlap\_factor 0.36" to "-loops:outer\_cycles 5" will turn on the NGK or KIC2 for H3 loop modeling. Without these flags, the code is running KIC1 for H3

```
    -nstruct 2000   
    -s grafted.relaxed.pdb                             # Output of the antibody.py
    -antibody::remodel              perturb_kic        # low-res H3 modeling
    -antibody::snugfit              true               # VL-VH orientation optimization via docking
    -antibody::refine               refine_kic         # high-res H3 modeling
    -antibody::cter_insert          false              # H3 cterminal insertion using Kink/Extend fragments
    -antibody::flank_residue_min    true               # minimize 2 stem residues each side of H3 during modeling
    -antibody::bad_nter             false              # if n-terminal stem of H3 is bad and you have a pdb file with correct stem to copy
    -antibody::h3_filter            true               # using bioinformatics rules of Kink/Extend to filter out bad H3 decoys
    -antibody::h3_filter_tolerance  20                 # the maximum number of filtering is set to 20
    -ex1 -ex2 -extrachi_cutoff 0                       # packing options
    -constraints:cst_file cter_constraint              # constraint file which can include one or two lines of below optional constraints: 
    -antibody:constrain_cter                           #   optional constraint (a) the H3 cterminus to be Kink/Extend
    -antibody:constrain_vlvh_qq                        #   optional constraint (b) the distance between two GLN-GLN residues one L and H chains
    -kic_bump_overlap_factor 0.36                      # KIC1 become KIC2 (or NGK) after turning on the flags from here
    -corrections:score:use_bicubic_interpolation false
    -loops:legacy_kic false
    -loops:kic_min_after_repack true
    -loops:kic_omega_sampling
    -loops:allow_omega_move true
    -loops:ramp_fa_rep
    -loops:ramp_rama
    -loops:outer_cycles 5
```

Inputs:

1.  Sequence-grafted and refined Fv pdb: grafted.relaxed.pdb
2.  Constraints file for optional use, output from steps 1 and 2: cter\_constraint

Outputs:

1.  Set of modeled and refined Fv pdbs with loop modeled CDH3 loops: \<grafted.relaxed\_000X.pdb\> We recommend generating at least 2000 decoys during this step

Post Processing
===============

You can use a set of decoys simultaneously for antibody-antigen docking simulations, such as SnugDock and EnsembleDock.

New things since last release
=============================

This is the first public release in Rosetta3

-   Supports the modern job distributor (jd2).
-   Support for [[constraints|constraint-file]] .

