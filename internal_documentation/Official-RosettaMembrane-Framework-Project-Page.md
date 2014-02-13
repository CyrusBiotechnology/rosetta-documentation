This page is the Official development page for the membrane protein framework. Its intention is not to be documentation for usage but as a list of ongoing tasks for the developers. 

## About the Project

### Developers
- Rebecca Alford ([rfalford12@gmail.com](rfalford12@gmail.com))
- Advised by Julia Koehler Leman ([julia.koehler1982@gmail.com](julia.koehler1982@gmail.com))

Last Modified: 2/12/2013

### Motivation
Rosetta is by default, oriented for modeling soluble proteins. To date, there are few applications which use membrane scoring (Yarov-Yaravoy et al. 2006); notably membrane _ab initio_, relax, and comparative modeling. However, because Rosetta does not account for conformational aspects specific to membrane-bound proteins, it is challenging to extend and develop new modeling protocols that can account for these differences. 

The goal of the new RosettaMembrane is to provide a generalized framework for modeling the conformational and chemical characteristics of membrane proteins. By providing a more accurate representation of membrane protein conformation, we hope to provide a new platform for improved development of membrane-adapted Rosetta modeling protocols. Furthermore, by adapting the existing protocols to Rosetta 3 and designing an object-oriented framework, we hope that the design promotes evolvability within the Rosetta libraries. 

This documentation describes the current progress in development of the membrane protein framework, coding conventions/guidelines, current tasks, and future tasks. In essence, this should serve as the developer's guide to using and developing RosettaMembrane Framework based protocols. 

### Design Objectives
* Centralized development for membrane proteins
* Fully object-oriented code base (Rosetta3)
* Support for multiple membrane chains
* Support for hybrid systems - some membrane and some non-membrane chains
* User intuitive application development and API
* Effective representation of membrane protein conformation and scoring integration
* Resource manager supported, jD2 supported
* Can easily extend to additional features: symmetry, ligand-docking, protein-protein docking

### Software Tasks (Still TODO)
1. Load membrane proteins as starting structure with JD2
2. Test with PyRosetta
3. Support non-membrane spanning chains
4. Test with RosettaScripts

## Code and Demo
The code for implementing membrane proteins lives in src/core/membrane. Integration tests and maybe a demo coming soon. 

## Tools
Tools for setting up larger runs and configuring JD2 jobs with the membrane framework can be found in Rosetta/tools/membrane_tools

## Usage

### Calling the Framework
Currently, the membrane protein framework overrides the prose provided by JD2. Membrane protocols should be called within a generic membrane mover template which generates a membrane protein and treats it as the starting structure. Syntax below: 

```
MembraneProteinFactoryOP mpf = new MembraneProteinFactory( fullatom, membrane_chains, include_lips );
PoseOP pose = mpf->create_membrane_pose();
protocol
```

## Dependencies in Rosetta
* Resource Manager
* Rosetta membrane scoring function (current)

## Project FAQ