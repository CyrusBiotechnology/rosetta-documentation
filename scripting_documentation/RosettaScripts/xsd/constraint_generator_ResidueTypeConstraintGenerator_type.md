<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
Generates residue type constraints (either to native or to a specific residue type) for the specified residues

```xml
<ResidueTypeConstraintGenerator name="(&string;)"
        favor_native_bonus="(1.0 &real;)" rsd_type_name3="(&string;)"
        use_native="(false &bool;)" residue_selector="(&string;)" />
```

-   **favor_native_bonus**: Unweighted score bonus the pose will receive for having the specified residue type at the specified position
-   **rsd_type_name3**: Three-letter code for the amino acid to which you want to constrain this residue. If unspecified, this defaults to the native amino acid at this position.
-   **use_native**: Use native structure (provided with in:file:native) as reference pose for defining desired residue identities
-   **residue_selector**: Selector specifying residues to be constrained. When not provided, all residues are selected. The name of a previously declared residue selector or a logical expression of AND, NOT (!), OR, parentheses, and the names of previously declared residue selectors. Any capitalization of AND, NOT, and OR is accepted. An exclamation mark can be used instead of NOT. Boolean operators have their traditional priorities: NOT then AND then OR. For example, if selectors s1, s2, and s3 have been declared, you could write: 's1 or s2 and not s3' which would select a particular residue if that residue were selected by s1 or if it were selected by s2 but not by s3.

---