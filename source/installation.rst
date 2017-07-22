Build process and Installation
===============================

Build
^^^^^
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
