# DnaInterfacePacker
*Back to [[Mover|Movers-RosettaScripts]] page.*
## DnaInterfacePacker

```
<DnaInterfacePacker name=(&string) scorefxn=(&string) task_operations=(&string,&string,&string) binding=(0, &bool) base_only=(false, &bool) minimize=(0, &bool) probe_specificity=(0, &bool) reversion_scan=(false, &bool)/>
```

-   binding: calculate binding energy
-   base\_only: consider only interaction with the DNA bases
-   minimize: minimize protein side chains at the interface
-   probe\_specificity: calculate binding energy of designed protein for alternative DNA targets and calculate a specificity score
-   reversion\_scan: revert mutations that do not contribute to the specificity score
