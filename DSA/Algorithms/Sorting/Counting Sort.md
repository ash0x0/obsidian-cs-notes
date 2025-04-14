  
[[Quicksort]] usually has a running time of [[O(n log n)]] , but is there an algorithm that can sort even faster? In general, this is not possible. Most sorting algorithms are [[Comparison Sort]], i.e. they sort a list just by comparing the elements to one another. A comparison sort algorithm cannot beat [[O(n log n)]] (worst-case) running time, since `n log(n)` represents the minimum number of comparisons needed to know where to place each element. For more details, you can see [these notes](http://www.cs.cmu.edu/~avrim/451f11/lectures/lect0913.pdf) (PDF).

**Alternative Sorting**  

Another sorting method, the counting sort, does not require comparison. 

# Mechanism

1. Create an integer array whose index range covers the entire range of values in your array to sort. 
2. Each time a value occurs in the original array, you increment the counter at that index.
3. At the end, run through your counting array, printing the value of each non-zero valued index that number of times.