# Solving random segmenation faults...

Ideally the source code *should* be littered with ANSI-C `assert`ions.
These are indispensable in detecting segmentation faults due to
dereferencing null pointers. 

Obviously there are not enough... so consider adding more!

One way to track down the source of a random segmentation fault, 
is to litter the pdf2htmlEX and/or poppler source code with
printfs... This will allow you to detect the last working code, 
and this might suggest where to start reading the code in detail.

## Poppler reference counting

Poppler uses object reference counting. Unfortunately the assumption
about "who" is responsible for incrementing and decrementing these
refernce counts *does* change between poppler releases. This means
that between poppler releases the pdf2htmlEX code should include
new increments or decrements *or* now has too many...

**Solution:** Check all {inc,dec}RefCnt uses...
from poppler-0.68.0 the USER (i.e. pdf2htmlEX) is supposed to
incRefCnt as well as decRefCnt.

