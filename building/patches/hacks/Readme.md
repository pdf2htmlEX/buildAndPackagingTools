# Poppler and pdf2htmlEX debugging hacks

This directory contains a number of (past) debugging hacks applied to the 
poppler and/or pdf2htmlEX source code to *temporarily* increase the amount
of debugging output... to help discover execution paths and variables.

These may or may not work for future code...

They provide examples of how you might want to hack the source code to 
increase the debug output while working on a difficult problem.

They are typcially created using the `git diff` command, pulled back from 
the AWS EC2 machine, and then reapplied to the code on a new AWS EC2 
machine.

**They should NEVER be applied to the working source repositories**
