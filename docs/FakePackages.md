# Creating fake Debian Packages

When developing pdf2htmlEX on a platform such as Ubuntu which has
*older* versions of poppler than the one you are developing
pdf2htmlEX to match... you sometimes need to fake things!

You can do this by using the
[Debian equivs](https://salsa.debian.org/perl-team/modules/packages/equivs)
package.

---

To build a fake dpkg to assert that some real package has been 
installed locally from source you type:

    sudo apt install equivs

This will install all of the required commands.

Then type: 

    equivs-control <<nameOfPackage>>-fake

This will create a template debian package manager file for use in the 
following two steps.

Then edit the `<<nameOfPackage>>-fake` control file by typing:

    nano <<nameOfPackage>>-fake

Edit the `<<nameOfPackage>>-fake` file to provide correct debian package 
details.  The most important two lines are the "Package" name (which 
should be `<<nameOfPackage>>-fake`) and "Provides" (which should be the 
name(s) of the packages which this fake package is asserting are 
already provided on this local system).

Once you have finished your edits, type:

    equivs-build <<nameOfPackage>>-fake

This will build the fake debian package.

---

The above has been adapted from the article:
[Debian Package Management, Part 2: A Developer's Guide](http://www.linuxjournal.com/article/4610)
or from the section 15.2.1. "Meta-Packages or Fake Packages" of the
[Debian handbook guide](http://debian-handbook.info/browse/stable/sect.building-first-package.html#idp19964272).
