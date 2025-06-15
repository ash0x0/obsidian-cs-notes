#Sorting 

Counting sort is a non-[[Comparison Sort]] algorithm that is used in special cases where we have an [[Array]] or data-structure with a finite set of known data classes.

It counts the instances of each of the classes in the data structure and creates a sorted data structure by filling each of the classes with its count in the desired order for classes.
# Mechanism

1. Iterate over the array we want to sort
2. Count the number of instances for each of the classes encountered (a [[Hash Table|Hashmap]] is useful for this)
3. Iterate over the array again from the beginning, placing the highest order item and repeating it by its count, then the second highest order, and so on
