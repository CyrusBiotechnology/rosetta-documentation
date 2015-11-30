#RosettaCM - Comparative Modeling with Rosetta

Metadata
========

Documentation by Sebastian Rämisch (raemisch@scripps.edu) and Jared Adolf-Bryfogle (jadolfbr@gmail.com)

Purpose
=======

This protocol functions to create a homology model given PDB files corresponding to one or more template structures.  It is used for comparative modeling of proteins.  

References
==========

[High-resolution comparative modeling with RosettaCM](http://www.sciencedirect.com/science/article/pii/S0969212613002979).  
Song Y, DiMaio F, Wang RY, Kim D, Miles C, Brunette T, Thompson J, Baker D.,
Structure. 2013 Oct 8;21(10):1735-42. doi:10.1016/j.str.2013.08.005. Epub 2013 Sep 12.

## Algorithm

At a high-level, the algorithm consists of a long Monte Carlo trajectory starting from a randomly-chosen template.  The MC trajectory employs the following moves: a) fragment insertion in unaligned regions, b) replacement of a randomly-chosen segment with that from a different template structure, and c) Cartesian-space minimization using a smooth (differentiable) version of the Rosetta [[centroid|centroid-vs-fullatom ]] energy function.  Finally, this is followed by all-atom optimization.

## Running the RosettaCM protocol

### Running the setup_CM.py script

The most straightforward way to run the protocol is through the rosetta_tools script, protein_tools/scripts/setup_rosettacm.py. As input, you need the following:
* Sequence alignment file in one of the supported formats (see below)
* Template PDB file(s)
* Target sequence fasta file

It is run with the following:

    Rosetta/tools/protein_tools/scripts/setup_RosettaCM.py [-h] --fasta FASTA
                          [--alignment ALIGNMENT]
                          [--alignment_format ALIGNMENT_FORMAT]
                          [--templates [TEMPLATES [TEMPLATES ...]]]
                          [--rosetta_bin ROSETTA_BIN] [--build BUILD]
                          [--platform PLATFORM] [--compiler COMPILER]
                          [--compiling_mode COMPILING_MODE]
                          [--setup_script SETUP_SCRIPT] [-j J] [--run]
                          [--keep_files] [--run_dir RUN_DIR] [--equal_weight]
                          [--use_dna] [--verbose]

With the following important options:

**--alignment ALIGNMENT --alignment_format ALIGNMENT_FORMAT**

The alignment file must be passed as input, and the format must be specified.  The following formats are valid: _grishin_, _modeller_, _vie_, _hhsearch_, _clustalw_, _fasta_.

**--templates [TEMPLATES [TEMPLATES ...]]]**

A list of PDBs of template structures.  The alignment file "tags" must match the names of the template PDB files.

**--rosetta_bin ROSETTA_BIN**

The path to the Rosetta _bin_ directory.

After setting up a job, cd to the _rosetta_cm_ directory, and run (pointing to installed Rosetta location):

    Rosetta/main/source/bin/rosetta_scripts.linuxgccrelease @flags -database Rosetta/main/database -nstruct 10

### Manually setting up a RosettaCM job

Alternately, it may be desirable to manually set up a RosettaCM job.  If one wishes to use nonstandard features (e.g. electron density data or user-specified constraints), this is recommended.  

Given a fasta file sequence of the protein to be modeled, a multiple sequence alignment, and PDBs of template structures, the first step of the protocol is to thread the same sequence onto one or multiple templates.  These templates can be homologous proteins or the same proteins from multiple crystal structures.  The second step is to use the [[HybridizeMover]] through [[RosettaScripts]] to create a single model from the template(s).

### Step 1: Created a threaded model

First we need to produce template pdb files for Rosetta by threading the target sequence onto one or several template protein structures.  As input, you need the following:
* Sequence alignment file in Grishin format
* Template PDB file(s)
* Target sequence fasta file

[[An overview of the Grishin file format|Grishan-format-alignment]]

