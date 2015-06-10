#Rosetta Basics

###If you are new to Rosetta, start [[here|Getting-Started]].

###If you are trying to find the right protocol to use for your biological application, look [[here|Solving-a-Biological-Problem]].

####Controlling Rosetta Execution
- [[Running Rosetta|running-rosetta-with-options]]
- [[Running Rosetta Parallel via MPI | running-rosetta-with-options#mpi ]]
- [[Graphics output and GUIs | graphics-and-guis]]
    * [[PyMOL]]
    * [[PyRosetta Toolkit GUI]]
    * Fold-It
- [[Scripting Rosetta|Scripting-Documentation]]
    * [[RosettaScripts]]
    * [[PyRosetta]]
    * [[Topology Broker|BrokeredEnvironment]]
- [[Analyzing Results]]

####[[Fundamental Rosetta Concepts|Rosetta-overview]]

- [[Brief history of Rosetta|RosettaTimeline]]

- [[Glossary of terms|Glossary]]
    - [[Longer form descriptions|RosettaEncyclopedia]]

- [[Units in Rosetta]]

- [[Scoring|scoring-explained]]

- [[Scorefunctions and Score types | Score Types]] - Description of the default Rosetta Scorefunction and common score types.
    *  [[MM Std Scorefunction | NC-scorefunction-info#MM-Standard-Scorefunction]]
    *  [[Orbitals Scorefunction | NC-scorefunction-info#Partial-Covalent-Interactions-Energy-Function-(Orbitals)]]
    *  [[Additional score types | score-types-additional]]
    *  [[Hydrogen bonding score term|hbonds]]

- [[Symmetry]]

- [[Minimization | Minimization Overview]] - Backbone and/or side chain degrees of freedom

- [[Comparing Structures]]

- [[I want to do _x_. How do I do _x_? | I-want-to-do-x ]] 

- Advanced Topics
    * [Rosetta3 Architecture](http://www.ncbi.nlm.nih.gov/pubmed/21187238)
    * [[Foldtree Overview]]
    * [[AtomTree Overview]]
    * [[Loop modeling styles and algorithms|loopmodel-algorithms]]

####Preparing structures to be used by Rosetta
* [[Preparing typical PDB files|preparing-structures]]
* [[Preparing PDB files for non-peptide polymers]]
* [[Preparing ligands]]

####[[Common/Useful Rosetta Options|options-overview]]
- [[Input options]]
- [[Output options]]
- [[Relational Database options | Database-options]]
- [[Run options]]
- [[Score options]]
- [[Packing options]]
- [[Renamed and Deprecated Options]]

####Common File Formats
- [[Fasta file]] - Input protein sequences
- [[Silent file]] - Rosetta-specific compact output representation
- [[Resfiles]] - Which residue sidechains can move and mutate
- [[Movemap file]] - Which sidechains and backbones can move
- [[Constraint file]] - Add energy restraints to scoring
- [[Fragment file]] - Database of backbone fragment conformations
- [[Loops file]] - Which regions of the protein should be rebuilt
- [[Residue Params file]] - Residue chemical information
- [[Symmetry file|Symmetry#Symmetry-definitions]] - Dealing with symmetric proteins.

####Protocol-specific file formats
- [[Matcher (Enzdes) Constraint Files|match-cstfile-format]] - A constraint file specialized for protein-ligand interactions
- [[Chemical shift file]] - NMR chemical shifts
- [[Bin transition probabilities file]] - Probabilities of transitioning from one mainchain torsion bin to another, used by some sampling schemes

####[[Working with Non-Protein Residues and Molecules|non-protein-residues]]
- General Guidance:
    * [[General Control | Ignore Unrecognized]]
    * [[Preparing PDB files for non-peptide polymers]]
    * [[How to turn on residue types that are off by default]]
    * [[Scorefunction and Scoreterm Info | NC-scorefunction-info]]
- [[DNA]]
- [[RNA]]
- [[Ligands]]
- [[Water]]
- [[Metals]]
- [[Carbohydrates]]
- [[Mineral Surfaces]]
- [[Noncanonical Amino Acids]]
    *  [[D-Amino Acids]]
    *  [[Alpha-Amino Acids with Nonstandard Side-Chains]]
    *  [[Beta-Amino Acids]]
- [[Noncanonical backbones]]

####Misc
- [[Database support]] - Relational database support in Rosetta.
    *  [[Database input/output|Database-IO]] - Platform-specific information on Rosetta database support.
    *  [[Sqlite3-interface]] - More information on the sqlite database interface.
- [[Full options list|full-options-list]] - A (mostly) complete list of availible Rosetta options.
- [[ Rosetta Job Distribution Discussion | JD2]]
- [[Analyzing results]]

####Help
- [RosettaCommons Forums](http://rosettacommons.org/forum)
- [RosettaCommons Bug Tracker](http://bugs.rosettacommons.org)


##See Also

* [[Getting Started]]: A page for people new to Rosetta. New users start here.
* [[Glossary]]: Brief definitions of Rosetta terms
* [[RosettaEncyclopedia]]: Detailed descriptions of additional concepts in Rosetta.
* [[Rosetta overview]]: Overview of major concepts in Rosetta
* [[Build Documentation]]: Information on setting up Rosetta
* [[Rosetta on different scales]]: Guidelines for how to scale your Rosetta runs
* [[TACC]]: Information for running Rosetta on the TACC/Stampede cluster.
* [[Solving a Biological Problem]]: Guide to approaching biological problems using Rosetta
* [[I want to do x]]: Guide to making specific structural pertubations using RosettaScripts
* [[Application Documentation]]: Links to documentation for a variety of Rosetta applications
* [[Commands collection]]: A list of example command lines for running Rosetta executable files
* [[Rosetta Servers]]: Web-based servers for Rosetta applications
* [[Scripting Documentation]]: Scripting interfaces to Rosetta
* [[Resources for learning biophysics and computational modeling]]
* [[Analyzing Results]]: Tips for analyzing results generated using Rosetta
* [[Comparing structures]]: Essay on comparing structures
* [[Using the ResourceManager|ResourceManager]]
* [[Non-protein Residues]]: Information on running Rosetta with non-protein residues and ligands