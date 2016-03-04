<!-- --- title: ResidueSelectors -->

*Back to [[TaskOperations|TaskOperations-RosettaScripts]] page.*

ResidueSelectors
----------------

ResidueSelectors define a subset of residues from a Pose. Their apply() method takes a Pose and a ResidueSubset (a utility::vector1\< bool \>), and modifies the ResidueSubset. Unlike a PackerTask, a ResidueSubset does not have a commutativity requirement, so the on/off status for residue *i* can be changed as many times as necessary. Once a ResidueSubset has been constructed, a [[Residue Level TaskOperation|Residue Level TaskOperations]] may be applied to the ResidueLevelTasks which have a "true" value in the ResidueSubset. ResidueSelectors should be declared in their own block and named, or declared as subtags of other ResidueSelectors or of TaskOperations that accept ResidueSelectors (such as the OperateOnResidueSubset task operation).

Note that certain other Rosetta modules (e.g. the [[ReadResfile|ReadResfileOperation]] TaskOperation, which is not a Residue Level TaskOperation but can still accept a ResidueSelector as input) may also use ResidueSelectors.  Ultimately, it is hoped that many Rosetta components will be modified to permit this standardized method of selecting residues.

The purpose of separating the residue selection logic from the modifications that TaskOperations perform on a PackerTask is to make available the complicated logic of selecting residues that often lives in TaskOperations. If you have a complicated TaskOperation, consider splitting it into a ResidueSelector and operations on the residues it selects.

ResidueSelectors can be declared in their own block, outside of the TaskOperation block. For example:

    <RESIDUE_SELECTORS>
       <Chain name=chA chains=A/>
       <Index name=res1to10 resnums=1-10/>
    </RESIDUE_SELECTORS>

[[_TOC_]]

### Logical ResidueSelectors

#### NotResidueSelector

    <Not name=(&string) selector=(&string)>

or

    <Not name=(&string)>
        <(Selector) .../>
    </Not>

-   The NotResidueSelector requires exactly one selector.
-   The NotResidueSelector flips the boolean status returned by the apply function of the selector it contains.
-   If the "selector" option is given, then a previously declared ResidueSelector (from the RESIDUE\_SELECTORS block of the XML file) will be retrieved from the DataMap
-   If the "selector" option is not given, then a sub-tag containing an anonymous/unnamed ResidueSelector must be declared. This selector will not end up in the DataMap

#### AndResidueSelector

    <And name=(&string) selectors=(&string)>
       <(Selector1)/>
       <(Selector2)/>
        ...
    </And>

-   The AndResidueSelector can take arbitrarily many selectors.
-   The AndResidueSelector takes a logical *AND* of the ResidueSubset vectors returned by the apply functions of each of the ResidueSelectors it contains.  <b>Practically speaking, this means that it returns the <i>intersection</i> of the selected sets -- the residues that are in set 1 AND in set 2.</b>  (Do not confuse this with the "or" selector, which returns the union of the two sets -- the residues that are in set 1 OR in set 2.)
-   The "selectors" option should be a comma-separated string of previously-declared selector names. These selectors will be retrieved from the DataMap.
-   The "selectors" option is not required, nor are the sub-tags required; but at least one of the two must be given. Both can be given, if desired.
-   Selectors declared in the sub-tags will be appended to the set of selectors for the AndResidueSelector, but will not be added to the DataMap.

#### OrResidueSelector

    <Or name=(&string) selectors=(&string)>
       <(Selector1)/>
       <(Selector2)/>
        ...
    </Or>

-   The OrResidueSelector can take arbitrarily many selectors.
-   The OrResidueSelector takes a logical *OR* of the ResidueSubset vectors returned by the apply functions of each of the ResidueSelectors it contains.  <b>Practically speaking, this means that it returns the <i>union</i> of the selected sets -- the residues that are in set 1 OR in set 2.</b>  (Do not confuse this with the "and" selector, which returns the intersection of the two sets -- the residues that are in set 1 AND in set 2.)
-   The "selectors" option should be a comma-separated string of previously-declared selector names. These selectors will be retrieved from the DataMap.
-   The "selectors" option is not required, nor are the sub-tags required; but at least one of the two must be given. Both can be given, if desired.
-   Selectors declared in the sub-tags will be appended to the set of selectors for the OrResidueSelector, but will not be added to the DataMap.

### Conformation Independent Residue Selectors

#### ChainSelector

    <Chain chains=(&string)/>

-   The string given for the "chains" option should be a comma-separated list of chain identifiers
-   Each chain identifier should be either an integer, so that the Pose chain index will be used, or a single character, so that the PDB chain ID can be used.
-   The ChainSelector sets the positions corresponding to all the residues in the given set of chains to true, and all the other positions to false.

