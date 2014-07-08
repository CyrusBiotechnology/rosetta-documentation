#RosettaScripts Documentation

[[_TOC_]]

"Skeleton" XML format
---------------------

Copy, paste, fill in, and enjoy

    <ROSETTASCRIPTS>
            <SCOREFXNS>
            </SCOREFXNS>
            <TASKOPERATIONS>
            </TASKOPERATIONS>
            <FILTERS>
            </FILTERS>
            <MOVERS>
            </MOVERS>
            <APPLY_TO_POSE>
            </APPLY_TO_POSE>
            <PROTOCOLS>
            </PROTOCOLS>
            <OUTPUT />
    </ROSETTASCRIPTS>

Anything outside of the \< \> notation is ignored and can be used to comment the xml file

General Description and Purpose
-------------------------------

RosettaScripts is meant to provide an xml-scriptable interface for conducting all of the tasks that interface design developers produce. With such a scriptable interface, it is hoped, it will be possible for non-programmers to 'mix-and-match' different design strategies and apply them to their own needs. It is also hoped that through a common interface, code-sharing between different people will be smoother. Note that at this point, the only movers and filters that are implemented in this application are the ones described below. More will be made available in future releases. At this point these include protocols from the protein-interface design, protein docking, enzyme-design, ligand-docking and -design, monomer design, and DNA-interface design groups. General movers for loop modeling and structure relaxation are also available.

