#Score Commands

Metadata
========

This document was edited 3/14/2014 by Jared Adolf-Bryfogle. This application in Rosetta3 was created and documented by Mike Tyka,et al.

Purpose and Algorithm
=====================

This application simply rescores PDBs and silent files. It also can be used to extract PDBs from silent files, assemble PDBs into silent files.

Command Line Options
====================

```
** Sample command:  score_jd2.linuxgccrelease -database db_path -no_output

General:
   -database                                   Path to rosetta_database

General Input:
   -in:file:path                               Base path for input data
   -no_optH                                    Dont change positions of Hydrogen atoms! (default true, specify false if you want optH)
   -score:optH_weights                         Weights file for optH (default standard.wts w/ sc12 patch)
   -score:optH_patch                           Weights patch file for optH
   -ignore_unrecognized_res                    Alternatively, add residue params file to rosetta_database
   -ignore_waters                              Ignore only WAT water molecules

PDB input:
   -in:file:s *.pdb   or                       PDB input file name
   -in:file:l  list_of_pdbs                    File containing list of PDB input file names relative to -in:file:path

Silent input:
   -in:file:silent silent.out                  silent input filesname
   -in:file:tags                               specify specific tags to be extracted, otherwise use all
   -in:file:fullatom                           for full atom structures
   -in:file:silent_struct_type <type>          Specify the input silent-file format e.g. 'binary' for binary silent files
                                                 protein_float, protein, rna, binary, score, binary_dna, pdb
   -in:file:silent_read_through_errors         Try to salvage damaged silent files
   -in:file:silent_renumber                    Renumber residues starting from 1
   -in:file:nonideal_silentfile                for non-ideal structures (such as from looprelax)
   -in:file:silent_optH

General Database:
   -inout:dbms:mode                            Specify database backend. default: 'sqlite3'
   -inout:dbms:database_name                   If sqlite3 the filename for the database
   -inout:dbms:pq_schema                       For PostgreSQL, the schema namespace in the database to use
   -inout:dbms:host                            NOTE to use mysql or postgres as a backend:
   -inout:dbms:user                               compile with 'extras=mysql' or 'extras=postgres' and use
   -inout:dbms:password                           the '-inout:dbms:mode mysql' or
   -inout:dbms:port                               the '-inout:dbms:mode postgres' flag
                                                  Consider using ~/.pgpass or ~/.my.cnf to store connection info

 Database Input:
   -in:use_database                               Indicate that structures should be read from the given database
   -in:select_structures_from_database            An sql query to select which structures should be extracted. e.g.:
                                                  "SELECT tag FROM structures WHERE tag = '7rsa';"

 General Output:

   -out:file:score_only                           Only output the score file
   -out:no_nstruct_label                          Do not append _#### to tag for output structures
   -out:overwrite                                 Overwrite structures if they already exist
   -out:path:all                                  Path where to write output data
   -out:prefix                                    Prefix for output structures
   -out:suffix                                    Suffix for output structures
   -out:file:fullatom                             Force full-atom output
   -out:file:scoretype <filename>                 Write scorefile (default default.sc)

PDB Output:
   -pdb                                           Output structures in PDB file format (Default)
   -pdb_gz                                        Output structures in zipped PDB file format

Silent Output:
  -out:file:silent <filename>                     Output structures to specified silent file (default 'default.out')
  -out:file:silent_gz                             Output structures to specified silent file in zipped format
  -out:file:silent_struct_type <type>             Specify the input silent-file format e.g.
                                                     protein_float, protein, rna, binary, score, binary_rna, pdb
  -out:file:silent_print_all_score_headers        Use if outputing structures for multiple sequences
  -out:file:silent_preserve_H                     Preserve hydrogens in PDB silent-file format

Database Output:
  -out:use_database                           Output structures to database (see General Database Options)


** Sample command:  relax.linuxgccrelease @flags > relax.log
** Options in flag file:
   -database                 Path to Rosetta databases
   -s                        Input file paths and name
   -relax:fast               Invokes a fast mode, which is almost as effective as a normal relax but 5-10 faster(optional)

** Normal IO and Score Function flags also apply         Call optH when reading silent files (useful for HisD/HisE determination)
                -score_app:linmin                           Run a quick linmin before scoring
Native:         -in:file:native                             native PDB if CaRMS is required
Scorefunction:  -score:weights  weights                     weight set or weights file
                -score:patch  patch                         patch set
                -rescore:verbose                            display score breakdown
                -rescore:output_only                        don't rescore
Output:         -nooutput                                   don't print PDB structures (default now)
                -output                                     force printing of PDB structures
                -out:file:silent                            write silent-out file
                -out:prefix  myprefix                       prefix the output structures with a string
```
