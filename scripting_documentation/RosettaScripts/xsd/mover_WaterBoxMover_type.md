<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
This mover cad solvate a pose in one of two ways: 1.) a uniform grid within a specified distance ("shell") of selected residues; 2.) based on statistics of where waters are found in high resolution crystal structures about polar side chains and backbone groups.

```xml
<WaterBoxMover name="(&string;)" remove_all="(false &bool;)"
        bb_sol="(false &bool;)" sc_sol="(false &bool;)"
        task_operations="(&task_operation_comma_separated_list;)"
        scorefxn="(&string;)" />
```

-   **remove_all**: XSD XRW TO DO
-   **bb_sol**: XSD XRW TO DO
-   **sc_sol**: XSD XRW TO DO
-   **task_operations**: A comma separated list of TaskOperations to use.
-   **scorefxn**: Name of score function to use

---