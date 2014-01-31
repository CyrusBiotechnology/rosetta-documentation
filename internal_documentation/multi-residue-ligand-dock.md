<!-- --- title: Multi-Residue-Ligand-Dock -->Documentation for multi\_residue\_ligand\_dock

 Author   
Gordon Lemmon

location: doc/apps/pilot/multi\_residue\_ligand\_dock.dox

last updated: 5-25-2009

Purpose
=======

This application performs ligand docking. It refactors previous versions of RosettaLigand. New features include docking of multiple ligands simultaneously and representing ligands as multiple fragments. These ligand fragments can have rotamer libraries and thus can be sampled just like side chain rotamers.

Caveat: this code currently only works with 2 ligand fragments. To get rotamer sampling to work correctly the neighbor atoms of the two fragments must be the same atoms as the CONNECT atoms (ie the atoms whose bond was broke to split the ligand into 2 fragments). Generalizing this is our next goal.

Table of Contents
=================

-   [[Overview of the Application|template-app-documenation-page#Overview]]
-   [[Relevant Literature|template-app-documenation-page#Literature]]
-   [[Required Tools and Input files|template-app-documenation-page#Needed-tools]]
-   [[Optional Tools and Input files|template-app-documenation-page#Optional-tools]]
-   [[Location of Demo Files|template-app-documenation-page#Demo-files-location]]
-   [[Preprocessing of input files|template-app-documenation-page#Preprocessing]]
-   [[Running the Application|template-app-documenation-page#Running]]
-   [[Postprocessing of Files|template-app-documenation-page#Post-processing]]
-   [[Frequently Asked Questions|template-app-documenation-page#Faq]]

Overview of the Application
===========================

As a fully refactored version of ligand\_dock, this application breaks one class into 15 or so classes. Additionally it utilizes JD2, streamlining input and output choices. New features include multiple ligand docking and multiple fragment ligands with ligand fragment rotamer sampling.

Relevant Literature
===================

-   Jens Meiler, David Baker. 2006. ROSETTALIGAND: protein-small molecule docking with full side-chain flexibility. Proteins. 65(3):538-48. The original application from Rosetta++
-   Ian Davis, David Baker. 2008. RosettaLigand Docking with Full Ligand and Receptor Flexibility. JMB 385:381-392. RosettaLigand implemented in MiniRosetta (Rosetta3) and released with several new features. Stretches of residues near the ligand are chosen for backbone minimization. Gradient minimization of phi/psi angles occures within these stretches. Also gradient minimization is applied to ligand torsion angles, and whole ligand conformers can be sampled.

Required Tools and Input files
==============================

The user will need:

-   a PDB with ligands placed appropriately (or separate PDBs to be combined)
-   Ligand '.params' files describing the chemistry of each ligand residue
-   a 'flags.txt' file that describes various standard Rosetta Options (input type, output type, nstructs,database location, and location of additional .params files). The flags file must list a docking:ligand:option\_file \<fileName\> option.
-   a "Ligand Options File" with a special syntax demonstrated in the demo directory. This file describes the default treatment of each ligand if any, as well as Ligand specific options. The user lists the unique one-letter chain ID for each ligand that they wish to dock. For each ligand the user lists options such as the amount/type of rotation and translation, whether to minimize the ligand, and whether to minimize the backbone around the ligand.

Optional Tools and Input files
==============================

-   Optional '.confs' files which contain TER separated HETATM records and describe various ligand residue conformations. If utilized these ligand rotamer libraries will be sampled with side chain rotamers during packing steps.

Location of Demo Files
======================

See readme\_for\_multi\_ligand\_dock\_demos within the demo directory shown above.

Preprocessing of input files
============================

rosetta/rosetta\_source/src/apps/public/ligand\_docking/pdb\_to\_molfile can be used to create a mol file for your ligand Next rosetta/rosetta\_source/python/apps/public/molfile2params can be used to create the necessary ligand .params files.

Running the Application
=======================

Here I will describe the syntax of the ligand option file.

Comments in this file begin with a '\#' sign
============================================

There are two section flags, 'DEFAULT' and 'LIGAND '
====================================================

Some options are only available as default options (they are not ligand specific)
=================================================================================

Other options are only available as ligand specific options (default doesn't make sense)
========================================================================================

Most options can be used either by default or as ligand specific options.
=========================================================================

Caveat: A ligand will NOT be docked by default. You must provide the 'LIGAND '
==============================================================================

tag even if you are going to use all default options.

DEFAULT \# applies to all ligands listed after LIGAND keywords. soft\_rep [old|new] \# default-only option with two accepted parameters. If provided, use the soft repulsive score during

docking (but continue using hard-repulsive during the final minimization). 'old' uses Rosetta++ electrostatics and is
=====================================================================================================================

recommended.
============

protocol [meiler2006|abbreviated|abbrev2|min\_only|custom \<int\> \<int\>] \# DEFAULT only option. The docking step consists of

cycles of either rotamer trials (1 residue at a time) or a "full repack" (several residues receive rotamer trials
=================================================================================================================

simultaneously). The choices are:
=================================

meiler2006. 50 cycles with repacks every 8th
============================================

abbreviated. 5 cycles with a repack on the 4th.
===============================================

abbrev2. 6 cycles with repacks on the 3rd and 6th.
==================================================

min\_only. No cycles of docking. Skip to final minimization.
============================================================

custom 42 6. This example would be 42 cycles with repacks every 6th.
====================================================================

LIGAND X \# X is the chain ID as seen in the PDB random\_conformer \# choose a random starting conformer (can also be a DEFAULT option) mutate\_same\_name3 \# during packing sample ligand rotamers that share the same 3 letter name (can also be a DEFAULT option) translate [uniform|gaussian] 5.0 50 \# Move the ligand up to 5.0 angstroms along a uniform distribution, or center a normal

distribution at 5.0 angstroms for a random ligand translation. Try up to 50 moves to find a ligand translation that
===================================================================================================================

lands the ligand neighbor atom in an empty space. (can also be a DEFAULT option)
================================================================================

rotate uniform 360 1000 \# Try for up to 1000 cycles of 360 degree rotations to collect a certain maximum number of rotations that

lead to a *set* of ligand poses with a minimum difference of RMSDs, all of which have acceptable attractive and repulsive scores.
=================================================================================================================================

Choose randomly from among this set. If no acceptable rotations are found use the one that leads to the best attractive and
===========================================================================================================================

and repulsive scores. Also filter for user provided constraints (not yet implemented). (can also be a DEFAULT option)
=====================================================================================================================

slide\_together \# after initial placement (translate/rotate) slide ligand 1 angstrom at a time until it runs into protein, i

then back up 1 angstrom (can also be a DEFAULT option)
======================================================

tether\_ligand 0.1 \# constrain ligand centroid during docking step so small repeated movements don't lead to ligand running

away (can also be a default option)
===================================

minimize\_ligand 10 \# Let ligand rotatable bonds minimize, with harmonic restraints where one standard deviation is 10 degrees: minimize\_backbone 0.3 \# In the final minimization, let the backbone minimize with harmonic restraints on the Calphas (stddev = 0.3 A). start\_from x1,y1,z1 x2,y2,z2 a space separated list of comma separated xyz triplets to randomly select from for initial ligand placement.

This option can NOT be a default option (you wouldn't want both ligands ending up in the same place!)
=====================================================================================================

Postprocessing of Files
=======================

If you chose to use the "out:file:atom\_tree\_diffs" flag, which I recommend because it makes extra small files, then you will have to also use apps/public/ligand\_docking/extract\_atom\_tree\_diffs to get PDBs back.

I wrote a python script that makes RMSD plots from your atom\_tree\_diff files. Ask me about it.

Frequently Asked Questions
==========================

Q. Does this code work? A. Reply hazy, try again. Q. Should I use this code? A. Signs point to yes. Q. Is there a publication yet? A. My sources say no. Q. Is this code awesome? A. It is certain.

Feel free to ask a question. I will consult my majic 8-ball and get back with you.
