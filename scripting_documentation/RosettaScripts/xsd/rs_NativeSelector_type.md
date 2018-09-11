<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
A ResidueSelector that applies a given residue selector to the native pose. If the native pose is shorter than the given pose, 'false' values will be appended onto the end of the returned residue subset. If the native pose is longer than the given pose, then the subset will be shortened to be the length of the given pose.

```xml
<NativeSelector name="(&string;)" residue_selector="(&string;)" >
    <Residue Selector Tag ... />
</NativeSelector>
```

-   **residue_selector**: Name of residue selector to be applied to the native pose.


"Residue Selector Tag": Any of the [[ResidueSelectors]]

---