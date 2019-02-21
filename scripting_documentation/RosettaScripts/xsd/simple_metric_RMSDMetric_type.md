<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
This metric calculates the RMSD between the input and the set comparison pose.
  You may use the cmd-line native if the option use_native is true.
  Default is to calculate all_heavy atoms - but this can be set using the option rmsd_types.

   We match all corresponding atoms for each residue to match.   By default we do not fail and are robust to length mismatches - only matching what we can for each residue.

```xml
<RMSDMetric name="(&string;)" custom_type="(&string;)"
        reference_name="(&string;)" residue_selector="(&string;)"
        residue_selector_ref="(&string;)" robust="(true &bool;)"
        use_native="(false &bool;)" super="(false &bool;)"
        rmsd_type="(&rmsd_types;)" />
```

-   **custom_type**: Allows multiple configured SimpleMetrics of a single type to be called in a single RunSimpleMetrics and SimpleMetricFeatures. 
 The custom_type name will be added to the data tag in the scorefile or features database.
-   **reference_name**: Name of reference pose to use
-   **residue_selector**: Calculate the RMSD for these residues for both reference and main pose.
-   **residue_selector_ref**: Selector for the reference pose (input native or passed reference pose. ).  Residues selected must be same number of residues selected for the main selector.
-   **robust**: Set whether we are robust to atom mismatches for selected residues.  By default we only match atoms that are corresponding. (True).
-   **use_native**: Use the native if present on the cmd-line.
-   **super**: Run a superposition on the residues in the residue_selector (or all) before RMSD calculation and the atoms selected for RMSD
-   **rmsd_type**: Type of calculation.  Current choices are: 
[rmsd_all, rmsd_all_heavy, rmsd_protein_bb_ca, rmsd_protein_bb_heavy, rmsd_protein_bb_heavy_including_O, rmsd_sc, rmsd_sc_heavy]

---