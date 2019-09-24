# Tips and Tricks

## Poppler ABI changes ;-(

Check all {inc,dec}RefCnt uses... from poppler-0.68.0 the USER is supposed 
to incRefCnt as well as decRefCnt.

## Finding hidden virtual function prototypes

### g++

make sure the -Woverloaded-virtual warnings are turned ON!

### clang++

make sure the -Woverloaded-virtual warnings are turned ON!

use clang-tidy:

  clang-tidy -p build -header-filter=.* <<a compilation unit>>

This will highlight *clang-diagnostic-overloaded-virtual* warnings.

Consider adding the 'cert-*' checks to check for CERT Secure Coding 
Guidelines

## General tips

### g++

To get the list of g++ warnings:

  g++ --help=warnings

### clang++

To get the list of clang++ warnings, see [Diagnostic flags in 
Clang](https://clang.llvm.org/docs/DiagnosticsReference.html).

Generally all warnings flags which g++ implements will be implemented by 
clang++. BUT they might be implemented differently (and hence triggered on 
slightly different conditions).

See: [Clang 
tools](https://github.com/llvm-mirror/clang/blob/master/docs/ClangTools.rst)

See: 
[Clang-tidy](https://github.com/llvm/llvm-project/blob/master/clang-tools-extra/docs/clang-tidy/index.rst)

See:
[Clang-Tidy](https://clang.llvm.org/extra/clang-tidy/)


See:
[Clang-check](http://manpages.org/clang-check)

See:
  * [Clang-query]
    (https://devblogs.microsoft.com/cppblog/exploring-clang-tooling-part-2-examining-the-clang-ast-with-clang-query/)
  * [AST Matcher 
    Reference](https://clang.llvm.org/docs/LibASTMatchersReference.html)
  * [Matching the Clang AST](https://clang.llvm.org/docs/LibASTMatchers.html)
