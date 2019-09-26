# Finding hidden virtual function prototypes...

## g++

Make sure the `-Woverloaded-virtual` warnings are turned ON!

## clang++

Make sure the `-Woverloaded-virtual` warnings are turned ON!

## clang-tidy

Use **clang-tidy**:

    clang-tidy -p build -header-filter=.* <<a compilation unit>>

This will highlight *clang-diagnostic-overloaded-virtual* warnings.

Consider adding the `cert-*` checks to check for CERT Secure Coding 
Guidelines
