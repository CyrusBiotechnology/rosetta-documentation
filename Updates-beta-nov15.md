### Using the beta_nov15 energy function parameters

For most protocols (those that use _getScoreFunction_ to set the protocol score function), the flag **-beta_nov15** _or_ simply **-beta** (which will always load the latest beta energy function) will load this version of the beta energy function.

For _RosettaScripts_ protocols, the flag **-beta_nov15** _or_ **-beta** must be provided, and the following scorefunction declaration must be made:

**\<beta weights=beta_nov15/\>** _or_ **\<beta weights=beta/\>**

### Optimized parameters 

Optimization followed the same scheme as [[beta_july15|Updates-beta-july15]], and used that function as a starting point.  However, in addition to optimizing several new parameters, modifications were made to the target function used during optimization:

* New docking and decoy discrimination sets were generated using beta_july15
* The distance distribution metric was modified to compute RMS error rather than error sum, and to weaken over-represented atom pairs, to address poor polar distance distributions in the July 2015 version

**Electrostatics & Partial charges (fa_elec)**

In this optimization we considered optimizing individual atomic partial charges.  To reduce the number of parameters as well as maintain the overall charge of the molecules, we used the following scheme:
* Break each residue in 2 backbone and 1 sidechain group
* Maintain the overall charge of that group (-1/0/+1) but fit a group scalefactor that increases the overall range of partial charges within that group
* Group together all backbone groups (so there is only one NH group parameter and one CO group parameter) and all non-polar, non-aromatic sidechains

More importantly, an issue where the count-pair logic in Rosetta was splitting dipoles has been corrected.  This is fixed by specifying a "representative atom" for each atom that is used for countpair purposes.  This is specified using the flag **-elec_representative_cp** or **-elec_representative_cp_flip** which uses the minimum or maximum length within each group, respectively, when determining what count pair multiplier to apply.  The beta energy function uses the maximum length (**-elec_representative_cp_flip**).

**Hatr (fa_atr for hydrogens)**

Attractive energies were enabled for hydrogen atoms (**-fa_Hatr**), and LJ parameters were refit.  Due to the significant changes this induces in the LJ parameters, this refitting was initially guided entirely by agreement to biophysical data using small molecule liquid simulations, then using the complete target function.

**Torsion library updates (fa_dun/rama_prepro/p_aa_pp)**

Several torsional updates were included before optimization:
* A new computed and smoothed version of the fa_dun and p_aa_pp energies was provided by Maxim Shapovalov and Roland Dunbrack
* Separate pre-proline and pre-non-proline rama potentials were computed and are now used, replacing **rama** with a new scoreterm **rama_prepro**
* Weights on all torsional terms were refit as part of the optimization process

**Miscellaneous**

Several minor modifications were made to the energy function as well:

* The fade-width of LK-ball solvation was reduced slightly
* Reference weights were again refit