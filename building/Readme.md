# Ansible AWS

This Ansible project contains the information required to commission and 
setup a bare standard Ubuntu instance.  This allows me to create a new bare 
standard instance in order to test compile the pdef2htmlEX package on a 
know base system, and will help to explicilty determine what dependencies 
are required for pdf2htmlEX.

## Setup

Using the Ubuntu chooser and then the AWS console create a new EC2 
instance:

  https://cloud-images.ubuntu.com/locator/ec2/

THEN edit the file `hostEnvs` to ensure the aws0, aws1, and/or aws2 are 
associated with the correct *public* ip address

THEN each time you change the `hostEnvs` file you need to type:

  source ./hostEnvs

in each terminal you are using.

THEN ssh for the first time by typing one of the following:

  ./sshAWS0
  ./sshAWS1
  ./sshAWS2

This assumes you have an ssh key agent installed and working...

## Develop, test-building and test-installation

TO *develop* a new version of pdf2htmlEX taken from your *own* fork of the 
pdf2htmlEX repository type one of the following:

  ./runPlaybook0 ec2-install-pdf2html-develop.yml
  ./runPlaybook1 ec2-install-pdf2html-develop.yml
  ./runPlaybook2 ec2-install-pdf2html-develop.yml

TO *test* *build* an existing version of pdf2htmlEX from the *main* 
pdf2htmlEX repository type one of the following:

  ./runPlaybook0 ec2-install-pdf2html-master.yml
  ./runPlaybook1 ec2-install-pdf2html-master.yml
  ./runPlaybook2 ec2-install-pdf2html-master.yml

TO *test* *install* an existing *.deb package type one of the following:

  ./runPlaybook0 ec2-install-pdf2html-testDeb.yml
  ./runPlaybook1 ec2-install-pdf2html-testDeb.yml
  ./runPlaybook2 ec2-install-pdf2html-testDeb.yml

*In each case ensure you have the correct github repository tags specified 
in the above *.yml files*

## Testing pdf2htmlEX itself

To test pdf2htmlEX on the testing machine type one of the following:

  ./pdf2htmlTest testrevelatornl
  ./pdf2htmlTest joyLoL

THEN open the resulting html in a web browser on you local machine by 
typing one of the following:

  openHTML0
  openHTML1
  openHTML2


## Teardown

Using the AWS console .... delete the unused instances

# pdf2htmlEX

## Build poppler

  cd poppler
  cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr \
    -DENABLE_LIBOPENJPEG=none
  make
  sudo make install

## Build pdf2htmlEX

  cd pdf2htmlEX
  cmake .
  make
  sudo make install