#### JumpDownstreamSelector

    <JumpDownstream jump=(&int)/>

-   The integer given for the "jump" argument should refer to a Jump that is present in the Pose.
-   The JumpDownstreamSelector sets the positions corresponding to all of the residues that are downstream of the indicated jump to true, and all the other positions to false.
-   This selector is logically equivalent to a NotSelector applied to the JumpUpstreamSelector for the same jump.

#### JumpUpstreamSelector

    <JumpUpstream jump=(&int)/>

-   The integer given for the "jump" argument should refer to a Jump that is present in the Pose.
-   The JumpUpstreamSelector sets the positions corresponding to all of the residues that are upstream of the indicated jump to true, and all the other positions to false.
-   This selector is logically equivalent to a NotSelector applied to the JumpDownstreamSelector for the same jump.

#### ResidueIndexSelector

    <Index resnums=(&string)/>

-   The string given for the "resnums" option should be a comma-separated list of residue identifiers
-   Each residue identifier should be either *an integer* , so that the Pose numbering can be used, *two integers separated by a dash* , designating a range of Pose-numbered residues, or *an integer followed by a single character* , e.g. 12A, referring to the PDB numbering for residue 12 on chain A. (Note, residues that contain insertion codes cannot be properly identified by this scheme).
-   The ResidueIndexSelector sets the positions corresponding to the residues given in the resnums string to true, and all other positions to false.

#### ResidueNameSelector
Selects residues by their full Rosetta residue type name. At least one of residue_names and residue_name3 must be specified.

    <ResidueName residue_names=(&string) residue_name3=(&string) />

residue_names - A comma-separated list of Rosetta residue names (including patches). For example, "CYD" will select all disulfides, and "CYD,SER:NTermProteinFull,ALA" will select all disulfides, alanines, and N-terminal serines -- all other residues will not be selected (i.e. be false in the ResidueSubset object).

residue_name3 - A comma-separated list of 3-letter Rosetta residue names.  These will be selected regardless of variant type. For example, "SER" will select residues named "SER", "SER:NtermProteinFull", and "SER:Phosphorylated".

**Example**
This example will select all variants of ALA, C-terminal ASN residues, and disulfides:

    <ResidueName residue_names="ASN:CtermProteinFull,CYD" residue_name3="ALA" />


### Conformation Dependent Residue Selectors

#### InterGroupInterfaceByVector

    <InterfaceByVector name=(%string) cb_dist_cut=(11.0&float) nearby_atom_cut=(5.5%float) vector_angle_cut=(75.0&float) vector_dist_cut=(9.0&float) grp1_selector=(%string) grp2_selector=(%string)/>

