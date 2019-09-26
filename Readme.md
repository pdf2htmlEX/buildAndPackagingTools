# pdf2htmlEX-packagingTools

A collection of build and packaging tools for the pdf2htmlEX project

We tag/release these files in sync with the pdf2htmlEX/pdf2htmlEX releases

# Project layout:

1. the **building** directory contains an [Ansible](https://ansible.com) 
   project which knows how to commision an AWS EC2 instance (typically an 
   Ubuntu AMI), for use to test-compile, package, and test-install a given 
   pdf2htmlEX (potential) release on a *clean* *known* Linux release. 
   (See the [Ansible documentation](https://docs.ansible.com))

2. the **packaging** directory contains the **debian** based 
   [build_dists.py](building/build_dists.py) packaging tool and associated 
   files. These are automatically copied into the pdf2htmlEX project on the 
   AWS EC2 build machine by the
   [ec2-install-pdf2html-master.yml](building/ec2-install-pdf2html-master.yml) 
   ansible playbook.

3. the **docs** directory contains more detailed information used to 
   develop new versions of pdf2htmlEX. In particular:

     * [NewPopplerRelease](docs/NewPopplerRelease.md) contains a useful 
       collection of steps use to bring pdf2htmlEX sources up to date with 
       a new [poppler](https://poppler.freedesktop.org/) release.

     * [TipsAndTricks](docs/TipsAndTricks/Readme.md) contains ideas which have 
       been useful in the past for identifying how changes to poppler 
       might impact pdf2htmlEX.

## To do

At the moment the debian based build script *does not sign* the debian archive.
We should be good people and fix this...
