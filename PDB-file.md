## TODO

## Metadata

The Rosetta Membrane Framework was developed by Julia Koehler Leman and Rebecca Alford at the Gray Lab at JHU. 
Last updated: 12/12/14. 

For questions please contact: 
- Julia Koehler Leman ([julia.koehler1982@gmail.com](julia.koehler1982@gmail.com))
- Rebecca Alford ([rfalford12@gmail.com](rfalford12@gmail.com))
- Corresponding PI: Jeffrey J. Gray ([jgray@jhu.edu](jgray@jhu.edu))

## Description

Some applications require a protein structure as an input, such as MPrelax, MP ddg, and MPDocking. The documentation of the specific application will tell you what is needed. Generally, PDBs do not need to be transformed into the membrane coordinate frame for Rosetta to work. However, if knowledge of the protein orientation in the membrane is needed, there are two servers besides Rosetta that provide either transformed PDB files or the rotation / translation matrices required for this transformation. 

The PDBTM ([http://pdbtm.enzim.hu/](http://pdbtm.enzim.hu/)) is a weekly-updated PDB of TransMembrane proteins that has provides the transformed PDBs for download. It uses the TMDET server ([http://tmdet.enzim.hu/](http://tmdet.enzim.hu/)) to correctly position the protein in the membrane. The provided XML file contains both the rotation / translation matrices as well as the optimal membrane thickness for this protein.  

The OPM database ([http://opm.phar.umich.edu/](http://opm.phar.umich.edu/)) provides a similar service with the PPM server ([http://opm.phar.umich.edu/server.php](http://opm.phar.umich.edu/server.php)) which might be more accurate for prediction of the optimal membrane thickness for the specific protein. 

In Rosetta, two movers are available that transform the protein into the membrane coordinate frame, both of which do a simple transformation based on protein topology information. **Be aware that NEITHER of these two movers uses the scoring function for optimal high-resolution positioning!!!**

The *MembranePositionFromTopologyMover* uses the protein topology (SpanningTopology from the spanfile) and the structure to calculate an optimal position and orientation (center and normal) of the membrane, as represented by the MembraneResidue. This mover should only be used for a **fixed protein and a flexible membrane** (i.e. a protein residue is at the root of the FoldTree).

The **TransformIntoMembraneMover** does the reverse from above by using the protein topology and the protein structure to calculate an optimal position and orientation of the membrane to then rotate and translate the protein into this fixed coordinate frame. This mover should only be used for a **fixed membrane and a flexible protein** (i.e. the membrane residue is at the root of the FoldTree).

The MembraneResidue can be present in the PDB file and can be read in using the flag `-membrane_new::setup::membrane_rsd <membrane residue number>`.

MEM

## Code and documentation

## Flags

## Example

## References

* Alford RF, Koehler Leman J, Weitzner BD, Duran AM, Tilley D, Elazar A, Gray JJ. (2015) An integrated framework enabling computational modeling and design of Membrane Proteins. PlosOne - in preparation 