or

    <InterfaceByVector name=(%string) cb_dist_cut=(11.0&float) nearby_atom_cut=(5.5%float) vector_angle_cut=(75.0&float) vector_dist_cut=(9.0&float)>
       <(Selector1)/>
       <(Selector2/>
    </InterfaceByVector>

-   Selects the subset of residues that are at the interface between two groups of residues (e.g. residues on different chains, which might be useful in docking, or residues on the same chain, which might be useful in domain assembly) using the logic for selecting interface residues as originally developed by Stranges & Leaver-Fay. This logic selects residues that are either already in direct contact with residues in the other group (i.e. contain atoms within the nearby\_atom\_cut distance threshold of the other group's atoms) or that are pointing their c-alpha-c-beta vectors towards the other group so that at least one c-beta *i* -c-alpha *i* -c-alpha *j* angle (between residues *i* and *j* ) is less than the vector\_angle\_cut angle threshold (given in degrees) and has its neighbor atom within the vector\_dist\_cut distance threshold (given in Angstroms) of at least one neighbor atom from the other group.
-   Groups 1 and 2 can be given either through the grp1\_selector and grp2\_selector options, (requiring that the indicated selectors had been previously declared and placed in the DataMap) or may be declared anonymously in the given subtags. Anonymously declared selectors are not added to the DataMap.
-   The cb\_dist\_cut is a fudge factor used in this calculation and is used only in constructing an initial graph; neighbor relationships are only considered between pairs of residues that have edges in this initial graph. cb\_dist\_cut should be greater than vector\_dist\_cut.

#### LayerSelector

The LayerSelector lets a user select residues by burial.  Burial can be assessed by number of sidechain neighbors within a cone along the CA-CB vector (the default method), or by SASA.

```
     <Layer name=(&string) select_core=(false &bool) select_boundary=(false &bool) select_surface=(false &bool)
          ball_radius=(2.0 &Real) use_sidechain_neighbors=(true &bool)
          sc_neighbor_dist_exponent=(1.0 &Real) sc_neighbor_dist_midpoint=(9.0 &Real)
          sc_neighbor_denominator=(1.0 &Real) sc_neighbor_angle_shift_factor=(0.5 &Real)
          sc_neighbor_angle_exponent=(2.0 &Real)
          core_cutoff=(5.2 &Real) surface_cutoff=(2.0 &Real)
     />
```

Options:
- select\_core=(false &bool), select\_boundary=(false &bool), select\_surface=(false &bool): Boolean flags that let the user choose which layer(s) should be selected.  Multiple layers are permitted, but at least one must be specified or no residues will be selected.
- use_sidechain_neighbors=(true &bool): Should the sidechain neighbor algorithm be used to determine which layer a residue lies in (the default) or should the older SASA-based method be used instead?

SASA-specific options:
- ball_radius=(2.0 &Real): The radius for the rolling ball algorithm used to pick residues by SASA.
- core_cutoff=(20.0 &Real), surface_cutoff=(40.0 &Real):  The SASA values (as a percentage of total surface area) below which a residue is sorted into the core group, or above which a residue is sorted into the surface group.  Note that setting use_sidechain_neighbors=false alters the default values of core_cutoff and surface_cutoff.

Sidechain neighbor-specific options:
- core_cutoff=(5.2 &Real), surface_cutoff=(2.0 &Real):  The number of sidechain neighbors (weighted counts -- see below) above which a residue is sorted into the core group, or below which a residue is sorted into the surface group.

Neighbor residues are counted, weighted by a factor that is a distance factor multiplied by an angle factor.  The two factors are calculated as follows:

**distance factor = 1 / (1 + exp( n*(d - m) ) )**, where **d** is the distance of the neighbor from the residue CA, **m** is the midpoint of the distance falloff, and **n** is a falloff exponent factor that determines the sharpness of the distance falloff (with higher values giving sharper falloff near the midpoint distance).

**angle factor = ( (cos(theta)+a)/(1+a) )^b**, where **theta** is the angle between the CA-CB vector and the CA-neighbor vector, **a** is an offset factor that widens the cone somewhat, and **b** is an exponent that determines the sharpness of the angular falloff (with lower values resulting in a broader cone with a sharper edge falloff).

The parameters above generally need not be changed from their default values.  If the user wishes to change them, though, he or she can do so by altering the following:

- sc_neighbor_dist_exponent=(1.0 &Real): Alters the value of **n**, the distance exponent.
- sc_neighbor_dist_midpoint=(9.0 &Real): Alters the value of **m**, the distance falloff midpoint.
- sc_neighbor_angle_shift_factor=(0.5 &Real): Alters the value of **a**, the angular shift factor.
- sc_neighbor_angle_exponent=(2.0 &Real): Alters the value of **b**, the angular sharpness value.
- sc_neighbor_denominator=(1.0 &Real): Alters the value by which the overall expression is divided.

#### NeighborhoodResidueSelector

    <Neighborhood name=(%string) resnums=(%string) distance=(10.0%float)/>

or

    <Neighborhood name=(%string) selector=(%string) distance=(10.0%float)/>

or

    <Neighborhood name=(%string) distance=(10.0%float)>
       <Selector ... />
    </Neighborhood>

-   The NeighborhoodResidueSelector selects all the residues within a certain distance cutoff of a focused set of residues.
-   It sets each position in the ResidueSubset that corresponds to a residue within a certain distance of the focused set of residues as well as the residues in the focused set to true, and sets all other positions to false.
-   The set of focused residues can be specified in one of three (mutually exclusive) ways: through a resnums string (see the ResidueIndexSelector [[above|TaskOperations-RosettaScripts#ResidueIndexSelector]] for documentation on how this string should be formatted), a previously-declared ResidueSelector using the "selector" option, or by defining a subtag that declares an anonymous ResidueSelector.

#### NumNeighborsSelector

    <NumNeighbors name=(%string) count_water=(false&bool) threshold=(17%integer) distance_cutoff=(10.0&float)/>

-   The NumNeighborsSelector sets to true each position in the ResidueSubset that corresponds to a residue that has at least *threshold* neighbors within *distance\_cutoff,* and sets all other positions to false.
-   The NumNeighborsSelector uses the coordinate of each residue's neighbor atom as a representative and counts two residues as being neighbors if their neighbor atoms are within *distance\_cutoff* of each other.
-   It is possible to include water residues in the neighbor count by setting the "count\_water" boolean to true

#### PrimarySequenceNeighborhoodSelector

    <PrimarySequenceNeighborhood name=(%string) selector=(%string) lower=(1%int) upper=(1%int) />

or

    <PrimarySequenceNeighborhood name=(%string) lower=(1%int) upper=(1%int) >
       <Selector ... />
    </PrimarySequenceNeighborhood>

-   The PrimarySequenceNeighborhoodResidueSelector selects all the residues within a certain number of residues of a given selection in primary sequence. For example, given a selection of residue 5, PrimarySequenceNeighborhood would select residues 4, 5 and 6 by default.  If upper was set to 2, it would select residues 4, 5, 6 and 7. This ResidueSelector is chain-aware, meaning it will not select residues on a different chain than the original selection. In the example above, if residue 5 were on chain 1 and residue 6 were on a chain 2, the PrimarySequenceNeighborhoodResidueSelector would select residues 4 and 5 only.

**lower** - Number of residues to select lower (i.e. N-terminal) to the input selection. Default=1

**upper** - Number of residues to select upper (i.e. C-terminal) to the input selection. Default=1


#### SecondaryStructureSelector

    <SecondaryStructure name=(%string) ss=(%string) include_terminal_loops=(%bool, false) always_use_dssp=(%bool, false) />

SecondaryStructureSelector selects all residues with given secondary structure. For example, you might use it to select all loop residues in a pose.  SecondaryStructureSelector uses the secondary structure information in the pose to compute residues to select -- if that information is not present, or if always_use_dssp is set, it calls DSSP to determine the secondary structure of the pose.

**ss** - The secondary structure types to be selected. This parameter is required. Valid secondary structure characters are 'E', 'H' and 'L'. To select loops, for example, use ss="L", and to select both helices and sheets, use ss="HE"

**include_terminal_loops** - (default: false) If false, one-residue "loop" regions at the termini of chains will be ignored. If true, all residues will be considered for selection.

**always_use_dssp** - (default: false). If true, dssp will be used to determine the pose secondary structure every time the SecondaryStructure residue selector is applied. If false, and a secondary structure is set in the pose, the secondary structure in the pose will be used without re-computing DSSP.

**Example**
The example below selects all residues in the pose with secondary structure 'H' or 'E'.

    <SecondaryStructure name="all_non_loop" ss="HE" />

####Task
    <Task name=(%string) fixed=(%bool, False) packable=(%bool, False) designable=(%bool, False) task_operations=(%string) />

The TaskSelector uses user-provided task operations to define a selection. Task operations are run on the pose, and residues are selected based on their status in the resulting PackerTask (designable, packable, or fixed). Note that if none of these options is specified, no residue will be selected. This is useful for legacy protocols which still use task operations to select residues (which were written before ResidueSelectors existed). New protocols should use ResidueSelectors to select residues.

**task_operations** - Required. The task operations used to define the selection.

**fixed** - If true, residues in the PackerTask marked as fixed (i.e. not packable or designable) will included in the selection. Default = False

**packable** - If true, residues in the PackerTask marked as packable will be included in the selection. Default = False

**designable** - If true, residues in the PackerTask marked as designable will be included in the selection. Default = False

####ResiduePDBInfoHasLabel

    <ResiduePDBInfoHasLabel name=(%string) property=(%string) />

The ResiduePDBInfoHasLabel residue selector selects all residues with the given PDB residue label. Some protocols (e.g. MotifGraft, Disulfidize) use these labels to mark residues, and this selector allows those residues to be selected without the user's knowledge of which residues were marked.

**label** - Required. The PDB residue info label to be selected. (e.g. "DISULFIDIZE")

**Example**
The example below selects all residues that were converted to disulfides by the Disulfidize mover.

    <ResiduePDBInfoHasLabel name="all_disulf" property="DISULFIDIZE" />


##See Also

* [[RosettaScripts|RosettaScripts]]: Using RosettaScripts
* [[Task Operations | TaskOperations-RosettaScripts]]: Other TaskOperations in RosettaScripts
* [[Conventions in RosettaScripts|RosettaScripts-Conventions]]
* [[I want to do x]]: Guide for making specific structural pertubations using RosettaScripts
* [[Scripting Interfaces|scripting_documentation/Scripting-Documentation]]: Other ways to interact with Rosetta in customizable ways
* [[Running Rosetta with options]]: Instructions for running Rosetta executables.
* [[Analyzing Results]]: Tips for analyzing results generated using Rosetta
* [[Rosetta on different scales]]: Guidelines for how to scale your Rosetta runs
* [[Preparing structures]]: How to prepare structures for use in Rosetta