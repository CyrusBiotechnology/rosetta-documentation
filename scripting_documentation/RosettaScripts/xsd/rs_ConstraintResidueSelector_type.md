<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
Selects those residues that have distance/angle/dihedral constraints

```xml
<ConstraintResidueSelector name="(&string;)" distance="(1 &bool;)"
        angle="(1 &bool;)" dihedral="(1 &bool;)" />
```

-   **distance**: Select distance (csttype=AtomPair) constraints.
-   **angle**: Select angle (csttype=Angle) constraints.
-   **dihedral**: Select dihedral (csttype=Dihedral) constraints.

---