# 8 Algorithms
## Sorting Algorithms
Sorting is a fundamental operation in computer science and there are several efficient algorithms for it, such as quicksort, merge sort and heap sort.
### Quicksort
A Divide and Conquer algorithm that chooses a "pivot" element from the array and partitions the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively.
```js
function quicksort(arr) {
  if (arr.length <= 1) {
    return arr;
  }
  let mid = arr.slice(arr.length
}
```


## Search Algorithms
Searching for an element in a large dataset is a common task and there are several efficient algorithms for it, such as binary search and hash tables 

## Graph Algorithms
Used to solve problems related to graphs, such as finding the shortest path between two nodes or determining if a graph is connected 

## Dynamic Programming
A technique for solving problems by breaking them down into smaller subproblems and storing the solutions to these subproblems to avoid redundant computation.

## Greedy Algorithms
Used to solve optimization problems by making the locally optimal choice to each step with the hope of finding a global optimum

## Divide and Conquer
An algorithm design paradigm based on multi-branched recursion. Breaks down a problem into sub-problems of the same or related type, until these become simple enough to be solved directly. 

## Backtracking
Backtracking is a general technique that considers searching every possible combination in a systematic manner, and abandons a particular path as soon as it determines that it cannot be part of the solution. 

## Randomized Algorithm
Use randomness to solve a problem. It can be useful to solve problems that cannot be solved deterministically or to improve the average case complexity of a problem. 
