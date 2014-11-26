## PerturbBundle

This mover operates on a pose generated with the MakeBundle mover.  It perturbs (<i>i.e.</i> adds a small, random value to) one or more Crick parameters, then alters the backbone conformation to reflect the altered Crick parameters.  This is useful for iterative Monte Carlo searches of Crick parameter space.

```
<PerturbBundle name=(&string) default_perturbation_type=(&string)
     r0_perturbation=(&real) r0_perturbation_type=(&string)
     omega0_perturbation=(&real) omega0_perturbation_type=(&string)
     delta_omega0_perturbation=(&real) delta_omega0_perturbation_type=(&string)
     delta_omega1_perturbation=(&real) delta_omega1_perturbation_type=(&string)
     delta_t_perturbation=(&real) delta_t_perturbation_type=(&string) >
          <Helix helix_index=(&int)
               r0_perturbation=(&real) r0_perturbation_type=(&string) r0_copies_helix=(&int)
               omega0_perturbation=(&real) omega0_perturbation_type=(&string) omega0_copies_helix=(&int)
               delta_omega0_perturbation=(&real) delta_omega0_perturbation_type=(&string) delta_omega0_copies_helix=(&int)
               delta_omega1_perturbation=(&real) delta_omega1_perturbation_type=(&string) delta_omega1_copies_helix=(&int)
               delta_t_perturbation=(&real) delta_t_perturbation_type=(&string) delta_t_copies_helix=(&int) />
</PerturbBundle>
```

Default options for all helices are set in the <b>PerturbBundle</b> tag.  A default perturber type for all perturbations can be set with the <b>default_perturbation_type</b> option; currently-accepted values are "gaussian" and "uniform".  These can be overridden on a parameter-by-parameter basis with individual <b>*_perturbation_type</b> options.  Default perturbation magnitudes are set with <b>*_perturbation</b> options.  If an option is omitted, that Crick parameter is not perturbed.  These can also be overridden on a helix-by-helix basis by adding <b>Helix</b> sub-tags.  Perturbable Crick parameters include:

<b>r0</b>: The major helix radius.<br/>
<b>omega0</b>: The major helix twist per residue.<br/>
<b>delta_omega0</b>: The radial offset about the major helix axis.<br/>
<b>delta_omega1</b>: The rotation of the minor helix about the minor helix axis.<br/>
<b>delta_t</b>: A value to offset the helix by a certain number of amino acid residues along its direction of propagation.<br/>