# ResetBaseline
Use this mover to call the reset_baseline method in filters Operator and CompoundStatement. Monte Carlo mover takes care of
resetting independently, so no need to reset if you use MC.

```
<ResetBaseline name=(&string) filter=(&filter)/>
```
- filter: the name of the Operator or CompoundStatement filter.


