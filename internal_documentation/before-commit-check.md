<!-- --- title: Before Commit Check -->List of things you should check in your code before committing it in to svn

Overview
========

This is Guideline reflect our current policy for core/protocols/devel libraries and unit test code. Make sure you check thing below before committing code in to svn.

Source code

-   It is expected that submitted code will be at least briefly documented. Use 'brief' doxygen comment tag for .hh files and 'details' tag for .cc file code documentation.
-   In general you should not use C++ standard 'float' type, - use core::Real instead.
-   Do not commit code containing direct output to std::cout/cerr streams in to core/protocols/devel libraries or unit test. Use core::util::Tracer instead.
    -   Unless you planing to terminate program abnormally (i.e utility\_exit(...)) because of the hard error. In that case you can can use cerr output to provide additional information for your error messages.

-   Use tabs rather than spaces for indentation and remove trailing whitespace. Tools for automating this are available from svn [https://svn.rosettacommons.org/source/branches/mini\_tools](https://svn.rosettacommons.org/source/branches/mini_tools) . Also make sure you follow the other [Coding Conventions](https://wiki.rosettacommons.org/index.php/Coding_Convention_and_Examples) such as removing "using" statements from header files.

Building

-   Test your build before committing it. (Replace '-j8' with number appropriate for your system, i.e. number of processors).
    -   Test library build. Execute 'scons -j8'.
    -   Test Unit test build is compiling. Execute 'scons -j8 cat=test' and make sure everything is compiling.
    -   Test bin and pilot\_apps\_all build. Execute 'scons -j8 bin pilot\_apps\_all'.

-   Make sure that everything is compiling and your modification did not produce any compilation warnings.

Unit test

-   Run unit test *before* committing any code, and make sure that your modifications did not break any tests.
    -   Run unit test. Execute 'python test/run.py -j8 –database \<path to DB\> –mute all' ('-j8' as above, optional. Tests must be run from the top-level directory.)

-   If your changes break tests - fix them after insuring that results are correct. If you can't fix some of the test/not sure if results correct - contact developer responsible for this test and ask for help.
-   (See also [[Different way to run unit tests|run-unit-test]] .)

Integration Test

-   Run the integration tests *before* committing any code. To run the integration tests. ('-j8' as above, optional.)

cd test/integration/

./integration.py -h

./integration.py -j8 -d \$MINIROSETTA\_DATABASE

Note that your code should be compared to the results of running the integration tests on the same revision of the code without your changes. As with unit tests consults other developers if the results of the tests don't match.

After commit

-   Check out 'clean' version of mini in to folder separate from your main working folder. And make sure it compile and run. This step especially important if you add new files, it help insure that all files present in to svn and nothing are forgotten.

Additional information about our current coding guideline can be found at Rosetta wiki at: [https://wiki.rosettacommons.org/index.php/Coding\_Convention\_and\_Examples](https://wiki.rosettacommons.org/index.php/Coding_Convention_and_Examples)

Information on Commenting Guidelines: [https://www.rosettacommons.org/internal/doc/Rosetta++CommentingGuidelines.html](https://www.rosettacommons.org/internal/doc/Rosetta++CommentingGuidelines.html)