The threaded model is then created with the following command:

    Rosetta/main/source/bin/partial_thread.linuxgccrelease \
         -database Rosetta/main/database \
         -in:file:fasta target.fasta \
         -in:file:alignment alignment.aln \
         -in:file:template_pdb 1k3d.pdb 1y12.pdb \
         -ignore_unrecognized_res

This application should only take a few seconds.

### Step 2: Run the Hybridize mover

This is the step where modeling is performed.

As input, you need the following:
* RosettaScripts xml file
* target sequence fasta file
* threaded template pdb files from Step 1

A simple XML for performing modelling is listed below (see [[RosettaScripts]] for a guide to the XML syntax in Rosetta).  More advanced options are specified on the [[HybridizeMover]] page.

    <ROSETTASCRIPTS>
    <SCOREFXNS>
        <stage1 weights=score3 symmetric=0>
            <Reweight scoretype=atom_pair_constraint weight=0.5/>
        </stage1>
        <stage2 weights=score4_smooth_cart symmetric=0>
            <Reweight scoretype=atom_pair_constraint weight=0.5/>
        </stage2>
        <fullatom weights=talaris2013_cart symmetric=0>
            <Reweight scoretype=atom_pair_constraint weight=0.5/>
        </fullatom>
    </SCOREFXNS>
    <MOVERS>
        <Hybridize name=hybridize stage1_scorefxn=stage1 stage2_scorefxn=stage2 fa_scorefxn=fullatom batch=1 stage1_increase_cycles=1.0 stage2_increase_cycles=1.0>
            <Template pdb="1k3d_templ.pdb" cst_file="AUTO" weight=1.000 />
            <Template pdb="1y12_templ.pdb" cst_file="AUTO" weight=1.000 />
        </Hybridize>
    <FastRelax name="relax" scorefxn=talaris2013  />
    </MOVERS>
    <PROTOCOLS>
        <Add mover=hybridize/>
        <Add mover=relax/>
    </PROTOCOLS>
    <OUTPUT scorefxn=talaris2013 />
    </ROSETTASCRIPTS>

Save the above XML file as **hybridize.xml**.  Then, RosettaCM is run using the following command:

    Rosetta/main/source/bin/rosetta_scripts.linuxclangrelease \
         -database Rosetta/main/database \
         -in:file:fasta target.fasta \
         -parser:protocol **hybridize.xml** \
         -default_max_cycles 200 \
         -dualspace

### Post Processing

See [[Analyzing Results]]: Tips for analyzing results generated using Rosetta

### Other tips

** How can I model with multiple chains? **

In the input fasta file, separate sequences of individual chains with a '/' character.

** How can I model with ligands/nucleic acids? **

Add the tag **use_hetatm=1** to the Hybridize mover line.  Then, manually add the aligned ligand(s), with residue numbers starting **one past the length of the fasta file**.  For nucleic acids, this is all that is needed.

For ligands, the params files must be generated using the [[molfile_to_params|preparing-ligands]] script, with a few non-standard options.  Given a mol2 file of the ligand in question, XXX.mol2, run:

    python Rosetta/main/source/src/python/apps/public/molfile_to_params.py \
       --keep-names \
       --clobber \
       --extra_torsion_output \
       --centroid XXX.mol2 \
       -p XXX -n XXX

And the following flags must be additionally provided:

    --extra_res_cen XXX.cen.params
    --extra_res_fa  XXX.fa.params
    --extra_improper_file XXX.tors


** How can I model with symmetry? **



## See Also

* [[RosettaScripts]] documentation
* [[HybridizeMover]]: The Hybridize Mover used by RosettaCM
* [[Structure prediction applications]]: A list of other applications to be used for structure prediction
* [[Application Documentation]]: List of Rosetta applications
* [[Running Rosetta with options]]: Instructions for running Rosetta executables.
* [[Comparing structures]]: Essay on comparing structures
* [[Analyzing Results]]: Tips for analyzing results generated using Rosetta
* [[Solving a Biological Problem]]: Guide to approaching biological problems using Rosetta
* [[Commands collection]]: A list of example command lines for running Rosetta executable files