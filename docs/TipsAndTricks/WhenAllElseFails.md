# When all else fails...

When all else fails... make a guess and then hack either 
the poppler source code or the pdf2htmlEX source code (or both).

Progressively add printf statements to record the different paths
taken during a run of the pdf2htmlEX command.

Typically these printf statements will announce the entry and exit
of important methods. They will also typically record the values
of various important variables (including values of the formal
parameters).

Eventually you should see what assumptions you or pdf2htmlEX are
making that are not correct...

It is then obvious (says he) what needs to be fixed...

Given you are working directly on the source code on the
AWS EC2 machine.... it is easy to get rid of the printfs...
just throw the AWS EC2 machine away and start again.... :-)
