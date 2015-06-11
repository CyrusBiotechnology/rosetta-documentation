#RNA Applications

These applications are specifically designed to work with RNA or RNA-protein complexes. For more information on working with RNA in Rosetta, see the [[Working with RNA|RNA]] page.


###RNA Structure Prediction

* [[RNA structure prediction|rna-denovo-setup]]: Predict 3-dimensional structures of RNA from their nucleotide sequence. Read this first.
* [[RNA motif prediction|rna-denovo]]: Model RNA motifs with fragment assembly of RNA with full atom refinement (FARFAR).
* [[RNA stepwise loop enumeration|swa-rna-loop]]: Build RNA loops using *deterministic* stepwise assembly.
* [[Stepwise monte carlo|stepwise]]: Stochastic version of stepwise assembly used to generate 3D models of proteins, RNA, and protein/RNA loops, motifs, and interfaces. This application is not exclusively for RNA but is compatible. 
*  [[RNA assembly with experimental constraints|rna-assembly]] - Predict 3-dimensional structures of large RNAs with the help of experimental constraints. Note – largely deprecated by newer pipeline (documentation coming soon).
* [[CS Rosetta RNA]]: Refines and scores an RNA structure using NMR chemical shift data.

###RNA Design

* [[RNA design]]: Optimize RNA sequence for fixed backbones.  

###RNA Analysis

* [[RECCES]]: RNA free energy calculation with comprehensive sampling.

###RNA Utilities

* [[RNA tools]]: A collection of tools for PDB editing, cluster submission, "silent file" processing, and setting up rna_denovo and ERRASER jobs.
* [[ERRASER]]: Refine an RNA structure given electron density constraints.  
* [[RNA pharmacophore]]: Extract and cluster the key features present in RNA (rings, hbond donors & acceptors) from the structure of a protein-RNA complex.
* [[RNA threading|rna-thread]] - Thread a new nucleotide sequence on an existing RNA structure.  
* [[Sample around nucleobase]]: Generates tables of interaction energies between an adenosine nucleobase and a user-specified probe.

##See Also

* [[Application Documentation]]: Home page for application documentation
* [[Structure Prediction Applications]]: List of structure prediction applications
* [[Design Applications]]: List of design applications
* [[Analysis Applications]]: List of analysis applications
* [[Utilities Applications]]: List of utilities applications
* [[