#StepWiseSampler
`StepWiseSampler` objects are concatenated together to define the sampling loop, and can go through millions of poses.

#How to use

Codes in the folder `src/protocols/stepwise/StepWiseSampler` are for the rotamer sampler class and its subclass,
which is subclass of moves::Mover.

Folder organization
-------------------
The root path contains generic abstract base classes and simple base
classes for inheritence and basic functionality.

Some classes take multiple rotamer sampler instances and assemble them in
linear of combinatorial way for complex tasks (e.g. RotamerComb,
RotamerSizedAny).

The `rna/` folder contains RNA specific Rotamer sampler for backbone, sugar
ring and chi angle.

How to use
----------
Rotamer samplers apply new rotamer conformation to a pose until all rotamer
has been navigated.

Example code:
```
RNA_SuiteRotamer sampler( ... ); //Declaration
//Various settings
sampler.set_fast( true );
sampler.set_random( false ); //Controls random sampling
//NEVER FORGET to initialize after setting
sampler.init();

//For non-random sampling (sampler ends at at finite # of steps)
for ( sampler.reset(); sampler.not_end(); ++sampler ) {
  sampler.apply( pose ); //Apply rotamer to pose
  ...
}

//For random sampling (sampler never ends)
sampler.set_random( true );
sampler.reset();
Size const max_tries = 10000;
for ( Size i = 1; i <= max_tries; ++i, ++sampler ) {
  sampler.apply( pose ); //Apply rotamer to pose
  ...
}
```


Go back to [[StepWiseSampleAndScreen|stepwise-sample-and-screen]].

Go all the way back to [[StepWise Overview|stepwise-classes-overview]].