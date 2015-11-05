#Rna Denovo: RNA 3D structure modeling

#Application purpose
To produce <i>de novo</i> models of small RNA motifs through Fragment Assembly of RNA with Full Atom Refinement (FARFAR).

Note that most of the functionality of rna_denovo/FARFAR is now available on a [ROSIE FARFAR server](http://rosie.rosettacommons.org/rna_denovo), if you want to do some easy tests.

#Code and Demo


The central code for the *rna\_denovo* application is in `       src/protocols/rna/RNA_DeNovoProtocol.cc      ` .

For a 'minimal' demo example of the RNA fragment assembly and full-atom minimization protocol and input files, see

`       demos/public/RNA_Denovo      `

#References


Das, R. and Baker, D. (2007), "Automated de novo prediction of native-like RNA tertiary structures", PNAS 104: 14664-14669. [for fragment assembly]. [Paper.](http://www.stanford.edu/~rhiju/Das_Baker_PNAS_2007.pdf) [Link.](http://www.pnas.org/content/104/37/14664.long)

Das, R., Kudaravalli, M., et al. (2007) "Structural inference of native and partially folded RNA by high throughput contact mapping", PNAS, 4144-4149. [for modeling large RNAs with constraints]. [Paper.](http://www.stanford.edu/~rhiju/das_MOHCA08.pdf) [Link.](http://www.pnas.org/content/105/11/4144.long)

Das, R., Karanicolas, J., and Baker, D. (2010), "Atomic accuracy in predicting and designing noncanonical RNA structure". Nature Methods 7:291-294. [for high resolution refinement] [Paper.](http://www.stanford.edu/~rhiju/DasKaranicolasBaker2010ALL.pdf) [Link.](http://www.nature.com/nmeth/journal/v7/n4/abs/nmeth.1433.html)

Sripakdeevong, P., Kladwang, W., and Das, R. (2011) "An enumerative stepwise ansatz enables atomic-accuracy RNA loop modeling", PNAS 108:20573-20578. [for loop modeling] [Paper](http://www.stanford.edu/~rhiju/Sripakdeevong_StepwiseAnsatz_2011.pdf) [Link](http://dx.doi.org/10.1073/pnas.1106516108)

#Application purpose


This code is intended to give three-dimensional de novo models of single-stranded RNAs or multi-stranded RNA motifs, with the prospect of reaching high (near-atomic-resolution) accuracy.

#Algorithm

The RNA structure modeling algorithm in Rosetta is based on the assembly of short (1 to 3 nucleotide) fragments from existing RNA crystal structures whose sequences match subsequences of the target RNA. The Fragment Assembly of RNA (FARNA) algorithm is a Monte Carlo process, guided by a low-resolution knowledge-based energy function. The models can then be further refined in an all-atom potential to yield more realistic structures with cleaner hydrogen bonds and fewer clashes; the resulting energies are also better at discriminating native-like conformations from non-native conformations. The two-step protocol has been named FARFAR (Fragment Assembly of RNA with Full Atom Refinement).

#Limitations


-   This method has been demonstrated to reach atomic accuracy for small motifs (12 residues or less) – the current bottleneck for larger RNAs is the difficulty of complete conformational sampling (as in other applications in Rosetta to, e.g., protein de novo modeling). On-going work attempts to resolve this issue, but requires great computational expense (see Sripakdeevong et al. paper above).

-   For larger RNAs, it appears most efficient to just carry out fragment assembly without refinement, specifying secondary structure (as described below). Although atomic accuracy is unlikely, models acccurate at nucleotide or helix resolution can be achieved, especially with constraints from experiments. See also: [[RNA assembly with experimental pair-wise constraints|rna-assembly]] and, more up to date, [[RNA de novo setup|rna-denovo-setup]].

-   As with most other modes in Rosetta, the final ensemble of models is not guaranteed to be a Boltzmann ensemble.

#Modes


-   By default, the code runs Monte Carlo fragment assembly, optimized in a knowledge-based low-resolution potential.

-   It is strongly suggested that you run with "-minimize\_rna", which permits the refinement in the high-resolution Rosetta potential, and results in models with few steric clashes and 'cleaner' hydrogen bonds.

-   There are variations of the code that permit just scoring or just minimizing in the high resolution Rosetta potential. These are described below in Tips.

#Input Files


##Required file


You need only one input file to run RNA structure modeling:

-   The [[fasta file]]: it is a sequence file for your rna.

##Optional additional files:


-   A [Parameter files to specify Watson/Crick base pairs and strand boundaries](#parameter-files-to-specify-watson/crick-base-pairs-and-strand-boundaries). This can specify base pairs that are held together during the run, as well as boundaries between independent chains.
-   Native pdb file, if all-heavy-atom rmsd's are desired. Must be in Rosetta's [PDB format for RNA](#File-Format).

##How to include these files.


A sample command line is the following:

```
rna_denovo.<exe> -fasta chunk002_1lnt_.fasta -nstruct 2 -out::file::silent test.out -cycles 1000
-minimize_rna 
```

The code takes about 1 minute to generate two models.

The fasta file has the RNA name on the first line (after \>), and the sequence on the second line. Valid letters are a,c,g, and u. The example fasta file is available in `       main/tests/integration/tests/rna_denovo/      ` .

<a name=parameter-files-to-specify-watson/crick-base-pairs-and-strand-boundaries />
##Parameter files to specify Watson/Crick base pairs and strand boundaries


RNA motifs are typically ensconced within Watson/Crick double helices, and involve several strands. [The most conserved loop of the signal recognition particle is an example, and is included here as chunk002\_1lnt\_RNA.pdb.] You can specify the bounding Watson/Crick base pairs in a "params file" with lines like the following:

```
CUTPOINT_OPEN 6    [means that one chain ends at residue 6]
STEM PAIR 1 12 W W A    [means that residues 1 and 12 should form a base pair with their Watson-Crick edges in
an antiparallel orientation]
```

and then run:

```
rna_denovo.<exe> -fasta chunk002_1lnt_.fasta -native chunk002_1lnt_RNA.pdb -params_file chunk002_1lnt_.prm -nstruct 2
-out::file::silent chunk002_1lnt.out -cycles 1000 -minimize_rna 
```

This command line also includes the "native" pdb, and will result in heavy-atom rmsd scores being calculated. Note that in early Rosetta versions, native pdb was required to have residues marked rA, rC, rG, and rU, but this is no longer the case after 2014 (see notes on PDB [[Format|loops-file#format]] below). The code again takes about 1 minute to generate two models. Finally, there are some notes on forcing other kinds of pairs below [ [Can I specify non-Watson-Crick pairs?](#Can-I-specify-non-Watson-Crick-pairs?) ].

##Use Of Alternative Fragment Sources


By default the RNA fragment assembly makes use of bond torsions derived from the large ribosome subunit crystal structure 1jj2, which have been pre-extracted in 1jj2. torsions (available in the database). If you want to use torsions drawn from a separate PDB (or set of PDBs), the following command will do the job.

```
rna_database.<exe>  -vall_torsions -s my_new_RNA1.pdb my_new_RNA2.pdb -o my_new_set.torsions
```

The resulting file is just a text file with the RNA's torsion angles listed for each residue. Then, when creating models, use the following flag with the rna\_denovo application:

```
-vall_torsions my_new_set.torsions
```

Similarly, the database of base pair geometries can be created with `rna_database -jump_library`, and then specified in the rna\_denovo application with `-jump_library_file`.

Last, a database of base pair step geometries (see [below](#Can-I-use-base-pair-steps?)) can be created with `rna_database -bps_database`. By default, this creates files for the standard canonical base pair steps. To also parse out noncanonical base pair steps, use  `-general_bps`; and `-use_lores_base_pair_classification` catches all pairs, including ones that are held in place by base-phosphate contacts but no base-base hydrogen bonds (as occurs in the sarcin/ricin loop). 

#Options


```
Required:
-in:fasta                                        Fasta-formatted sequence file. [FileVector]

Commonly used:
-out:file:silent                                 Name of output file [scores and torsions, compressed format]. default="default.out" [String]
-params_file                                     RNA params file name.[String]. For Example: -params_file chunk002_1lnt_.prm
-in:native                                       Native PDB filename. [File].
-out:nstruct                                     Number of models to make. default: 1. [Integer]
-minimize_rna                                    High resolution optimize RNA after fragment assembly.[Boolean]
-vary_geometry                                   Vary bond lengths and angles (with harmonic constraints near Rosetta ideal) for backbone and sugar degrees of freedom [Boolean]

Less commonly used, but useful
-cycles                                          Number of Monte Carlo cycles.[default 10000]. [Integer]
-bps_moves                                       Base pair step moves. For adjacent base pairs within stems or that are
                                                 obligate pairs, draw sequence-matched fragments that encompass both
                                                 pairs. Adjacent means that base pairs have contiguous residues on one 
                                                 strand, and at most 3 intervening residues on the other.   
-filter_lores_base_pairs                         Filter for models that satisfy structure parameters. [Boolean]
-output_lores_silent_file                        If high resolution minimizing, output intermediate low resolution models. [Boolean]
-dump                                            Generate pdb output. [Boolean]
-vall_torsions                                   Source of RNA fragments. [default: 1jj2.torsions]. [Boolean]
-jump_library_file                               Source of base-pair rigid body transformations if base pairs are specified.
                                                   [default: 1jj2_RNA_jump_library.dat] [String]
-close_loops                                     Attempt closure across chainbreaks by cyclic coordinate descent after fragment moves [Boolean]
-cst_file                                        Specify constraints (typically atom pairs) in Rosetta-style constraint file. [String]
-output_lores_silent_file                        if doing full-atom minimize, also save models after fragment assembly but before refinement (file will be called *.LORES.out) [Boolean]
-dump                                            output pdbs that occur during the run, even if using silent file output.

Advanced [used in rna_assembly]
-in:file:silent                                  List of input files (in 'silent' format) that specify potential template structures or 'chunks'
-input_res                                       Positions at which 'chunks' are applied. If there is more than one chunk file, specify indices for
                                                   the first file and then the second file, etc.
                                                 (Used to be called -chunk_res.)
-in:database                                     Path to rosetta databases. Default is based on location of rosetta executables. [PathVector]
```

#Tips


##File Format 
<a name="File-Format" />

Note that in older versions of Rosetta, the PDBs may have residue types marked as rA, rC, rG, and rU and unusual atom names. Versions of Rosetta released after 3.5 have residue and atom names matching BMRB/NDB standard nomenclature. If you have a "standard" PDB file, there is a python script available to convert it to current Rosetta format:

```
tools/rna_tools/bin/make_rna_rosetta_ready.py <pdb file>
```

##Can I specify non-Watson-Crick pairs? 
<a name="Can-I-specify-non-Watson-Crick-pairs?" />

You can also specify base pairs that must be forced, even at the expense of creating temporary chainbreaks, in the params file, with a line like:

```
OBLIGATE PAIR 2 11 W W A
```

This also allows the specification of non-Watson-Crick base pairs. In the line above, you can change the W's to H (hoogsteen edge) or S (sugar edge); and the A to P (antiparallel to parallel). The base edges are essentially the same as those defined in the classification by Leontis & Westhof. The latter (A/P) are determined by the relative orientation of base normals. [The cis/trans classification of Leontis & Westhof would be an alternate to the A/P, but we found A/P more convenient to compute and to visually assess.] The base pairs are drawn from a library of base pairs extracted from the crystallographic model of the large ribosomal subunit 1JJ2.

When specifying pairs, if there are not sufficient CUTPOINT\_OPEN's to allow all the pairs to form, the code will attempt to choose a (non-stem) RNA suite to put in a cutpoint, which can be closed during fragment assembly with the -close\_loops option. If you want to pre-specify where this cutpoint will be chosen, add a line like

```
CUTPOINT_CLOSED 6
```

##Can I use fragments that take advantage of our rich database of base pairings? 
<a name="Can-I-use-base-pair-steps?" />

Yes, by using the flags `-bps_moves`, you can ask the application to try to  draw from a database of "base pair steps". There are two kinds of those steps. 


First, for stems (specified by the "STEM" lines in params files), adjacent base pairs form base pair steps, involving four nucleotides (i,i+1,j,j+1) where (i,j+1) and (i+1,j) are paired. There is a set of such steps in Rosetta's database, drawn from the ribosome. The RNA's fold tree will be set up with appropriate jump connections and cutpoints so that those base step conformations can be substituted in during fragment assembly.

Second, if you have specified obligate pairs -- but with unknown pairing edges and orientations ('X' in the params file) -- special fragments will be set up for such pairs that involve nucleotides that are adjacent in sequence. For example, if your params file contains lines like:

```
OBLIGATE PAIR 2 11 X X X
OBLIGATE PAIR 3  8 X X X
```

using the flag `-bps_moves` will trigger moves that substitute sequence-matched fragments for the nucleotides at (2,3,8,11). This happens if on at least one strand, the base pair step involves residues that are immediately contiguous (2 and 3 in this example). On the other strand, the base pair step must involve residues that are contiguous are have no more than 3 intervening 'bulge' residues (8 and 11 in this example). Note that these base pair steps will generally include noncanonical pairs.  There's a demo of this functionality applied to model the sarcin/ricin loop in `demos/public/RNA_Denovo_with_base_pair_steps/`


Note: For noncanonical pairs, we don't allow specification of edges and orientations at the moment -- the database gets pretty sparse with that level of specification. Also note: If there is a base pair step that includes a pair both inside a Watson/Crick stem and a more general 'obligate pair', the stem pairing may actually come out as non-Watson-Crick, which often happens anyway for base pairs at the edge of stems.


##What do the scores mean?


The most common question we get is on what the terms in the 'SCORE lines' of silent files mean. Here's a brief rundown, with more explanation in the papers cited above.

```
***Energy interpreter for low resolution silent output:
score                                              Final total score
rna_rg                                           Radius of gyration for RNA
rna_vdw                                          Low resolution clash check for RNA
rna_base_backbone                                Bases to 2'-OH, phosphates, etc.
rna_backbone_backbone                            2'-OH to 2'-OH, phosphates, etc.
rna_repulsive                                    Mainly phosphate-phosphate repulsion
rna_base_pair_pairwise                           Base-base interactions (Watson-Crick and non-Watson-Crick)
rna_base_pair                                    Base-base interactions (Watson-Crick and non-Watson-Crick)
rna_base_axis                                    Force base normals to be parallel
rna_base_stagger                     Force base pairs to be in same plane
rna_base_stack                                   Stacking interactions
rna_base_stack_axis                              Stacking interactions should involve parallel bases.
atom_pair_constraint                             Harmonic constraints between atoms involved in Watson-Crick base
                                                 pairs specified by the user in the params file
rms                                              all-heavy-atom RMSD to the native structure

***Energy interpreter for fullatom silent output:
score                                            Final total score
fa_atr                                           Lennard-jones attractive between atoms in different residues
fa_rep                                           Lennard-jones repulsive between atoms in different residues
fa_intra_rep                                     Lennard-jones repulsive between atoms in the same residue
lk_nonpolar                                      Lazaridis-karplus solvation energy, over nonpolar atoms
hack_elec_rna_phos_phos                          Simple electrostatic repulsion term between phosphates
hbond_sr_bb_sc                                   Backbone-sidechain hbonds close in primary sequence
hbond_lr_bb_sc                                   Backbone-sidechain hbonds distant in primary sequence
hbond_sc                                         Sidechain-sidechain hydrogen bond energy
ch_bond                                          Carbon hydrogen bonds
geom_sol                                         Geometric Solvation energy for polar atoms
rna_torsion                                      RNA torsional potential.
atom_pair_constraint                             Harmonic constraints between atoms involved in Watson-Crick base pairs
                                                 specified by the user in the params file
angle_constraint                                 (not in use)

N_WC                                             number of watson-crick base pairs
N_NWC                                            number of non-watson-crick base pairs
N_BS                                             number of base stacks

[Following are provided if the user gives a native structure for reference]
rms                                              all-heavy-atom RMSD to the native structure
rms_stem                                         all-heavy-atom RMSD to helical segments in the native structure, defined by 'STEM' entries in the parameters file.
f_natWC                                          fraction of native Watson-Crick base pairs recovered
f_natNWC                                         fraction of native non-Watson-Crick base pairs recovered
f_natBP                                          fraction of base pairs recovered
```

##How do I just score?
<a name="How-do-I-just-score?" />

To get a score of an input PDB, you can run the 'denovo' protocol but ask there to be no fragment assembly cycles and no rounds of minimization:

```
rna_score.<exe> -database <path to database>  -s <pdb file> [<pdb file 2> ...] -out:file:silent SCORE.out  [-native <native pdb>]
```

If you want to minimize under the low resolution RNA potential (used in FARNA), add the flag '-score:weights rna\_lores.wts'. Then you can check the score in SCORE.out:

```
 grep SCORE SCORE.out
```

But this is not recommended if you are trying to score a model deposited in the PDB or created by other software – see next [How do I just minimize?](#How-do-I-just-minimize?)

##How do I just minimize? 
<a name="How-do-I-just-minimize?" />

If you take a PDB created outside Rosetta, very small clashes may be strongly penalized by the Rosetta all-atom potential. Instead of scoring, you should probably do a short minimize, run:

```
rna_minimize.<exe> -database <path to database>  -s <pdb file> [<pdb file 2> ...] -out:file:silent MINIMIZE.out  [-native <native pdb>]
```

If you want to minimize under the low resolution RNA potential (used in FARNA), add the flag '-score:weights rna\_lores.wts'. Then check out the scores in MINIMIZE.out.

```
 grep SCORE MINIMIZE.out
```

You can extract models from silent files as described in [Extraction Of Models Into PDB Format](#Extraction-Of-Models-Into-PDB-Format), but you'll also get models with the same names as your input with the suffix '\_minimize.pdb'.

##Other options

For building models of larger RNAs, check these sections: [[RNA assembly with experimental pair-wise constraints|rna-assembly]] and [[RNA denovo setup|rna-denovo-setup]].

#Expected Outputs


You will typically use the protocol to produce a silent file – how do you get the models out?

#Post Processing


##Extraction Of Models Into PDB Format
<a name="Extraction-Of-Models-Into-PDB-Format" />

The models from the above run are stored in compressed format in the file test.out, along with lines representing the score components. You can see the models in PDB format with the conversion command.

```
rna_extract.<exe>  -in:file:silent test.out -in:file:silent_struct_type  rna -database <path to database>
```

Note that the PDBs have residue types marked as rA, rC, rG, and rU.

##How can I cluster models?
<a name="How-can-I-cluster-models?" />

There is one executable for clustering, it currently requires that all the models be in a silent file and have scores. (If you don't have such a silent file, use the rna\_score executable described in [How do I just score?](#How-do-I-just-score?) ). Here's the command line:

```
 rna_cluster.<exe>   -database <path to database>    -in:file:silent <silent file with models> -out:file:silent <silent file with clustered models>   [-cluster:radius <rmsd threshold>] [-nstruct <maximum number of clusters>]
```

The way this clustering works is it simply goes through the models in order of energy, and if a model is more than the rmsd threshold than the existing clusters, it spawns a new cluster.

#New things since last release


Added applications rna\_minimize, rna\_helix, rna\_cluster. Updated torsional potential to be smooth.

Written in 2008. Last updates: Nov. 2011 and Aug. 2014 by Rhiju Das (rhiju [at] stanford.edu).


##See Also

* [[RNA applications]]: The RNA applications home page
* [[Structure Prediction Applications]]: List of structure prediction applications
* [[Application Documentation]]: Home page for application documentation
* [[RNA]]: Guide to working with RNA in Rosetta
* [[Running Rosetta with options]]: Instructions for running Rosetta executables.
* [[Comparing structures]]: Essay on comparing structures
* [[Analyzing Results]]: Tips for analyzing results generated using Rosetta
* [[Solving a Biological Problem]]: Guide to approaching biological problems using Rosetta
* [[Commands collection]]: A list of example command lines for running Rosetta executable files