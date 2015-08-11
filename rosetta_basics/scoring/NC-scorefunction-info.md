#Scorefunctions that work well with protein and non-protein residues and molecules

With the addition of the [[talaris2013 scorefunction | score-types]] as the default Rosetta scorefunction, most non-protein residues and molecules can be scored, however other scorefunctions exist that work well with protein and non-protein residues and molecules.

[[_TOC_]]

## MM Standard Scorefunction <a name="MM-Standard-Scorefunction" />
Creator Names:
* P. Douglas Renfrew (doug.renfrew@gmail.com)
* Bonneau Lab

Description:
* Developed to score non-canonical alpha-amino acid side chains, and used in the generation of rotamer libraries for NCAA
* Currently being used to score peptoids, and oligo-oxypiperizines (OOPs), and hydogen-bond surrogets (HBSs)
* Removes knowledge-based terms that are evaluated based on residue type identity

* *  *fa_dun*, *rama*, *pair*, *ref*
* Adds a MM torsion and MM Lennard-Jones terms that are evaluated **intra**-residue

* *  *mm_intra_lj_rep*, *mm_intra_lj_atr*, *mm_twist*
* Adds an explicit unfolded state energy term based on pdb fragments

* *  *unfolded*

Preliminary Results:
* Testing done in [Renfrew et. al., PLOS ONE 2012](http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0032637)

* * rotamer libraries produced are comparable to dun02
* * rotamer recovery is comparable to score12
* * sequence recovery not as good as score12
* * 5 peptides were designed to target the calpain/calpastatin interface that had disassociation constants (Kd) less than or equal to the wild type peptide fragment
* Testing done in [Drew et. al., PLOS ONE 2013](http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0067051)
* * Designed ROSIE servers to work with non-canonical backbones (NCBBs)
* Used to design 9 OOPs against he P53/MDM2 interface (Kevin Drew, to be published soon)
* * 4 OOPs had nM Kd and 4 had uM Kd and 1 did not bind

Caveats:
* Has not received as much testing as score12
* No long-range electrostatics
* Formulation of unfolded energy under-estimates the attractive component of energy function which lead to the over incorporation of large side chains

How to use:
* Flags (assuming protocol respects command-line options)
<pre>
 -score::weights mm_std
</pre>
* Weight set
 <code>mm_std.wts</code>

Note that you might also want to provide a custom weights file that turns on the **ring_close** term, if you are working with cyclic noncanonicals (e.g. sugars).  See below for details on **ring_close**.

##Partial Covalent Interactions Energy Function (Orbitals)
Creator Names:
* Steven Combs (steven.combs1@gmail.com)
* Meiler Lab

Description:
* Developed to score salt bridges, cation-pi, pi-pi, and hbonds more accurately
* Not typed on residue types, but on orbital types associated with atoms. Allows for extensibility to ligands and non-canonical amino acids

Preliminary Results:
* Performs as well as talaris2013 in design.
* Improves geometry of saltbridges, cation-pi, pi-pi, and hbond interactions at the acceptor/donor interface
* Works for all residuetypes (ligands, DNA, RNA) because it is typed on orbitals
* Recapitulates average number of hbonds, salt bridges, cation-pi, pi-pi interactions better than talaris2013

Caveats: 
* Slightly slower than talaris2013
* Just changed the names of all the scoreterms so that they match those in the soon to be published paper
* Adjustment of individual terms (cation_pi, pi_pi, hbond, salt_bridge) is now easier by directly manipulating the respective term (pci_cation_pi, pci_pi_pi, pci_hbond, pci_salt_bridge)
* Still uses the hbond backbone-backbone scfxn to score backbone-backbone interactions

How to Use:
* Flags (assuming protocol respects command-line options)
<pre>
 -score::weights orbitals_talaris2013
 -add_orbitals
 -output_orbitals (optional. Outputs the orbitals in the pdb file)
</pre>
* Weight Set

 <code>orbitals.wts </code>(for pre-talaris2013 defaults), <code>orbitals_talaris2013.wts</code>, <code>orbitals_talaris2013_softrep.wts</code> (analogous to soft_rep/soft_rep_design scorefunction but not rigorously tested)

## General ring closure term (**ring_close**)
Creator: Vikram K. Mulligan (vmullig@uw.edu), Baker laboratory

Although not a full scorefunction itself, the **ring_close** score term is meant to be a generalized version of the **pro_close** term (which holds the proline ring closed during minimization).  Unlike **pro_close**, though, which is proline-specific, **ring_close** is intended to work with any canonical or noncanonical residue with a ring.  Since Rosetta thinks about molecules as a branching tree of atoms (the AtomTree), rings cannot be properly represented during minimization, meaning that there must be a cutpoint in any cyclic chain of atoms.  This could result in rings drifting open during minimization.  The **pro_close** term creates a harmonic potential between proline's delta carbon and a virtual atom ("shadow atom") attached to the mainchain nitrogen.  This holds the proline ring closed.  The **ring_close** score term does the same for any "shadow atom" and its real counterpart, on any residue type.

Shadow atoms are defined in the params file for a ResidueType with **VIRTUAL_SHADOW** lines.  Typically, one wants to have two such lines in order to enforce closure of a ring.  For example, in the cis-ACPC params file, we have:

```
VIRTUAL_SHADOW VCM CM
VIRTUAL_SHADOW VCD CD
```

The above lines indicate that the VCM virtual atom (which is attached to the CD atom that is part of the cis-ACPC ring) is expected to "shadow", or match the position of the mainchain CM atom, and the VCD virtual atom (which is attached to the mainchain CM atom) is expected to "shadow" the CD atom.  In order to enforce this, a scorefunction with the **ring_close** score term turned on must be used.

Note that there are three scoring terms that all enforce closure of proline: **ring_close**, **pro_close**, and **cart_bonded**.  To avoid double-counting, these functions should not be used together.  The **ring_close** term is intended for use in situations in which the minimizer can move only the torsional degrees of freedom, so that it would be unduly expensive to use the **cart_bonded** scoring term.

Note also that **pro_close** has two behaviours: in addition to enforcing ring closure of proline residues, it also imposes torsional constraints on the omega torsion angle of the preceding residue.  If one wishes to continue to use **pro_close** for the latter purpose, but have **ring_close** handle ring closure, you can disable the ring closure part of **pro_close** with the **-score:no_pro_close_ring_closure** flag.  The two score terms, **pro_close** and **ring_close** should <i>only</i> be used together if this flag is set.  If **ring_close** is given the same weighting as **pro_close**, it enforces closure with the same strength.  More commonly, though, **ring_close** is probably going to be used with the MM scoring function.

##See Also

* [[Scoring explained]]
* [[Score terms and score functions|score-types]]
* [[Additional score terms|score-types-additional]]
* [[Hydrogen bond energy term|hbonds]]
* [[Guides for non-protein inputs|non-protein-residues]]
* [[Scoring options|score-options]]