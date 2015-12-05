Optimizing software in C++
An optimization guide for Windows, Linux and Mac
platforms
By Agner Fog. Copenhagen University College of Engineering.
Copyright © 2009. Last updated 2009-09-26.

# Exceptions and stack unwinding

The function F1 is supposed to call the destructor for the object x when it returns. But what
if an exception occurs somewhere in F1? Then we are breaking out of F1 without returning.
F1 is prevented from cleaning up because it has been brutally interrupted. Now it is the
**responsibility of the exception handler to call the destructor of x**. This is only possible if F1
has **saved all information about the destructor to call or any other cleanup** that may be
necessary. If F1 calls another function which in turn calls another function, etc., and if an
exception occurs in the innermost function, then the exception handler **needs all information
about the chain of function calls** and it needs to follow the **track backwards though the
function calls to check for all the necessary cleanup jobs** to do. This is called stack
unwinding.
All functions have to save some information for the exception handler, **even if no exception
ever happens**. This is the reason why exception handling is expensive.

