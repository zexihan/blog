---
layout:     post
title:      "MergeSort, Recurrences, and Asymptotics"
subtitle:   "Algorithms"
date:       2017-09-02
author:     "Zexi"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Algorithms
---

# Merge Sort

Like [QuickSort](https://www.geeksforgeeks.org/quick-sort/), Merge Sort is a [Divide and Conquer](https://www.geeksforgeeks.org/divide-and-conquer-introduction/) algorithm. It divides input array in two halves, calls itself for the two halves and then merges the two sorted halves. **The merge() function** is used for merging two halves. The merge(arr, l, m, r) is key process that assumes that arr[l..m] and arr[m+1..r] are sorted and merges the two sorted sub-arrays into one. See following C implementation for details.

![avatar](2017-09-02-MergeSort-Recurrences-and-Asymptotics/1.png)

```python
# Merges two subarrays of arr[]. 
# First subarray is arr[l..m] 
# Second subarray is arr[m+1..r] 
def merge(arr, l, m, r): 
    n1 = m - l + 1
    n2 = r- m 
  
    # create temp arrays 
    L = [0] * (n1) 
    R = [0] * (n2) 
  
    # Copy data to temp arrays L[] and R[] 
    for i in range(0 , n1): 
        L[i] = arr[l + i] 
  
    for j in range(0 , n2): 
        R[j] = arr[m + 1 + j] 
  
    # Merge the temp arrays back into arr[l..r] 
    i = 0     # Initial index of first subarray 
    j = 0     # Initial index of second subarray 
    k = l     # Initial index of merged subarray 
  
    while i < n1 and j < n2 : 
        if L[i] <= R[j]: 
            arr[k] = L[i] 
            i += 1
        else: 
            arr[k] = R[j] 
            j += 1
        k += 1
  
    # Copy the remaining elements of L[], if there 
    # are any 
    while i < n1: 
        arr[k] = L[i] 
        i += 1
        k += 1
  
    # Copy the remaining elements of R[], if there 
    # are any 
    while j < n2: 
        arr[k] = R[j] 
        j += 1
        k += 1
  
# l is for left index and r is right index of the 
# sub-array of arr to be sorted 
def mergeSort(arr,l,r): 
    if l < r: 
  
        # Same as (l+r)/2, but avoids overflow for 
        # large l and h 
        m = (l+(r-1))/2
  
        # Sort first and second halves 
        mergeSort(arr, l, m) 
        mergeSort(arr, m+1, r) 
        merge(arr, l, m, r) 
```

**Time Complexity:** Sorting arrays on different machines. Merge Sort is a recursive algorithm and time complexity can be expressed as following recurrence relation.
T(n) = 2T(n/2) + ![\Theta(n)](https://www.geeksforgeeks.org/wp-content/ql-cache/quicklatex.com-13ebbf70ea41ddbd496e1f917a0a9f75_l3.svg)
The above recurrence can be solved either using Recurrence Tree method or Master method. It falls in case II of Master Method and solution of the recurrence is ![\Theta(nLogn)](https://www.geeksforgeeks.org/wp-content/ql-cache/quicklatex.com-9a23201324ac0d925d9337f1ff4ec68f_l3.svg).
Time complexity of Merge Sort is ![\Theta(nLogn)](https://www.geeksforgeeks.org/wp-content/ql-cache/quicklatex.com-9a23201324ac0d925d9337f1ff4ec68f_l3.svg) in all 3 cases (worst, average and best) as merge sort always divides the array in two halves and take linear time to merge two halves.

**Auxiliary Space:** O(n)

**Algorithmic Paradigm:** Divide and Conquer

[Lecture Notes](/blog/docs/algorithms/CS161Lecture01.pdf)

[IPython Notebook: Insertion Sort and Merge Sort](/blog/docs/algorithms/lecture2_sorting.html)

[auxiliary .py file for IPython Notebook](/blog/docs/algorithms/tryItABunch.py)

Relevant reading:

* CLRS Sections 2.3 and 3.