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
<poppler-config.h>`. The actual `poppler-config.h` file will be created by
the poppler build system when you (re)compile poppler. (Alternatively, it 
can be found in the `libpopper-private-dev` debian package if you are using
the `libpoppler` package which comes with a Debian/Ubuntu release).

### Testing the build of pdf2htmlEX

Unfortunately, even if the six `Cairo*` files listed above have not changed,
other files in the poppler libraries might have changed...

This means that the only way to know that the current pdf2htmlEX sources will
compile with a given poppler version is to create an AWS EC2 instance
and test compile/install both the newer version of poppler as well as the 
associated version of pdf2htmlEX.

To do this you can follow the instructions contained in the
[building/Readme](../building/Readme.md).

Once you have an AWS EC2 instance running and configured with one of the
[ec2-install-pdf2html-develop-XXX.yml](../building/ec2-install-pdf2html-develop-stg.yml)
ansible playbooks, log into your server via ssh and then type:

    ./dobuildPoppler
    ./doinstallPoppler
    cd pdf2html
    ./dobuild

And then look at the error messages... (easy eh?)

(**NOTE** before running one of the playbooks, you need to make sure
that all of the correct poppler and pdf2htmlEX versions are correctly
recorded in the playbook you plan to use.)

### Hacking the pdf2htmlEX sources

Once you have compilation error messages, you can hack the pdf2htmlEX 
source code on the AWS EC2 machine until you get it to compile...

The
[`TipsAndTricks`](https://github.com/pdf2htmlEX/buildAndPackagingTools/tree/master/docs/TipsAndTricks)
directory of the `docs` directory of this repository has a number of
tips and tricks that have been used to identify the changes required
to get pdf2htmlEX compiling with a new version of poppler.

### Getting the changes back to your working repository

While you do have a clone of your development pdf2htmlEX git repository,
getting any changes back to your main working machine takes some care.
Since the AWS EC2 test-compile machine is not very secure, you **do not**
want to place your GitHub keys on the test-compile machine.

To transfer any changes I have made to the pdf2htmlEX source code
I do a `git diff > patchFile` to create a patch file which I then
pull back from the AWS EC2 machine to where my working repository is.
I can then use the `splitpatch patchFile` tool to split the patchFile
into patches for each individual file, review the changes and then 
apply them (if needed) to the files using the `patch` tool.

### Going round the loop

The process of getting stable changes to the pdf2htmlEX source code
*will* take a number of iterations, many of which will include the
creation of fresh new clean AWS EC2 machines with the current best
working version of the pdf2htmlEX source code.

### Once you have compiling code

Once you have code that compiles it is now time to test pdf2htmlEX
with some pdf's. To do this type:

    cd
    pdf2htmlTest testrevelatornl
    pdf2htmlTest joyLoL

If these both work without errors, you can then view the resulting
html using your local browser by typing:

    openHTML0

on your local machine.

The `testrevelatornl.pdf` file is a very simple single paragraph of
text. The `joyLoL.pdf` file is a very much more complex document with
a table of contents, citation links to the bibliography, as well as
a number of SVG based box diagrams (which include embedded text which
*should* be selectable).

If this all works then you can progress to the creation of a Debian package.

If something fails.... you need to go around to loop a couple more times.

### Creating a Debian package.

Once you have working pdf2htmlEX code, you need to issue a pull request
on the pdf2htmlEX/phd2htmlEX repository to get your code into the master
repository.

Once your changes have been accepted, you need to build a Debian package.

**Before you begin** you *must* update the pdf2htmlEX version number in the
[CMakeLists.txt](https://github.com/pdf2htmlEX/pdf2htmlEX/blob/master/CMakeLists.txt)
file in the master pdf2htmlEX repository. You need to update line 13:

    set(PDF2HTMLEX_VERSION "0.18.0")

At the moment we are essentially using a [Semantic Versioning](https://semver.org/) system:
* We are using the *Major* versions for substantial changes to pdf2htmlEX.
* We are using the *Minor* versions to reflect changes in the underlying
  Ubuntu/Debian releases.
* We are using the *Patch* versions to reflect changes in the poppler releases
  which might have happened *between* Ubuntu/Debian releases.

**Before you begin** you *must* also update the name of the ubuntu release in the
[build_dists.py](https://github.com/pdf2htmlEX/buildAndPackagingTools/blob/master/packaging/build_dists.py)
file in the pdf2htmlEX-buildAndPackagingTools repository. You need to update line 22:

    supported_distributions=('disco',)

Once you have updated these two version variables, you will then need
to create an new fresh clean AWS EC2 machine with the 
source code from the *master* pdf2htmlEX repository.

To do this you can follow the instructions contained in the
[building/Readme](../building/Readme.md).

Once you have an AWS EC2 instance running and configured with one of the
[ec2-install-pdf2html-master.yml](../building/ec2-install-pdf2html-master.yml)
ansible playbook, log into your server via ssh and then type:
    
    ./dobuildPoppler
    ./doinstallPoppler
    
    equivs-build libpoppler-dev
    sudo apt install ./libpoppler-dev_1.0_all.deb
    
    cd pdf2html
    ./build_dist.py

The `build_dist.py` script will ask you to fill in the changeslog file
using an editor of your choice. It will then go on and build the Debian package.

You can use the `getDeb0 <<debian package file name>>` command to pull
the debian package archive back to your working machine putting it in the
`tmp` directory. The `getDeb0` tool will also create `md4sum` and `sha256sum`
files which contain `md5sum` and `sha256sum` fingerprints of the debian
archive. (All in the `tmp` directory).

### Testing a debian archive

Once you have a debian archive you need to test *it* to make sure
your installation instructions work as written (and there are no
hidden dependencies). Agian, we need to do this on a fresh clean
AWS EC2.

To do this you can follow the instructions contained in the
[building/Readme](../building/Readme.md).

Once you have an AWS EC2 instance running and configured with one of the
[ec2-install-pdf2html-testDeb.yml](../building/ec2-install-pdf2html-testDeb.yml)
ansible playbook, log into your server once via ssh and then in a new terminal
on your local machine type:

     pushDeb0 <<path to your debian package>>
     
Then back on your new AWS EC2 machine type:

    cd
    ./dobuildPoppler
    ./doinstallPoppler
    
    sudo apt install ./<<debian package name>>

(note the `./` is *required* for apt to install from a *local* package).

You can then test the pdf2htlmEX you have just installed using the commands:

    cd
    pdf2htmlTest testrevelatornl
    pdf2htmlTest joyLoL

as above. As above you can use the:

    openHTML0

on your local machine to view the resulting html files.

If all goes well you can then release the code....

### Releasing a new pdf2htmlEX version

If you have release permissions, go the the pdf2htmlEX
[releases](https://github.com/pdf2htmlEX/pdf2htmlEX/releases)
page and click on the "Draft new release" button.

Choose an appropriate release/tag name. To make it simple
for all users, please ensure the release/tag name *begins* 
with a `v` is then followed by the pdf2htmlEX version being 
released (as recorded in line 13 of the
[CMakeLists.txt](https://github.com/pdf2htmlEX/pdf2htmlEX/blob/master/CMakeLists.txt)
file) which is then followed by `-poppler-` and the poppler
version used to build this version of pdf2htmlEX.

A typical release/tag should look like:

    v0.18.1-poppler-0.75.0

Then in provide a description of what this new release
contains. You might also include some simple installation
instructions.

You should also upload any associated Debian package archives
you created in the previous steps, along with their associated
md5 and sha256 check sum fingerprints.

Finally, when all of the release information is correct, click
on the "Publish release" button.

### You are now done!

Relax :-)

