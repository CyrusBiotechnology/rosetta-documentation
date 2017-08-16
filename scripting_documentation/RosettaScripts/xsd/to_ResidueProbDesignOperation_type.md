<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
Randomly adds allowed residue types to the PackerTask. Based on the user specified weights [0.0-1.0] for each residue type per position this operation will add the residue type to the list of allowed design residues in a non-deterministic manner.

```xml
<ResidueProbDesignOperation name="(&string;)" weights_file="(&string;)"
        include_native_restype="(true &bool;)"
        keep_task_allowed_aas="(false &bool;)" sample_zero_probs="(0.0 &real;)"
        picking_rounds="(1 &positive_integer;)" />
```

-   **weights_file**: (REQUIRED) Path to a file containting weights for each residue type at each position in the following format:
POSNUM RESIDUETYPE WEIGHT
With weight as a real number between 0.0 and 1.0. Unspecified residues and/or residue types will automatically default to a weight of 1.0.
-   **include_native_restype**: The native residue type is always an allowed residue type.
-   **keep_task_allowed_aas**: If set to true, the sampled residue types will not replace all other previously allowed residue types.
-   **sample_zero_probs**: Overwrite the sampling probability for all residue types with a weight of zero with the given weight. For example, if you have a probability of zero, you can sample this instead at like .3 or .5 or whatever you want. The idea is that you don't need to go and change your input data to add some variability in the data - useful if you have a very low sampling rate of the input data.
-   **picking_rounds**: Allowed residue types can be sampled multiple times. Especially of interest in combination with the option 'keep_task_allowed_aas'.

---