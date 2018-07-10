<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
XRW TO DO

```xml
<SpliceIn name="(&string;)" tolerance="(0.23 &real;)"
        ignore_chain_break="(false &bool;)" debug="(false &bool;)"
        min_seg="(false &bool;)" CG_const="(false &bool;)"
        rb_sensitive="(false &bool;)" chain_num="(1 &non_negative_integer;)"
        segment="(&string;)" superimposed="(true &bool;)"
        delete_hairpin="(false &bool;)"
        delete_hairpin_n="(4 &non_negative_integer;)"
        delete_hairpin_c="(13 &non_negative_integer;)" tail_segment="(&n_or_c;)"
        source_pdb_to_res="(&refpose_enabled_residue_number;)"
        skip_alignment="(false &bool;)" use_sequence_profile="(&bool;)"
        scorefxn="(&string;)" template_file="(&string;)"
        task_operations="(&task_operation_comma_separated_list;)"
        design_task_operations="(&string;)" residue_numbers_setter="(&string;)"
        torsion_database="(&string;)" database_entry="(&non_negative_integer;)"
        database_pdb_entry="(&string;)" design_shell="(6.0 &real;)"
        repack_shell="(8.0 &real;)" randomize_cut="(false &bool;)"
        cut_secondarystruc="(false &bool;)" delta_lengths="(&int_cslist;)"
        thread_original_sequence="(false &bool;)" rtmin="(true &bool;)"
        dbase_iterate="(false &bool;)" checkpointing_file="(&string;)" >
    <Segments name="(&string;)" profile_weight_away_from_interface="(1.0 &real;)"
            current_segment="(&string;)" >
        <segment name="(&string;)" pdb_profile_match="(&string;)" profiles="(&string;)" />
    </Segments>
</SpliceIn>
```

-   **tolerance**: tolernace of bond length std when checking for chain breaks
-   **ignore_chain_break**: if there is a chain break don't fail
-   **debug**: more verbose during run
-   **min_seg**: minimize segment after splice in?
-   **CG_const**: apply coordinate constraints on C-gamma atoms
-   **rb_sensitive**: apply rigid body addaptations
-   **chain_num**: which chain number to apply splice on
-   **segment**: segment name
-   **superimposed**: are thr structures superimposed
-   **delete_hairpin**: delete hairpin segment?
-   **delete_hairpin_n**: how many residues to delete on N-ter hairpin
-   **delete_hairpin_c**: how many residues to delete on C-ter hairpin
-   **tail_segment**: what direction is the tail segment
-   **source_pdb_to_res**: residue number of last residue on source segment
-   **skip_alignment**: whether to align pose to source pdb
-   **use_sequence_profile**: Use sequence profiles in design?
-   **scorefxn**: Name of score function to use
-   **template_file**: What is the template file to use during design
-   **task_operations**: A comma separated list of TaskOperations to use.
-   **design_task_operations**: XRW TO DO
-   **residue_numbers_setter**: XRW TO DO
-   **torsion_database**: torsion db file name
-   **database_entry**: choose db entry by number
-   **database_pdb_entry**: XRW TO DO
-   **design_shell**: design shell radius
-   **repack_shell**: packing shell radius
-   **randomize_cut**: should the cut be put randomly in the designed segment
-   **cut_secondarystruc**: put cut into ss
-   **delta_lengths**: comma separated list of allowed segment delta lengths
-   **thread_original_sequence**: thread sequence from source pdb onto design
-   **rtmin**: should we apply rtmin after design
-   **dbase_iterate**: should we iterate through the db file
-   **checkpointing_file**: name of checkpointing file


Subtag **Segments**:   Wrapper for multiple segments tags

-   **profile_weight_away_from_interface**: XRW TO DO
-   **current_segment**: XRW TO DO


Subtag **segment**:   individual segment tag

-   **pdb_profile_match**: XRW TO DO
-   **profiles**: XRW TO DO

---