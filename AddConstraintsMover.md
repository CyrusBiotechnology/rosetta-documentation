# AddConstraints
*Back to [[Mover|Movers-RosettaScripts]] page.*
## AddConstraints Mover

This mover uses one or more constraint generators to generate a set of constraints and add them to the pose. The set of constraints can be specifically removed by the RemoveConstraints mover.

Reminder: constraints added to the pose will not do anything if the appropriate constraint score terms are not turned on. 

```
<AddConstraints name=(&string) >
   <!-- Constraint generator 1 ... -->
   <!-- Constraint generator 2 ... -->
   <!-- Constraint generator n ... -->
</AddConstraints>
```

###Example

This example adds coordinate constraints to all residues in sheets.

```
<RESIDUE_SELECTORS>
    <SecondaryStructureSelector name="sheet" ss="E" use_dssp="1" />
</RESIDUE_SELECTORS>
<MOVERS>
    <AddConstraints name="add_coord_csts" >
        <CoordinateConstraintGenerator name="coord_cst_gen" residue_selector="sheet" />
    </AddConstraints>
</MOVERS>
```

##See Also

* [[RemoveConstraints]]
* [[ConstraintSetMover]]
* [[ConstraintScoreFilter]]
* [[Constraints file format|constraint-file]]
* [[AtomCoordinateCstMover]]
* [[ClearConstraintsMover]]
* [[AddConstraintsToCurrentConformationMover]]
* [[AddSidechainConstraintsToHotspotsMover]]
* [[TaskAwareCstsMover]]
