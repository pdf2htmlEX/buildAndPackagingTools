# When Poppler method signatures change...

When Poppler method signatures change, use the command:

    reset; grep -sinr <<name of method>>

in a local copy of the poppler source code.

This should provide examples of what changes the poppler
programmers made to their own code to adapt to their own
method signature changes.

In this situation, it can be helpful to have *two* local 
copies of the poppler source code, one copy at the last
version for which pdf2htmlEX is know to work, and on copy
of poppler at the new version. Once you have identified
examples of the uses of the new method signatures, you
can diff the poppler code between the two versions to
see *how* the poppler programmers made the changes.

## When object creation signatures change...

When an object's creatino signature chagnes, it can be 
rather harder to find example code... so instead of searching
for the object creation (which can be difficult
to identify using `grep`) try reading the code and find
some associated method which *uses* the created object
as a target of your `grep`ing command... 
