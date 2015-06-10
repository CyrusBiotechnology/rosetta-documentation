# Prepack
## Prepack

Performs something approximating r++ prepacking (but less rigorously without rotamer-trial minimization) by doing sc minimization and repacking. Separates chains based on jump\_num, does prepacking, then reforms the complex. If jump\_num=0, then it will NOT separate chains at all.

```
<Prepack name=(&string) scorefxn=(score12 &string) jump_number=(1 &integer) task_operations=(comma-delimited list) min_bb=(0 &bool)/>
  <MoveMap>
  ...
  </MoveMap>
</Prepack>
```

-   min\_bb: minimize backbone in the bound state, before separating the partners. This option activates MoveMap parsing.
-   MoveMap: just like in FastRelax and MinMover, but is only activated if min\_bb is set to true.


