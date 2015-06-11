# SymPackRotamersMover and SymRotamerTrialsMover
*Back to [[Mover|Movers-RosettaScripts]] page.*
## SymPackRotamersMover and SymRotamerTrialsMover

The symmetric versions of pack rotamers and rotamer trials movers (they take the same tags as asymmetric versions)

```
<SymPackRotamersMover name=symm_pack_rot scorefxn=score12_symm task_operations=.../>
<SymRotamerTrialsMover name=symm_rot_trials scorefxn=score12_symm task_operations=.../>
```


##See Also

* [[Symmetry]]: Using symmetry in Rosetta
* [[SymmetryAndRosettaScripts]]
* [[Fixbb]]: Application to pack rotamers
* [[SetupForSymmetryMover]]
* [[SetupNCSMover]]
* [[DetectSymmetryMover]]
* [[ExtractAsymmetricUnitMover]]
* [[ExtractAsymmetricPoseMover]]
* [[SymMinMover]]
* [[SymDofMover]]
* [[MakeBundleMover]]
