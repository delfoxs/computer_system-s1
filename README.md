README.template

## Project Number/Title 

* Authors: Your Name, and your group members’ names
* Group name: Your Group Name

## Overview

Concisely explain what the program does. If this exceeds a couple of
sentences, you're going too far. Generally, you should be pulling this
right from the project specification. We don't want you to just cut and
paste, but paraphrase what is stated in the project specification.

## Manifest

* `mergesort.c` – Implements sequential and parallel merge sort logic.
* `mergesort.h` – Header file declaring functions, global arrays, and structs for mergesort.
* `test-mergesort.c` – Main program: generates arrays, calls mergesort, measures execution time, and prints results.
* `Makefile` – Builds the `test-mergesort` executable and handles cleanup.

## Building the project


To build the project, run:

```bash
make
```
This will compile test-mergesort.c and mergesort.c and produce the executable test-mergesort.

To clean up the generated object files and executable, run:

```bash
make clean
```
Run the executable with three arguments:
```bash
./test-mergesort <input size> <cutoff level> <random seed>
```

&lt;input size&gt;: Number of elements to sort (must be at least 2)

&lt;cutoff level&gt;: Number of parallel thread levels (0 for single-threaded)

&lt;random seed&gt;: Seed for generating the random array

Example:
```bash
./test-mergesort 100000 4 1234
```

## Features and usage

Summarise the main features of your program. It is also appropriate to
instruct the user how to use your program.

## Testing

First, I tested the basic functionality and correctness:
- Used small arrays (e.g., 10 elements) for sorting, observed the program’s outputs for arrays **A** and **B**, and ensured the results were in non-decreasing order.
- Tested edge cases (input sizes of 0 or 1). The program correctly displayed “the input size must be at least 2!” and terminated.
- Tested medium-sized arrays (e.g., 100 elements). The program sorted correctly, verifying the basic correctness of the merge sort logic.

Next, I tested the parallel speedup:
- Ran the program with different input sizes (`n = 10^4, 10^6, 10^8`) and different thread depth levels (`cutoff = 0, 1, 2, 3, 4, 5, 8`).
- Recorded the sorting time for each run and compared single-threaded (`cutoff = 0`) versus multi-threaded (`cutoff > 0`) performance.
- The results show that enabling multi-threading (e.g., `cutoff = 8`) significantly reduced runtime compared with the single-threaded case, confirming the acceleration effect of the parallel merge sort.

In addition, I examined the array printing logic used mainly for debugging in test-mergesort.c:
- Although the sorting result is correct in most cases, some values in array **B** occasionally appeared abnormal (e.g., 0 or random large numbers).
- This issue arises because only `mergesort.c` is allowed to be modified for the project, which can lead to write conflicts when the global buffer **B** is accessed concurrently by multiple threads.
- The known issue does not affect the final sorting correctness, but it is visible when printing **B**.

Through these tests, I verified the program’s functional correctness, parallel speedup, and handling of extreme inputs, while also identifying a known multi-threaded output anomaly.


## Known Bugs

- When multi-threaded sorting is enabled (`cutoff > 0`), the global temporary array **B** may contain abnormal values (e.g., 0 or random large numbers) in some cases.
- This primarily appears when printing the debugging view of **B** and is caused by concurrent access and writes to the same global array **B** by multiple threads, leading to write conflicts.
- The issue does not affect the final sorting result; array **A** is always sorted correctly.
- Because the project rules allow changes only in `mergesort.c`, the concurrent access conflict on **B** has not been fixed.


## Reflection and Self Assessment

During development and testing, I encountered several issues and gained valuable experience.

1. **Boundary handling in merge**  
   When implementing the merge sort algorithm, I initially failed to consider unbalanced arrays in the merge process—i.e., the left and right subarrays might not be the same length. My first implementation only handled the ideal case where both sides were equal in length. As a result, when the array length was odd or the recursive splits were uneven, some elements in the temporary array **B** were not properly assigned.  
   I resolved this by modifying the merge function to add loops that process the remaining elements on either side, ensuring that all elements are copied back into the original array **A** correctly. This deepened my understanding of boundary conditions for merge sort on arrays of odd and even lengths.

2. **Parallel level control and parameter passing**  
   While implementing parallel merge sort, I ran into issues with thread parameter passing and level control. In the initial code, the `level` parameter wasn’t incremented correctly, which caused the cutoff logic to fail and prevented any meaningful multi-threaded speedup.  
   After analysis, I ensured that `level` increments correctly in both recursive calls and thread arguments, and that once the cutoff level is reached, the algorithm falls back to the standard (single-threaded) merge sort. This successfully produced parallel acceleration.

3. **Testing coverage and performance**  
   - Used small arrays (e.g., 10 elements) and medium arrays (e.g., 100 elements) to verify sorting correctness.  
   - Tested extreme cases (input sizes 0 or 1) to confirm proper error handling and termination.  
   - Ran the program with different input sizes (`n = 10^4, 10^6, 10^8`) and different parallel levels (`cutoff = 0, 1, 2, 3, 4, 5, 8`), recorded sorting times, and verified multi-threaded acceleration. The results show that enabling multi-threading (e.g., `cutoff = 8`) yields significantly shorter runtimes than single-threaded execution, demonstrating the effectiveness of parallel merge sort.

4. **Debug output anomaly of array B**  
   I observed abnormal values (e.g., 0 or random large numbers) when printing array **B**. This is due to the project constraint that only `mergesort.c` can be modified, which can cause concurrent write conflicts to the global buffer **B** during multi-threaded execution. This known issue does not affect the final sorted result but is visible during debugging output.

Overall, this project helped me master the implementation details and boundary handling of merge sort, and deepen my understanding of multi-threaded programming, thread synchronization, and performance optimization. The development and testing process taught me systematic approaches to debugging, analysis, and iterative improvement, and also highlighted the challenges and enjoyment of designing and implementing solutions under constraints.




## Sources Used

YouTube video: 
Algorithms: Merge Sort

https://www.youtube.com/watch?v=KF2j-9iSf4Q&t=372s
 – for understanding the merge sort algorithm and its recursive structure.
