<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
XRW TODO

```xml
<SetupForSequenceSymmetryMover name="(&string;)"
        independent_regions="(&string;)" />
```

-   **independent_regions**: Comma-separated list of residue selectors. Each one defines a region of sequence symmetry. For example, say you had a dimer and a trimer in the same system. You would pass one residue selector that selects all of the residues in the dimer and a second residue selector that selects all of the residues in the trimer. independent_regions="dimer_selector,trimer_selector". If the user provides residue selectors, this will not enforce sequence symmetry on regions not covered by any selection. Otherwise the entire protein will be treated as one single region of sequence symmetry.

---