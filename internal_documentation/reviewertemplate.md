<!-- --- title: Reviewertemplate -->Reviewer's Template

 Author   
Andrew Leaver-Fay

Metadata
========

Last edited by Andrew Leaver-Fay on April 6th, 2010.

Expectations
============

What as a reviewer are you expected to verify? There are three aspects. The easiest aspect both to describe and to perform is to determine whether the documentation conforms to the template. You should also review the documentation for poor grammar, incorrect spelling, or unclear phrases. The final aspect, the one hardest to describe and the one that requires more effort on your part, is to determine whether the documentation presents clearly the purpose and interface for a particular application.

-   Structural verification: Please note if any section of the documentation template is missing or if the purpose of that section is not met. It is OK if the documentation includes additional sections, but it is not OK if the sections in the template are missing. Please refer to the [[Documentation for AppName application|app-name]] template for the list of all required sections and their expected contents.
-   Grammatical verification: Are there sentence fragments? Does the author trail off mid thought? Do not hold back on correcting the writing; the clearer the writing, the better the documentation. A fresh pair of eyes can find mistakes that the original author is unable to see.
-   Content verification:
    -   Is the purpose of the application clear?
    -   Are its limitations obvious?
    -   If it is part of a pipeline, are the other parts of the pipeline referred to?
    -   Are concepts introduced before they are used?
    -   Are the effects of including or excluding relevant flags clear?
    -   Does the author describe which flags should usually be on, and why?
    -   Are the customizable features of the protocol highlighted, e.g. if the user wants to provide a new score file or set of score files?
    -   If there are various ways to control the extent of sampling (e.g. sidechain rotamers, backbone flexibility, loop modeling, local refinement vs full scale docking), are they described thuroughly? Does the author describe the recommended extent of sampling? Does the author describe when greater or lesser sampling is required?
    -   Is it clear how much CPU time would be expected to finish a typical task, e.g. 100 CPU hours for a reliable docking funnel?
    -   Are all relevent file formats described or linked to using the ATref command?
    -   Are links provided to other relevant applications or modes of functionality, e.g. ligand parameter file definition?
    -   Are the references complete?
    -   Do the "try me" examples work? (You are expected to run them.)
    -   Are the outputs from the try me examples described clearly? Do the outputs make sense?
    -   Has the documenter provided a set of expected outputs (e.g. a score file from a large abrelax run) and described how the outputs are to be judged (e.g. gnuplot commands to generate score vs rms plots)? Are expected post-processing steps described thuroughly (e.g. clustering?)
    -   Code references: are there links to the major classes and their documentation? Please verify that the dox page for the main classes is decently thorough; you are not expected to review the class documentation, but rather to verify that the class documentation exists.

Finished Product of the Review
==============================

Your review should include the following three sections in the following order:

-   Up/Down ruling on whether the documentation is complete. Only give an Up ruling if there are neither structural deficiencies nor content problems. If the only problems are spelling or grammar, than an Up ruling can be given.
-   Major comments. Are there sections that are unclear, or absent?
-   Minor comments. Nitpick. Add as much detail here as you can; your comments are exceedingly helpful.

Please send your review, and how much time your review took, to the editor and to your PI so that your PI will know how much work their lab has invested in the review process. The editor will forward your review along to the documenter. You may be contacted a subsequent time to finish the review for this application, or a subsequent round of review may be sent to another person in another lab.
