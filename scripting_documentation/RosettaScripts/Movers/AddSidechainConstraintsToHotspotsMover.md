# AddSidechainConstraintsToHotspots
## AddSidechainConstraintsToHotspots

Adds harmonic constraints to sidechain atoms of target residues (to be used in conjunction with HotspotDisjointedFoldTree). Save the log files as those would be necessary for the next stage in affinity maturation.

```
<AddSidechainConstraintsToHotspots name=(&string) chain=(2 &Integer) coord_sdev=(1.0 &Real) resnums=(comma-delimited list of residue numbers)/>
```

-   resnums: the residues for which to add constraints. Notice that this list will be treated in addition to any residues that have cut points on either side.
-   coord\_sdev: the standard deviation on the coordinate restraints. The lower the tighter the restraints.


