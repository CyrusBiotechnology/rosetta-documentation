# SaveAndRetrieveSidechains
*Back to [[Mover|Movers-RosettaScripts]] page.*
## SaveAndRetrieveSidechains

To be used after an ala pose was built (and the design moves are done) to retrieve the sidechains from the input pose that were set to Ala by build\_Ala\_pose. OR, to be used inside mini to recover sidechains after switching residue typesets. By default, sidechains that are different than Ala will not be changed, **unless** allsc is true. Please note that naming your mover "SARS" is almost certainly bad luck and strongly discouraged.

    <SaveAndRetrieveSidechains name=(save_and_retrieve_sidechains &string) allsc=(0 &bool) task_operations=("" &string) two_steps=(0&bool) multi_use=(0&bool)/>

-   task\_operations: see [RepackMinimize](#RepackMinimize)
-   two\_steps: the first call to SARS only saves the sidechains, second call retrieves them. If this is false, the sidechains are saved at parse time.
-   multi\_use: If SaveAndRetrieveSidechains is used multiple times with two\_steps enabled throughout the xml protocol, multi\_use should be enabled. If not, the side chains saved the first time SaveAndRetrieveSidechains is called, will be retrieved for all the proceeding calls.


