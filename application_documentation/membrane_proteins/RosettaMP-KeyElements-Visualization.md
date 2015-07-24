# Membrane visualization with the PyMOL Viewer

## Description

The Membrane Framework in Rosetta interacts with Pymol through the PyMOL Mover. For more information, please see the general visualization page. When the membrane framework is in use,  two membrane planes are visualized and updated in real-time during a Rosetta simulation. To run it

1. open pymol first
2. run the PymolPyRosettaServer.py script inside Pymol
3. run your Rosetta script from the regular terminal
4. then lean back, watch and enjoy some popcorn.

## Code and documentation

The **VisualizeMembraneMover** is a mover that adds virtual residues representing the membrane planes to the pose that can be visualized individually. It is less light-weight than the Pymol plugin described above because (1) the pose is extended by a large number of additional residues and (2) the membrane is represented by a grid of residues, instead of 4x2 points defining the dimensions of the membrane. It is still useful if you are using a visualization program different from Pymol.

The **VisualizeEmbeddingMover** is a mover that adds virtual residues for each transmembrane span and also the average of those representing the overall embedding of the pose in the membrane. Currently (2/17/15), the virtual residues are not updated after a move, so make sure to only call it at the last step in your protocol. 

## Command line

`-show_simulation_in_pymol 0` invokes the PymolMover, updating the protein in Pymol every frame. If the protein is a membrane protein, the membrane will be shown automatically. 

## References

* Alford RF, Koehler Leman J, Weitzner BD, Duran AM, Tilley DC, Elazar A, Gray JJ. (2015) An integrated framework enabling computational modeling and design of Membrane Proteins. PlosOne - in preparation 
* Baugh EH, Lyskov S, Weitzner BD, Gray JJ (2011) Real-Time PyMOL Visualization for Rosetta and PyRosetta. PLoS ONE 6(8): e21931.
