---
title: Files and directories
---

### Brief History

In April/May 2012, STEP Class Library was renamed to STEPcode. This was
done because the old name wasn't accurate - the class libraries are only
a fraction of the software.

The STEP Class Library (SCL) originated at the National Institute of
Standards and Technology, or NIST. NIST started working with STEP in the
80's and continued until the late 90's. Some components of SCL were
originally written in Lisp and then re-written in mixed C and C++ in the
early 90's. The rest of SCL was written in C++ to begin with.

### What is *in* STEPcode?

STEPcode (SC) includes the class libraries, some of the most widely used
EXPRESS schemas, some tools to work with EXPRESS, and support libraries
for those tools. Two of the tools can create schema-specific libraries
that are used with the class libraries. There are also some test files
and programs.

#### Schema-specific libraries

The schema-specific libraries handle specialized data structures
described in the schema, allowing bi-directional translation between an
in-memory representation of the data and one suitable for serialization.
The in-memory representation stores the data in SDAI objects (below).

#### Class libraries

The class libraries (under src/, directories clutils, cldai, cleditor,
clstepcore) are runtime libraries - that is, they are used by an
application that wants to read or write STEP data.

-   **cldai** implements many of the classes for SDAI (ISO 10303-22),
    the Standard Data Access Interface. Any classes not implemented in
    cldai are implemented in clstepcore (below).
-   **cleditor** implements read and write for ASCII files that conform
    to ISO 10303-21, commonly referred to as Part 21 files or Exchange
    files.
-   **clstepcore** contains a large number of classes used by cldai,
    cleditor, and the schema-specific libraries.
-   **clutils** contains various generic utility functions.

#### Tools

The tools are fedex\_plus, fedex\_python, check\_express, and exppp.

-   **check\_express** uses libexpress (below) to check an EXPRESS
    schema for errors. Formerly named **express**, it is the simplest
    possible use of libexpress.
-   **fedex\_plus** parses an EXPRESS schema using libexpress, analyzes
    the in-memory representation of the EXPRESS, and writes
    schema-specific C++ source code that is the compiled into one of the
    aforementioned schema-specific libraries.
-   **fedex\_python** is a work in progress. It is similar to
    fedex\_plus but writes Python code instead.
-   **exppp**, the EXPRESS Pretty Printer, formats EXPRESS in a way that
    is easy to read. The executable is currently not built because it is
    rarely used, but the library is still built and is used by
    fedex\_plus and fedex\_python.

#### Other libraries

The following libraries are used prior to runtime, in the process of
generating source code for the schema-specific libraries. **base** is
also used at runtime, along with a few hashing functions in
**libexpress**.

-   **base** currently only contains memory management functions -
    implementations of malloc, calloc, realloc, free, new, and delete
    that warn of improper use and record the file and line number.
-   **libexppp** formats EXPRESS so that it is easy to read. It is used
    to format some things that fedex\_plus and fedex\_python do not
    currently translate into C++ or Python, such as FUNCTIONs and RULEs.
-   **libexpress** parses an EXPRESS schema in a file and builds a model
    of the schema in memory.

#### EXPRESS schemas

Standard (and draft standard) schemas are located in subdirectories
under **data/**. Part 21 files that are known to conform (if any) are
located in those subdirectories. See the README in that directory for
details.

#### Tests

-   **p21read** is the most commonly used test program. When the
    executable is built, it is linked to a schema-specific library. When
    invoked, it reads a part 21 file specified on the command line. Once
    the data is loaded in memory, it is written to another file.
    Currently, we only check that p21read reports success. It would be
    best to compare the input and output data, but this is complicated
    by the fact that variations are allowed in the form of whitespace
    and comments, and entities may be reorderd.
-   **src/test** contains the p21read source as well as some other
    example programs that are not currently utilized.
-   When CMake is configured with testing enabled, any Part 21 files
    found in the same directory as a schema that is to be built are
    automatically used for tests with p21read.
-   **test/** contains all other test files and scripts
