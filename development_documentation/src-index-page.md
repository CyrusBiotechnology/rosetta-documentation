<!-- --- title: Src Index Page -->An overview page for the src directory of the Rosetta

From a conceptual standpoint the src directory contains all the C++ code specfic to Rosetta. The directory structure within Rosetta delineates conceptual dependencies. A Rosetta developer should keep in mind the layered architecture depicted in the figure below. The arrows represent allowed dependency flows. Any code dependency on directories not accessible by following the arrows is not allowed. A strict vertical code dependency has been observed thus far in writing Rosetta code. Historically lateral independence within sub directories has not been required, however recent design decisions have identified this as a goal. Consequently, the figure below should be updated to display those dependencies as they are achieved.

[[/images/RosettaLib.png]]

Core Library
============

core\_dir\_index The core directory contains classes which manages most of the internal of Rosetta. This covers topics such as chemical representation of models, conformational representation of models, low level operations on conformations, energetical evaluation of models.

Readmes, Tutorials and Reference Documents for the Core Library
---------------------------------------------------------------

```
- kinematics
    - @ref atomtree_overview "AtomTree overview and concepts"
    - @ref foldtree_overview "FoldTree overview and concepts"
- optimization
    - @ref minimization_overview "Minimization overview and concepts"
- pack
    - @ref packer_task "How to use the PackerTask"
    - @ref resfiles "Resfile syntax and conventions"
    - @ref resfile_reader "How to write new resfile commands"
- scoring
    - @ref new_energy_method "How to add a new scoring term"
    - @ref constraint_file  "Constraints file formats"
- util
    - @ref tracer "Tracer, tool for debug IO"
```

Numeric Library
===============

[[Numeric Library|namespace-numeric]]

Utility Libary
==============

[[Utility Library|namespace-utility]] The utility directory contains utility classes such as [[utility::vector1|vector1]] ( a base 1 indexed of child class of std::vector with a few extra additions), izstream a infilestream which allows reading of zipped files.

Protocols Library
=================

protocols\_index\_page The protocols directory contains higher level code which makes use of the core directory components to accomplish modelings tasks. The code in this direct should be in a state such that it is useable by a general audience (Please provide adequate documentation for all code placed in this directory). One of the main base classes to be aware of is the Mover class. This classes is the interface class which most Rosetta developers inherit from when creating new modeling protocols. Most developers will interface with the Rosetta code on at this level. Unless they need to extend the model representation capabilities of Rosetta.

Development Library
===================

devel\_index\_page The devel directory is a staging area for protocols. Essential code in this area is not considered mature. Standards of documentation are lax (Note though that documenting code after the fact is much more difficult. So it is HIGHLY recommended that a developer provide good documentation even for code that is flucuating rapidly. It will make your life easier and more productive and your colleague will appreciate your throughness) Code in this region is not considered part of the Rosetta release.

Adding New Directories in "top level" of src
============================================

New directories in src "top level" of src should only be added after consulting with the general community. New directories indicate the design of the suite of libraries is insufficient. Design decisions affect the entire community. Hence, input from the community should be solicited. In once a decision has been reached added a new section to this page explaining the place of the new directory in relation to the rest of the code. Added a new Doxygen page to act as an index page to explain the structuring the subdirectories with the new "top level" src directory. See this Template Doxygen Page for New Category of Source Files as an example (Note you can copy the template index page as starting point and fill in the relevant fields it is located at doc/template\_doxygen\_pages/TemplateIndexPage.dox.

Adding New Subdirectories
=========================

New subdirectories in one of the above existing directories can be added much more readily without damaging the overall structure of the library. First determine which level of the library the new concepts fit into. If one is unsure of where new code should be placed, consult experienced Rosetta developers. This can either be in one's lab or even better send a message out to the develop mailing list (One might find out that some else has a complimentary idea with a starting point already available). Once it has been decide where the new directory should be placed and named, create a new Template page for Subdirectories of Libraries for that directory. A template Doxygen page is located at doc/template\_doxygen\_pages/DirectoryTemplatePage.dox. You copy over the template page and fill in the fields. The Doxygen page should explain what the code found in the directory can do, any expected lateral dependencies ( dependencies on code in sibiling sub directories), and the names of authors contributing to the directory.
