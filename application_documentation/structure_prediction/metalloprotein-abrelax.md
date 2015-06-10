#Metalloprotein ab initio / relax documentation

Metadata
========

Author: Chu Wang; transcribed by Steven Lewis (smlewi@gmail.com)

This page is a copy of an email sent by Chu Wang, the author of this code. The transcription into the doc folder was performed 6/18/12 by Steven Lewis. It is known that this documentation is not yet "up to spec".

Notes from Chu
==============

Hi Steven,

I don't remember writing one, and below is an email I sent to Bryan Der from your lab a while ago regarding how to set zinc constraints in Rosetta. Maybe you can put it somewhere for people to refer to (since I don't have written access to Rosetta svn repository any more). Thanks.

Chu

Hi Bryan,

I have not been following up with updated versions of Rosetta for a while, but there should be an integration test in the following location with all example input files. I am not sure there is a formal documentation around, but the protocols are based on standard protein folding and loop modeling protocols with additional constraint files. Here are a few additional files you will need:

/trunk/mini/test/integration/tests/metalloprotein\_abrelax/input

1.  fasta – more annotated than the standard one with [CYZ] or [HIS] to mark the residues that chelate the zinc, and Z[ZN] at the end for extra "zinc" residue.

2.  residue\_pair\_jump\_cst – explanation after \#\#

    ```
    BEGIN  ## starting marker
    jump_def: 3 60 59 59
    ## a rigid-body jump from the residue 3(first zinc-chelating residue) to 60(zinc) with a cut point starting at 59 and ending at 59 (last protein residue) This is to define how the fold tree is generated.
    aa: CYS ZN
    ## the residues on both sides of the jump
    cst_atoms: SG CB CA ZN V1 V2
    ## how distance, angular and dihedral parameters are defined
    jump_atoms: C CA N ZN V1 V2
    ## how the jump is defined between in the atom tree
    disAB: 2.20
    angleA: 68.0
    angleB: 70.5
    dihedralA: -150.0 -120.0 -90.0 -60.0 -30.0 0.0 30.0 60.0 90.0 120.0 150.0 180.0
    dihedralAB: -150.0 -120.0 -90.0 -60.0 -30.0 0.0 30.0 60.0 90.0 120.0 150.0 180.0
    dihedralB: 120.0
    ## all the degrees of freedom as defined in Figure 1 and Table 1 to generate various jump transformations between CYS and ZN
    END ## ending marker
    ```

    each blocks as above can define one zinc binding site and if you have more than one zinc, append more blocks like this.

3.  cen\_cst or fa\_cst files.– this is used to define constraints to keep chelating geometry optimal between zinc and other three chelating residues. The files are in standard Rosetta distance and angular constraints format.

Hope this helps.
