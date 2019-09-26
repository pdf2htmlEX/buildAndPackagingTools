# General tips

## g++

To get the list of g++ warnings:

  g++ --help=warnings

## clang++

To get the list of clang++ warnings, see [Diagnostic flags in 
Clang](https://clang.llvm.org/docs/DiagnosticsReference.html).

Generally all warnings flags which g++ implements will be implemented by 
clang++. BUT they might be implemented differently (and hence triggered on 
slightly different conditions).

See: [Clang 
tools](https://github.com/llvm-mirror/clang/blob/master/docs/ClangTools.rst)

## clang-tidy

See: 
[Clang-tidy](https://github.com/llvm/llvm-project/blob/master/clang-tools-extra/docs/clang-tidy/index.rst)

See:
[Clang-Tidy](https://clang.llvm.org/extra/clang-tidy/)

## clang-check

See:
[Clang-check](http://manpages.org/clang-check)

## Other tools

See:
  * [Clang-query]
    (https://devblogs.microsoft.com/cppblog/exploring-clang-tooling-part-2-examining-the-clang-ast-with-clang-query/)
  * [AST Matcher 
    Reference](https://clang.llvm.org/docs/LibASTMatchersReference.html)
  * [Matching the Clang AST](https://clang.llvm.org/docs/LibASTMatchers.html)
