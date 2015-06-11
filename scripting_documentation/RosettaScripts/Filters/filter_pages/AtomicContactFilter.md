# AtomicContact
*Back to [[Filters|Filters-RosettaScripts]] page.*
## AtomicContact

Do two residues have any pair of atoms within a cutoff distance? Somewhat more subtle than ResidueDistance (which works by neighbour atoms). Iterates over all atom types of a residue, according to the user specified restrictions (sidechain, backbone, protons)

```
<AtomicContact name=(&string) residue1=(&integer) residue2=(&integer) sidechain=1 backbone=0 protons=0 distance=(4.0 &integer)/>
```

Some movers (e.g., PlaceSimultaneously) can set a filter's internal residue on-the-fly during protocol operation. To get this behaviour, do not specify residue2.

