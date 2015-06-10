# Rotate
## Rotate

```
<Rotate name="&string" chain="&string" distribution=[uniform|gaussian] degrees=(&int) cycles=(&int)/>
```

The Rotate mover is for performing a course random rotation throughout all rotational degrees of freedom. Usually 360 is chosen for "degrees" and 1000 is chosen for "cycles". Rotate accumulates poses that pass an attractive and repulsive filter, and are different enough from each other (based on an RMSD filter). From this collection of diverse poses, 1 pose is chosen at random. "cycles" represents the maximum \# of attempts to find diverse poses with acceptable attractive and repulsive scores. If a sufficient \# of poses are accumulated early on, less rotations then specified by "cycles" will occur. This mover uses an attractive-repulsive grid for lightning fast score lookup.


