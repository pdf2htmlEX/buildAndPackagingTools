# pdf2htmlEX patches

This directory contains a dated collection of patches to the pdf2htmlEX 
source code.

Each patch was created on a development AWS EC2 machine and represent the 
changes required to get pdf2htmlEX to compile with a given version of 
poppler.

These patches were created using the `git diff > patchFile` command and 
then pulled back to the working server using the `getPatchFile0` command.

They can be applied to the pdf2htmlEX source code using the command:

    patch -p1 < <<path to the patch file>>

while in the root of the pdf2htmlEX repository.
