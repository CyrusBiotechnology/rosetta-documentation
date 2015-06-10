# MonteCarloUtil

(This is a devel Mover and not available in released versions.)

This mover takes as input the name of a montecarlo object specified by the user, and calls the reset or recover\_low function on it.

```
<MonteCarloUtil name=(&string) mode=(&string) montecarlo=(&string)/>
```

-   mode: Mode of the monte carlo mover. can be either "reset" or "recover\_low"
-   montecarlo: the monte carlo object to act on


