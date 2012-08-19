---
title: How to test with CTest
---

Without ctest script
--------------------

**Preferred method**

-   configure with SCL\_ENABLE\_TESTING=ON, i.e.

`   cmake .. -DSCL_ENABLE_TESTING=ON`

-   build:

`   make`

-   run tests:

`   make test`

-   results will look like

` Running tests...`  
` Test project /opt/step/test-scl/build_ctest`  
`         Start   1: generate_cpp_ap239_arm_lf`  
`   1/137 Test   #1: generate_cpp_ap239_arm_lf ........................................   Passed    3.55 sec`  
`         Start   2: build_cpp_sdai_ap239_arm_lf`  
`   2/137 Test   #2: build_cpp_sdai_ap239_arm_lf ......................................   Passed   84.97 sec`

and so on.

Via ctest script
----------------

**Note:** this is not recommended, as results are not reported unless
result submission is enabled and you look at
[my.cdash.org](http://my.cdash.org/index.php?project=StepClassLibrary)
after running the tests. From the **STEPcode/** dir, run CTest:

`   ctest -S run_ctest.cmake`

It will warn you that .SCL\_CTEST\_PREFS.cmake is missing. This is
normal, unless you are set up to submit test results to my.cdash.org.
Free my.cdash.org accounts have limits on the number of people that are
allowed to submit tests, so please discuss on the mailing list before
you create the file.