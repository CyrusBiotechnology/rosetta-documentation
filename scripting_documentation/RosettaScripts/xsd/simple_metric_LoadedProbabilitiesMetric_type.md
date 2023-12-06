<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
A class to load a probabilities weights file into a PerResidueProbabilitiesMetric. Does not involve the current pose for the calculate function, but stores the loaded values in the pose cache.

References and author information for the LoadedProbabilitiesMetric simple metric:

LoadedProbabilitiesMetric SimpleMetric's author(s):
Moritz Ertelt, University of Leipzig [moritz.ertelt@gmail.com]  (Wrote the LoadedProbabilitiesMetric.)

```xml
<LoadedProbabilitiesMetric name="(&string;)" custom_type="(&string;)"
        output_as_pdb_nums="(false &bool;)" residue_selector="(&string;)"
        filename="(&string;)" />
```

-   **custom_type**: Allows multiple configured SimpleMetrics of a single type to be called in a single RunSimpleMetrics and SimpleMetricFeatures. 
 The custom_type name will be added to the data tag in the scorefile or features database.
-   **output_as_pdb_nums**: If outputting to scorefile use PDB numbering+chain instead of Rosetta (1 - N numbering)
-   **residue_selector**: If a residue selector is present, we only calculate and output metrics for the subset of residues selected. The name of a previously declared residue selector or a logical expression of AND, NOT (!), OR, parentheses, and the names of previously declared residue selectors. Any capitalization of AND, NOT, and OR is accepted. An exclamation mark can be used instead of NOT. Boolean operators have their traditional priorities: NOT then AND then OR. For example, if selectors s1, s2, and s3 have been declared, you could write: 's1 or s2 and not s3' which would select a particular residue if that residue were selected by s1 or if it were selected by s2 but not by s3.
-   **filename**: (REQUIRED) The name of the output weights file storing the probabilities.

---