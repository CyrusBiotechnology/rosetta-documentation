# NormalModeRelax
*Back to [[Mover|Movers-RosettaScripts]] page.*
## NormalModeRelax

NormalModeRelax relaxes a structures to the coordinate directions derived from Anisotropic Network Model (ANM). The way how it works is: a) generate a extrapolated coordinates, b) put coordinate restraints to that coordinates, and c) run minimization or FastRelax. Current implementation tries multiple normal modes and returns the best scoring one. 

```
<NormalModeRelax name="&string"
       cartesian=(true &bool)
       centroid=(true &false)
       scorefxn=("" &string)
       nmodes=(5 &Size)
       mix\_modes=(false &bool)
       pertscale=(1.0 &Real)
       randomselect=(false &bool)
       relaxmode=("min" &string)
       outsilent=("" &string)
       nsample=(nmodes &Size)
       cartesian\_minimize=(false &bool) />
```

-   cartesian: Use Cartesian normal model, which is recommended over Torsional normal mode 
-   centroid: Use centroid description in the Mover. relaxmode = "relax" is prevented in this case.  
-   scorefxn: Scorefxn to relax poses and select among multiple solutions. For centroid mode, stage4
-   nmodes: How many normal modes to try.
-   mix\_modes: Whether use mixture of modes.
-   pertscale: By how much to perturb the backbone in Angstrom. 
-   randomselect: Do random selection among sampled structures instead of picking best scoring one.
-   relaxmode: The way of relaxing structure. 
-   cartesian_minimize: Use cartesian minimization for relax or minimization.
-   nsample: How many structures to try. This only works when mix_modes = true; otherwise will try 2*nmodes.
-   outsilent: Specifies name of output silent file to store all the sampled structures. By default the Mover won't output any silent. 

This mover can also take an optional MoveMap (see [[FastRelax|FastRelaxMover]] documentation for details) to define the residue subset to which it should be applied. MoveMap definition will be useful to control the behavior of heteromolecules during relax or minimization stage.  In the absence of the MoveMap, the mover is applied to the whole pose.

See:
https://en.wikipedia.org/wiki/Anisotropic_Network_Model
Anisotropy of fluctuation dynamics of proteins with an elastic network model, A.R. Atilgan et al., Biophys. J. 80, 505 (2001)


