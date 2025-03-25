A form of [[Recursion]] where the recursive call is the last action in the function.

The benefit of  tail recursion is that for certain programming languages (e.g.  `C++`) the compiler could optimize the memory allocation of the call stack by reusing the same space for every recursive call, rather than creating the space for each one. 

As a result, one could obtain the constant [[O(1)]] space for the overhead of the recursive calls.

A function cannot be tail recursive if there are multiple occurrences of recursive calls in the function, even if the last action is the recursive call. Because the system has to maintain the function call stack for the sub-function calls that occur within the same function.