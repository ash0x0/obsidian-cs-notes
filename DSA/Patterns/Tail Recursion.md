A form of [[Recursion]] where the recursive call is the last action in the function (no computation or other actions taken on its result as well) and there's only one recursive call in the function.

The benefit of tail recursion is that for C++ the compiler could optimize the memory allocation of the call stack by reusing the same space for every recursive call, rather than creating the space for each one. 
Also since the recursion return is the last action, we can return from the deepest call in the chain to the original caller straightaway and not resolve the chain so no need for the callstack to store intermediate recursive calls.

As a result, one could obtain the constant [[O(1)]] space for the overhead of the recursive calls.

A function cannot be tail recursive if there are multiple occurrences of recursive calls in the function, even if the last action is the recursive call. Because the system has to maintain the function call stack for the sub-function calls that occur within the same function.