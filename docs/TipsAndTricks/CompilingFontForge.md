# Compiling FontForge from git

For some reason the 20170731 version of FontForge, as pulled from the git 
archive as well as debian sources, does not compile "out of the box".

The <fontforge-config.h> header MUST be included before the <math.h> header.

In particular the files:

    gdraw/drawboxborder.c
    gdraw/gpsdraw.c

require alteration to allow the system to compile.
