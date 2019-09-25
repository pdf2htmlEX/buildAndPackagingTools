# Updating pdf2htmlEX to a new poppler release

This note collects the steps to take when updating pdf2htmlEX to a new 
poppler release.

The [poppler](https://poppler.freedesktop.org/) sources can be browsed/cloned at 
either:

* [cgit](http://cgit.freedesktop.org/poppler/poppler)

      git clone https://anongit.freedesktop.org/git/poppler/poppler.git

* [gitLab](https://gitlab.freedesktop.org/poppler/poppler)

      git clone https://gitlab.freedesktop.org/poppler/poppler.git

### Copy 3rdparty/poppler/git files

From the poppler sources you need to copy the six files

* CairoFontEngine.cc
* CairoFontEngine.h
* CairoOutputDev.cc
* CairoOutputDev.h
* CairoRescaleBox.cc
* CairoRescaleBox.h

into the pdf2htmlEX/3rdparty/poppler/git directory of a development fork.

To find the required files, in a local clone of the poppler sources, type 
the following:

    git tag

this will list the current poppler tags,

    git checkout <<your chosen tag>>

this will checkout the sources at the required poppler tag/release. Since 
the poppler sources change so fast, you will usually be checking out these 
files in a "detached" HEAD. Since we are not *developing* or making changes 
to the poppler source this is no problem.

You will find the six files listed above in the poppler subdirectory of the 
main poppler repository. This poppler subdirectory also contians most of 
the poppler source code. To list the "Cairo*" files, while in the poppler 
repository, type:

    ls poppler/Cairo*

To copy them into place type:

    cp poppler/Cairo* <<path to development pdf2htmlEX>>/3rdparty/poppler/git


### Review changes

At this point it is important to move to the pdf2htmlEX development 
repository and review the `git diff` to see what has changed between the 
last and current poppler releases. (This will hint what problems you might 
have compiling pdf2htmlEX).

### Renaming config.h files

Since we are essentially working with multiple packages (poppler, 
fontforge, and pdf2htmlEX) all in the same source tree, it is *very* 
important to keep their various "config.h" files distinct.

We do this by renaming them:

* poppler-config.h
* fontforge-config.h
* pdf2htmlEX-config.h

Unfortunately, the six files you have just copied from the poppler sources, 
all `#include <config.h>`. These must be changed to `#include 
<poppler-config.h>`.
