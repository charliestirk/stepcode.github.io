---
title: Minimal Part 21 C++ example
---

Minimal C++ example based on [P21read](P21read "wikilink") - note, this
hasn't been tested!

Important Classes
-----------------

-   [Registry](http://stepcode.org/doxygen/class_registry.html)
    (contains information about types present in the current schema)
-   [InstMgr](http://stepcode.org/doxygen/class_inst_mgr.html) (holds
    instances that have been created or that have been loaded from a
    file)
-   [STEPfile](http://stepcode.org/doxygen/class_s_t_e_pfile.html)
    (takes care of reading and writing Part 21 files, and creates an
    SDAI\_Application\_instance for every instance read)
-   [SDAI\_Application\_instance](http://stepcode.org/doxygen/class_s_d_a_i___application__instance.html)
    (the base class for every type of instance that STEP deals with)

Code
----

`#include <scl_cf.h>
extern void SchemaInit( class Registry & );
#include <STEPfile.h>
#include <sdai.h>
#include <STEPattribute.h>
#include <ExpDict.h>
#include <Registry.h>
#include <errordesc.h>
#ifdef HAVE_UNISTD_H
# include <unistd.h>
#endif

int main( int argc, char * argv[] ) {

    // the registry contains information about types present in the current schema; SchemaInit is a function in the schema-specific SDAI library
    Registry  registry( SchemaInit );

    //the InstMgr holds instances that have been created or that have been loaded from a file
    InstMgr   instance_list;

    // STEPfile takes care of reading and writing Part 21 files
    STEPfile  sfile( registry, instance_list, "", false );

    // read a file, using the name from the command line
    sfile.ReadExchangeFile( argv[1] );

    // check for errors; exit if they are particularly severe
    if( sfile.Error().severity() < SEVERITY_USERMSG ) {
        sfile.Error().PrintContents( cout );
    }
    if ( sfile.Error().severity() <= SEVERITY_INCOMPLETE ) {
        exit(1);
    }

    /**************************************************
    ** do something with the data here
    ***************************************************/

    // write to "file.out", then check for write errors. The write operation overwrites any errors caused by previous operations.
    sfile.WriteExchangeFile( "file.out" );
    if( sfile.Error().severity() < SEVERITY_USERMSG ) {
        sfile.Error().PrintContents( cout );
    }
}`

Related Pages
-------------

-   [How to use STEPcode in an
    application](How to use STEPcode in an application "wikilink")

[Category:Code discussion](Category:Code discussion "wikilink")