A paper describing RosettaScripts is available at: Fleishman et al. (2011) PLoS 1 6:e20161
(http://www.plosone.org/article/fetchObject.action?uri=info%3Adoi%2F10.1371%2Fjournal.pone.0020161&representation=PDF)

At the most abstract level, all of the computations that are needed in interface design fall into two categories: Movers and Filters. Movers change the conformation of the complex by acting on it, e.g., docking/design/minimization, and filters decide whether a given conformation should go on to the subsequent steps. Filters are meant to reduce the amount of computation that is conducted on conformations that show no promise. Then, a RosettaScript is merely a sequence of movers and filters.

The implementation for this behaviour is done by the following components:

-   **ParsedProtocol** , **Filter** , and **Mover**
     ParsedProtocol maintains a vector of pairs of movers and their associated filters. By using the TrueFilter or the NullMover, filters and movers can be essentially decoupled by any protocol. The setup of having pairs of movers and filters is used simply because in most contexts filters will be conceptually associated with a mover and vice versa.
-   **DockDesignParser.cc** This function parses an xml file and populates DockDesignMover with pairs of Movers and Filters. All of the movers and filters that are supported should also be defined in this function.


Example XML file
----------------

The following simple example will compute ala-scanning values for each residue in the protein interface:

    <ROSETTASCRIPTS>
            <SCOREFXNS>
                    <interface weights=interface/>
            </SCOREFXNS>
            <FILTERS>
                    <AlaScan name=scan partner1=1 partner2=1 scorefxn=interface interface_distance_cutoff=10.0 repeats=5/>
                    <Ddg name=ddg confidence=0/>
                    <Sasa name=sasa confidence=0/>
            </FILTERS>
            <MOVERS>
                    <Docking name=dock fullatom=1 local_refine=1 score_high=soft_rep/>
            </MOVERS>
            <APPLY_TO_POSE>
            </APPLY_TO_POSE>
            <PROTOCOLS>
                    <Add mover_name=dock filter_name=scan/>
                    <Add filter_name=ddg/>
                    <Add filter_name=sasa/>
            </PROTOCOLS>
            <OUTPUT scorefxn=interface />
    </ROSETTASCRIPTS>

Rosetta will carry out the order of operations specified in PROTOCOLS, starting with docking (in this case this is all-atom docking using the soft\_rep weights). It will then apply alanine scanning, repeated 5 times for better convergence, for every residue on both sides of the interface computing the binding energies using the interface weight set (counting mostly attractive energies). The binding energy (ddg) and surface area (sasa) will also be computed. All of the values will be output in a .report file. Notice that since ddg and sasa are assigned confidence=0, they are not used here as filters that can terminate a trajectory per se, but rather for reporting the values for the complex. An important point is that filters never change the sequence or conformation of the structure, so the ddg and sasa values are reported for the input structure following docking, with the alanine-scanning results ignored.

The movers do change the pose, and the output file will be the result of sequentially applying the movers in the protocols section. The standard scores of the output (either in the pdb, silent or score file) will be from the commandline-specified scorefunction, unless the OUTPUT tag is specified, in which case the corresponding score function from the SCOREFXNS block will be used.

Additional example xml scripts, including examples for docking, protein interface design, and prepacking a protein complex, amongst others, can be found in the rosetta/rosetta\_demos/public/rosetta\_scripts/ directory. (Or online at [https://svn.rosettacommons.org/trac/browser/trunk/rosetta/demos/public/rosetta\_scripts](https://svn.rosettacommons.org/trac/browser/trunk/rosetta/rosetta_demos/public/rosetta_scripts) for those with svn access.)

Example commandline
-------------------

The following command line would run the above protocol, given that the protocol file name is ala\_scan.xml

    bin/rosetta_scripts.linuxgccrelease -s < INPUT PDB FILE NAME > -use_input_sc -nstruct 20 -jd2:ntrials 2 -database ~/minirosetta_database/ 
    -ex1 -ex2 -parser:protocol ala_scan.xml -parser:view

The ntrials flag specifies how many trajectories to start per nstruct. In this case, each of 20 trajectories would make two attempts at outputting a structure. If no ntrials is specified, a default value of 1 is assumed.

The parser:view flag may be used with rosetta executables that have been compiled using the extras=graphics switch in the following way (from the Rosetta root directory):

    scons mode=release -j3 bin extras=graphics

When running with -parser:view a graphical viewer will open that shows many of the steps in a trajectory. This is extremely useful for making sure that sampling is following the intended trajecotry.

Input and Output Files
----------------------

Running a typical protocol requires input of an xml file and a starting pdb file, as in the example commandline above. Alternatively, to run the protocol on many structures, save a simple list of the pdb files to be used and replace the flag -s \<INPUT PDB FILE NAME\> in the commandline with -l \<INPUT LIST FILE NAME\>. Some movers and filters require specific input files (for example, a pdb file containing stub residues for hot-spot residue placement for PlaceStub or PlaceSimultaneously movers), and in such cases the required input file/s are described below and are generally called via the xml script.

During a run, if any defined filters are not satisfied then the trajectory will be killed and no output files returned, and Rosetta will continue on to the next ntrial (or if all ntrials have been attempted and failed, Rosetta will continue with any remaining nstructs as defined in the commandline). For a successful run in which all filters are satisfied, the output will include a pdb file and a score.sc file. The output pdb name is identical to the input pdb file name with a suffix denoting the nstruct number.

The score.sc file tabulates the energy terms and filter values for every successful nstruct. The pdb file ends with an energy table for all residues and lists the values of any filters in the same order they are used in the xml protocol. By default, the scorefunction used in the score file and the PDB energy table is "commandline" (the score function specified by commandline options). To change this to a different scorefunction, see the [OUTPUT tag](#OUTPUT) .

Using an IntelliSense editor to help with generating RosettaScripts
-------------------------------------------------------------------

An xml-schema was generated for us by Avner Aharoni (Microsoft) using Visual Studio. Using this schema in a compatible editor provides a specific editor for writing RosettaScripts, complete with word completion, grammatical error warnings and help with options. We are currently aware of two editors that are fully compatible with this schema

### Editing RosettaScripts in emacs

The nXML emacs add-on is compatible with the RosettaScripts.rnc schema (found in src/apps/public/rosetta\_scripts/RosettaScripts.rnc).

1.  Download nXML from [http://www.thaiopensource.com/nxml-mode/](http://www.thaiopensource.com/nxml-mode/)
2.  Read the nXML portion of the emacsWiki at [http://www.emacswiki.org/cgi-bin/wiki/NxmlMode](http://www.emacswiki.org/cgi-bin/wiki/NxmlMode)
3.  Load the RosettaScripts.rnc file into emacs+nXML
4.  Load your protocol
5.  Have fun!

### Editing RosettaScripts in VisualStudio

MS-Windows users can download Visual Studio Express (free of charge) which provides an xml editor that is compatible with the RosettaScripts.xsd schema (found in src/apps/public/rosetta\_scripts/RosettaScripts.xsd). The following instructions were provided by Avner Aharoni:

1.  Download VB express from [http://www.microsoft.com/express/download/](http://www.microsoft.com/express/download/)
2.  Save the schema in the following folder C:\\Program Files\\Microsoft Visual Studio 9.0\\Xml\\Schemas
3.  Create empty xml file on disk (a file with the .xml suffix)
4.  Open it in the Visual Studio Express, go to its properties (view =-\> property window F4) and set the RosettaScripts.xsd schema for use.

RosettaScripts Conventions
--------------------------

### General Comments

This file lists the Movers, Filters, their defaults, meanings and uses as recognized by RosettaScripts. It is written in an xml format and using many free viewers (e.g., vi) will highlight key xml notations, so long as the file has extension .xml

Whenever an xml statement is shown, the following convention will be used:

    <...> to define a branch statement (a statement that has more leaves)
    <.../> a leaf statement.
    "" defines input expected from the user with ampersand (&) defining the type that is expected (string, float, etc.)
    () defines the default value that the parser will use if that is not provided by the protocol.

### Specifying Residues

There are two residue numbering conventions that are used in Rosetta - "pose numbering" and "pdb numbering". Pose numbering assigns a value of 1 to the first residue of the first chain, and then sequentially numbers from there, ignoring the start of new chains and missing residues. Pdb numbering uses the chain/residue/insertion code designation that is present in the input pdb file. Generally, whenever a residue identifier is given with a chain, it's PDB numbered, and without a chain is the pose number.

For example, if you have a PDB file which has two chains, with residues 12-62 for chain A and residues 5-20 and 32 to 70 for chain B, the pose number for pdb residue 12 of chain A would be 1, and pdb residue 62 of chain A would be pose numbered 51. Pdb chain B residue 5 would be pose numbered 52, and chain B residue 32 would be pose number 68.

In many of the RosettaScripts tags that take a residue identifier, there is a joint option to specify it in either pose numbering or PDB numbering, notated as something like res\_num/pdb\_num. For tags which have this option, you can specify *either* res\_num= or pdb\_num=, but not both. The res\_num option takes a pose numbered residue designation, and the pdb\_num option takes a pdb numberd designation in the form of "42.A" or "42A" where A specifies the chain and 42 is the pdb residue number. At this time it is not possible to specify an insertion code with the pdb\_num option.

Care must be exercised when using PDB numbering with protocols that change the length of the pose. Insertion of residues can invalidate the PDB information associated with the pose, resulting in errors when the pdb numbering is decoded. Additionally, some RosettaScripts objects will convert the pdb number to a pose number based on the input structure numbering, resulting in potential mis-alignment if residues are added/deleted.

Options Available in the XML Protocol File
------------------------------------------

### Variable Substitution

Occasionally it is desirable to run a series of different runs with slightly different parameters. Instead of creating a number of slightly different XML files, one can use script variables to do the job.

If the -parser:script\_vars option is set on the command line, every time a string like "%%variable\_name%%", is encountered in the XML file, it is replaced with the corresponding value from the command line.

For example, a line in the XML like
```
<AlaScan name=scan partner1=1 partner2=1 scorefxn=interface interface_distance_cutoff=%%cutoff%% repeats=%%repeat%%/>
```
can be turned into
```
<AlaScan name=scan partner1=1 partner2=1 scorefxn=interface interface_distance_cutoff=10.0 repeats=5/>
```
with the command line option
```
   -parser:script_vars repeat=5 cutoff=10.0
```

These values can be changed at will for different runs, for example:
```
   -parser:script_vars repeat=5 cutoff=15.0
   -parser:script_vars repeat=2 cutoff=10.0
   -parser:script_vars repeat=1 cutoff=9.0
```

Multiple instances of the "%%var%%" string will all be substituted, as well as in any [[subroutine|Movers-RosettaScripts#Subroutine]] XML files. Note that while currently script\_vars are implemented as pure macro text substitution, this may change in the future, and any use aside from substituting tag values may not work. Particularly, any use of script variables to change the parsing structure of the XML file itself is explicitly \*not\* supported, and you have a devious mind for even considering it.


revert\_design\_to\_wt application
----------------------------------

This application is not yet strictly speaking part of RosettaScripts but is strongly related to the design purposes of RS. Work in ongoing to supersede this application with a more useful RS implementation. In the meantime here is an explanation.

The application was described in:

Fleishman et al. Science 332: 816. Here is the relevant excerpt:

For each design that passed the abovementioned filters, the contribution of each amino-acid substitution at the interface is assessed by singly reverting residues to their wildtype identities and testing the effects of the reversion on the computed binding energy. If the difference in binding energy between the designed residue and the reverted one is less than 0.5R.e.u. in favor of the design, then the position is reverted to its wildtype identity. A Rosetta application to compute these values is available in the Rosetta release and is called revert\_design\_to\_native. A report of all residue changes was produced and each suggestion was reviewed manually.

Usage: revert\_design\_to\_native -revert\_app:wt \<Native protein PDB\> -revert\_app:design \<Designed PDB\> -ex1 -ex2 -use\_input\_sc -database \<\> \> log

Keep the log. At its end you'll find a summary of all mutations attempted and their significance for binding energy.


### Predefined Movers

The following are defined internally in the parser, and the protocol can use them without defining them explicitly.

#### NullMover

Has an empty apply. Will be used as the default mover in \<PROTOCOLS\> if no mover\_name is specified. Can be explicitly specified, with the name "null".

### Predefined Filters

#### TrueFilter

Always returns true. Useful for defining a mover without using a filter. Can be explicitly specified with the name "true\_filter".

#### FalseFilter

Always returns false. Can be explicitly specified with the name "false\_filter".

### Predefined Scorefunctions

-   score12: The default all-atom scorefunction used by rosetta ab-initio and design
-   score\_docking: high resolution docking scorefxn (standard+docking\_patch)
-   score\_docking\_low: low resolution docking scorefxn (interchain\_cen)
-   soft\_rep: soft\_rep\_design weights.
-   score4L: low resolution scorefunction used for loop remodeling (chainbreak weight on)
-   score\_empty: all weights = 0.
-   commandline: the scorefunction specified by the commandline options (Note: not recommended for general use.)

SCOREFUNCTIONS
--------------

The SCOREFXNS section defines scorefunctions that will be used in Filters and Movers. This can be used to define any of the scores defined in the rosetta\_database

```
<"scorefxn_name" weights=("empty" &string) patch=(&string)>
    <Reweight scoretype=(&string) weight=(&Real)/>
    <Set (option name)=(value)/>
</"scorefxn_name">
```

where scorefxn\_name will be used in the Movers and Filters sections to use the scorefunction. The name should therefore be unique and not repeat the predefined score names. One or more Reweight tag is optional and allows you to change/add the weight for a given scoretype. The Set tag is optional and allows you to change certain scorefunction options. For example:

```
<scorefxn1 weights=fldsgn_cen>
    <Reweight scoretype="env" weight=1.0/>
</scorefxn1>
```

#### Scorefunction Options

One or more option can be specified per Set tag:

-   exclude\_protein\_protein\_hack\_elec=(&bool) - Don't compute hack\_elec energies for protein-protein interactions (equivalent to the -ligand::old\_estat command line option for ligand\_dock/enzyme\_design)
-   exclude\_DNA\_DNA=(&bool)
-   exclude\_DNA\_DNA\_hbond=(&bool)
-   use\_hb\_env\_dep\_DNA=(&bool)
-   use\_hb\_env\_dep=(&bool)
-   smooth\_hb\_env\_dep=(&bool)
-   decompose\_bb\_hb\_into\_pair\_energies=(&bool) - Store backbone hydrogen bonds in the energy graph on a per-residue basis (this doubles the number of calculations, so is off by default)

### Global Scorefunction modifiers

The apply\_to\_pose section may set up constraints, in which case it becomes necessary to set the weights in all of the scorefunctions that are defined. The default weights for all the scorefunctions are defined globally in the apply\_to\_pose section, but each scorefunction definition may change this weight. For example, to set the HotspotConstraint (backbone\_stub\_constraint) value to 6.0

```
<my_spiffy_score weights="soft_rep_design" patch="dock" hs_hash=6.0/>
```

The following modifiers are recognized:

#### HotspotConstraints modifications

```
hs_hash=(the value set by apply_to_pose for hotspot_hash &float)
```

#### Symmetric Scorefunctions

To properly score symmetric poses, they must be scored with a symmetric score function. To declare a scorefunction symmetric, simply add the tag:

```
symmetric=1
```

For example, symmetric score12:

```
<score12_symm weights="score12_full" symmetric=1/>
```

OUTPUT
------

The top-level OUTPUT tag allows for setting certain output options

### scorefxn

\<OUTPUT scorefxn=(name &string) /\>

The scorefunction specified by the OUTPUT tag will be used to score the pose prior to output. It is the score function which will be represented in the scores reported in the scorefile and the output PDB of the run.

If not specified, the "commandline" scorefunction (the scorefunction specified by commandline options) is used.

TASKOPERATIONS
--------------

TaskOperations are used by movers to tell the "packer" which residues/rotamers to use in reorganizing/mutating sidechains. When used by certain Movers, the TaskOperations control what happens during packing, usually by restriction "masks". TaskOperations can also be used by movers to specify sets of residues to act upon in non-packer contexts.

### Available TaskOperations

See [[TaskOperations (RosettaScripts)|TaskOperations-RosettaScripts]]

APPLY\_TO\_POSE
---------------

This is a section that is used to change the input structure. The most likely use for this is to define constraints to a structure that has been read from disk.

#### Sequence-profile Constraints

Sets constraints on the sequence of the pose that can be based on a sequence alignment or an amino-acid transition matrix.

```
<profile weight=(0.25 &Real) file_name=(<input file name >.cst &string)/>
```

sets residue\_type type constraints to the pose based on a sequence profile. file\_name defaults to the input file name with the suffix changed to ".cst". So, a file called xxxx\_yyyy.25.jjj.pdb would imply xxxx\_yyyy.cst. To generate sequence-profile constraint files with these defaults use DockScripts/seq\_prof/seq\_prof\_wrapper.sh

#### SetupHotspotConstraints (formerly hashing\_constraints)

```
<SetupHotspotConstraints stubfile=(stubs.pdb &string) redesign_chain=(2 &integer) cb_force=(0.5 &float) worst_allowed_stub_bonus=(0.0 &float) apply_stub_self_energies=(1 &bool) apply_stub_bump_cutoff=(10.0 &float) pick_best_energy_constraint=(1 &bool) backbone_stub_constraint_weight=(1.0 &Real)>
  <HotspotFiles>
    <Add file_name=(&string) nickname=(&string) stub_num=(&integer)/>
      ...
  </HotspotFiles>
</SetupHotspotConstraints>
```

-   stubfile: a pdb file containing the hot-spot residues
-   redesign\_chain: which is the host\_chain for design. Anything other than chain 2 has not been tested.
-   cb\_force: the Hooke's law spring constant to use in setting up the harmonic restraints on the Cb atoms.
-   worst\_allowed\_stub\_bonus: triage stubs that have energies higher than this cutoff.
-   apply\_stub\_self\_energies: evaluate the stub's energy in the context of the pose.
-   pick\_best\_energy\_constraint: when more than one restraint is applied to a particular residue, only sum the one that makes the highest contribution.
-   backbone\_stub\_constraint\_weight: the weight on the score-term in evaluating the constraint. Notice that this weight can be overridden in the individual scorefxns.
-   HotspotFiles: You can specify a set of hotspot files to be read individually. Each one is associated with a nickname for use in the placement movers/filters. You can set to keep in memory only a subset of the read stubs using stub\_num. If stubfile in the main branch is not specified, only the stubs in the leaves will be used.

MOVERS
------

Each mover definition has the following structure

```
<"mover_name" name="&string" .../>
```

where "mover\_name" belongs to a predefined set of possible movers that the parser recognizes and are listed below, name is a unique identifier for this mover definition and then any number of parameters that the mover needs to be defined.

### Available Movers

See [[Movers (RosettaScripts)|Movers-RosettaScripts]]

FILTERS
-------

Each filter definition has the following format:

```
<"filter_name" name="&string" ... confidence=(1 &Real)/>
```

where "filter\_name" belongs to a predefined set of possible filters that the parser recognizes and are listed below, name is a unique identifier for this mover definition and then any number of parameters that the filter needs to be defined.

If confidence is 1.0, then the filter is evaluated as in predicate logic (T/F). If the value is less than 0.999, then the filter is evaluated as fuzzy, so that it will return True in (1.0 - confidence) fraction of times it is probed. This should be useful for cases in which experimental data are ambiguous or uncertain.

### Available Filters

See [[Filters (RosettaScripts)|Filters-RosettaScripts]]

LOOP\_DEFINITIONS
-----------------

A loop definition is used by movers that preform loop modeling. A loops definition consists of a set of loops where each loop describes the start residue, the stop residue, and optionally specifies a fold-tree cutpoint, a skip-rate and whether the loop should be extended.

### Available Loop Definers

#### Loops

Explicitly specify a set of loops:

        <Loops name=(&string)>
            <loop start=(&Size) stop=(&Size) cut=(0 &Size) skip_rate(0.0 &Real) extended=(0 &Boolean)/>
            ....
        </Loops>

#### LoopsFile

Load a loops specification from a loops file

        <LoopsFile name=(&string) filename=(&string)/>

#### LoopsDatabase

Load a set of loops from a database with a table having the following schema, defining a single loop per row

        CREATE TABLE loops (
            tag TEXT,
            start INTEGER,
            stop INTEGER,
            cut INTEGER,
            skip_rate REAL,
            extended BOOLEAN);

The LoopsDatabase builder return all loops associated with the job distributor input tag

Note: you can specify a different table using the 'database\_table' field

Note: if you would like to query the database for loops differently, you can either pre-query the table and store it, or extend, subclass, or create a different LoopsDefiner class.

Note: If the database configuration information is not specified, the relevant options in the option system are used.

        <LoopsDatabase name=(&string) database_mode=sqlite3 database_name=(&string) database_separate_db_per_mpi_process=(0 &Boolean) database_read_only=(0 &string) database_table=('loops' &string)/>
        <LoopsDatabase name=(&string) database_mode=['mysql', 'postgres'] database_name=(&string) database_host=(-mysql:host &string) database_user(-mysql:user &string) database_password=(-mysql:password &string) database_port=(-mysql:port &string) database_table=('loops' &string)/> 

LIGAND\_AREAS
-------------

```
<[name_of_this_ligand_area] chain="&string" cutoff=(float) add_nbr_radius=[true|false] all_atom_mode=[true|false] minimize_ligand=[float] Calpha_restraints=[float] high_res_angstroms=[float] high_res_degrees=[float] tether_ligand=[float] />
```

LIGAND\_AREAS describe parameters specific to each ligand, useful for multiple ligand docking studies. "cutoff" is the distance in angstroms from the ligand an amino-acid's C-beta atom can be and that residue still be part of the interface. "all\_atom\_mode" can be true or false. If all atom mode is true than if any ligand atom is within "cutoff" of the C-beta atom, that residue becomes part of the interface. If false, only the ligand neighbor atom is used to decide if the protein residue is part of the interface. "add\_nbr\_radius" increases the cutoff by the size of the ligand neighbor atom's radius specified in the ligand .params file. This size can be adjusted to represent the size of the ligand, without entering all\_atom\_mode. Thus all\_atom\_mode should not be used with add\_nbr\_radius.

Ligand minimization can be turned on by specifying a minimize\_ligand value greater than 0. This value represents the size of one standard deviation of ligand torsion angle rotation (in degrees). By setting Calpha\_restraints greater than 0, backbone flexibility is enabled. This value represents the size of one standard deviation of Calpha movement, in angstroms.

During high resolution docking, small amounts of ligand translation and rotation are coupled with cycles of rotamer trials or repacking. These values can be controlled by the 'high\_res\_angstrom' and 'high\_res\_degrees' values respectively. A tether\_ligand value (in angstroms) will constrain the ligand so that multiple cycles of small translations don't add up to a large translation.

INTERFACE\_BUILDERS
-------------------

```
<[name_of_this_interface_builder] ligand_areas=(comma separated list of predefined ligand_areas) extension_window=(int)/>
```

An interface builder describes how to choose residues that will be part of a protein-ligand interface. These residues are chosen for repacking, rotamer trials, and backbone minimization during ligand docking. The initial XML parameter is the name of the interface\_builder (for later reference). "ligand\_areas" is a comma separated list of strings matching LIGAND\_AREAS described previously. Finally 'extension\_window' surrounds interface residues with residues labeled as 'near interface'. This is important for backbone minimization, because a residue's backbone can't really move unless it is part of a stretch of residues that are flexible.

MOVEMAP\_BUILDERS
-----------------

```
<[name_of_this_movemap_builder] sc_interface=(string) bb_interface=(string) minimize_water=[true|false]/>
```

A movemap builder constructs a movemap. A movemap is a 2xN table of true/false values, where N is the number of residues your protein/ligand complex. The two columns are for backbone and side-chain movements. The MovemapBuilder combines previously constructed backbone and side-chain interfaces (see previous section). Leave out bb\_interface if you do not want to minimize the backbone. The minimize\_water option is a global option. If you are docking water molecules as separate ligands (multi-ligand docking) these should be described through LIGAND\_AREAS and INTERFACE\_BUILDERS.

SCORINGGRIDS
------------

```
<SCORINGGRIDS ligand_chain=(string) width=(real)>
    <[name_of_this_scoring_grid] grid_type=(string) weight=(real)/>
</SCORINGGRIDS>
```

The SCORINGGRIDS block is used to define ligand scoring grids (currently used only by the Transform mover). The grids will initially be created centered at the ligand chain specified, and will be cubical with the specified width in angstroms. Rosetta automatically decides whether or not a grid needs to be recalculated. Grids are recalculated if any non-ligand atoms change position. The weight specified for each grid is multiplied by the ligand score for that grid. The following grid\_types can currently be specified:

-   ClassicGrid: The scoring grids originally used in The Davis, 2009 implementation of RosettaLigand
-   VdwGrid: A knowledge based potential derived grid approximating shape complementarity
-   HbdGrid: A knowledge based potential derived grid approximating protein hydrogen bond donor interactions
-   HbaGrid: A knowledge based potential derived grid approximating protein hydrogen bond acceptor interactions
