Build process and Installation
===============================

Build with Bash
^^^^^^^^^^^^^^^

When you have bash installed you just have to have to run the script "build.sh" from the repository. This script will search for a compiler, linker and java. With this informations a Makefile get's generated and executed.

Build without Bash
^^^^^^^^^^^^^^^^^^

When you don't have a bash installed you have to copy the file "Makefile.rh4n" to "Makefile" and edit his content.

Replace the value of following make variable:

{{CC}}
------

This is the path to your c compiler. XLC or GCC.

{{AR}}
------

This is the path to your ar binary.

{{CARGS1}}
----------

This are agruments for the compiler wich must be set before any source files.

Most likely for GCC: -c -g -fPIC

Most likely for GCC: -c -g -fpic


{{CARGS2}}
----------

This are agruments for the compiler after specifed the outputfile
Currently this can be set to a empty str









When you don't want to build the framework yourself every version since 2.0 is avalible as a precompiled binary on the `release page<https://github.com/audacity363/realHTML4Natural/releases/>`_ over at GitHub.

But when you want to build it yourself it is very easy:

There are two types of Makefiles for the different make programs: 

- IBMs make
- GNUs make

Every modul have a Makefile for this "make" types. When you want do compile the complete framework you can use the Makefile in the root directory.

When you don't want do build it on your system there are docker files for creating docker images with a gcc compiler.


Installation
^^^^^^^^^^^^

There is no real installation but there is just a core binaries to copy:

- ./natuser_lib/librealHTML4Natural.so

This is the shared object which provides the INTERFACE 4 functions "genpage" and "flipbuf". It must be mentioned in your NATUSER Env-var. 
