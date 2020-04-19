# Lecture 12 - Searching & Sorting

Searching and sorting techniques are an important part of computer science. Additionally, looking at the algorithms that are used to search and sort can be instructive for how we look at other algorithms and how we judge efficiency in different techniques.

## Big-O Notation
In order to describe the efficiency of an algorithm, we look at how many operations need to happen as the algorithm processes the data. This is usually expressed in terms of the list size where there are **n** items in the list.
- O(n)
- O(n<sup>2</sup>)
- O(n log(n))

## Searching
When we have data, retrieving the data quickly and efficiently becomes an important issue. There are several techniques for doing this with different pre-conditions and efficiency.

### Linear Search
A linear search is just as it sounds. The algorithm will begin searching with the first item and proceed in a linear fashion until the desired item is found or the list is exhausted and the item was not found.

Best case: **O(1)** - Element is first checked, only 1 check is made, this is extremely quick but also unlikely.  
Worst case: **O(n)** - The element is in the last spot in the list. For a list of **n** items it would take n steps before it was found.  
Average case: **O(n/2)** - The item is somewhere in the middle of the list, sometimes near the beginning (fast) and sometimes near the end (slower).  
Not in List: **O(n)** - The algorithm needs to check every element to determine that the item is not in the list.  
Since **n/2** is really driven by the size of n, we simplify to call this algorithm **O(n)**.

Example:  
data = [5, 12, 17, 9, 32, 18, 7]  
***Where is the 9?***  
[**5**, 12, 17, 9, 32, 18, 7] - nope  
[5, **12**, 17, 9, 32, 18, 7] - nope  
[5, 12, **17**, 9, 32, 18, 7] - nope  
[5, 12, 17, **9**, 32, 18, 7] - yep  
***9 is located in spot 3***


### Binary Search
A binary search is like playing the high/low number guessing game. The middle element in the list is selected, if it is too big or too small then half of the list is known to not contain the number. The precondition for this is that the list must be sorted in order for the binary search to work.

When you are looking for an item in a list, the first guess would be the middle spot, if that is wrong then we can dismiss half the list and look at the middle spot of the remining numbers.

Example:  
data = [6, 9, 12, 13, 17, 20, 25, 29, 33, 35]  
***Where is the 25?***  
[6, 9, 12, 13, **17**, 20, 25, 29, 33, 35] - nope, too small  
[~6, 9, 12, 13, 17~, 20, 25, **29**, 33, 35] -nope, too big  
[~6, 9, 12, 13, 17~, **20**, 25, ~29, 33, 35~] -nope, too small  
[~6, 9, 12, 13, 17, 20~, **25**, ~29, 33, 35~] -found it  
***25 is located in spot 6***

This was a worst-case example but in 10 elements, it only 4 evaluations before the number was found. If the list size were doubled from 10 to 20 elements, it would only require 1 more evaluation.

This is a logarithmic function and so the efficiency is **O( log<sub>2</sub>(n) )**

For a list of 1,000,000 elements, the search would only require 20 iterations. Obviously the big caveat is that the list must be sorted before this technique will work.

## Sorting
There are many sorting techniques, with different advantages and disadvantages. We are not going to look at all of them but look at a few of the more well-known algorithms.

### Bubble Sort
In a bubble sort, the first two elements are compared to see if they are in order. If not, they swap places in the list. Then the next two elements are compared... this process continues until the largest element in the list "bubbles" up to the final position. The process is repeated until all the items are in order.

Example:  
data = [5, 12, 17, 9, 32, 18, 7]
Bubble sort of the data  
[**5, 12**, 17, 9, 32, 18, 7] - In order, no swap needed.  
[5, **12, 17**, 9, 32, 18, 7] - In order, no swap needed.  
[5, 12, **17, 9**, 32, 18, 7] - Out of order, swap.  
[5, 12, 9, **17, 32**, 18, 7] - In order, no swap needed.  
[5, 12, 9, 17, **32, 18**, 7] - Out of order, swap.  
[5, 12, 9, 17, 18, **32, 7**] - Out of order, swap.  

[5, 12, 9, 17, 18, 7, ***32***] - After one pass, the largest element is now in the correct spot.  

This will continue, though the we can now ignore the last element since we know that it is in the correct location.

For our list of 7 numbers, on the first pass we needed to do 6 evaluations. On the next pass, we will need to do 5, then 4, and so on until we have a fully sorted list.

For a list of **n** items, we will need to do (n-1) + (n-2) + ... + 1 evaluations which can be simplified to (n-1)(n-2) / 2. Once this is multiplied there is a n<sup>2</sup> which is the most important factor. Therefore, we call this algorithm's efficiency **O(n<sup>2</sup>)**

We can add a shortcut that if a pass is made without any switches, we know that the array is in order and we can stop the process now.

### Selection Sort
In a selection sort, the largest element is found in the list and placed in the last spot. The process is repeated with the next-largest element until the list is fully sorted.

Example:  
data = [5, 12, 17, 9, 32, 18, 7]
Selection sort of the data  
[**5**, 12, 17, 9, 32, 18, 7] - Largest value, so far.    
[5, **12**, 17, 9, 32, 18, 7] - New largest value    
[5, 12, **17**, 9, 32, 18, 7] - New largest value    
[5, 12, 17, **9**, 32, 18, 7] - Not larger than the best so far  
[5, 12, 17, 9, **32**, 18, 7] - New largest value
[5, 12, 17, 9, 32, **18**, 7] - Not larger than the best so far  
[5, 12, 17, 9, 32, 18, **7**] - Not larger than the best so far  
Swap the largest value to the last spot  
[5, 12, 17, 9, **7**, 18, **32**]
Now the process is repeated with the next n-1 elements

As with Bubble Sort, the algorihtm must go through the array and evaluate (n-1) + (n-2) + ... + 1 elements. Giving this a runtime of **O(n<sup>2</sup>)**

Unlike Bubble Sort, there are no shortcuts so the even if the data is already sorted, it will still take the full time.

### Insertion Sort
With insertion sort, the algorithm only looks at the first element of the list, which is sorted since it is only 1 element. Then the next element is added to the sorted array and "inserted" into the correct location. This process continues until all of the elements have been inserted to the array.

Example:  
data = [5, 12, 17, 9, 32, 18, 7]
Insertion sort of the data  
[**5**, 12, 17, 9, 32, 18, 7]  - Sorted list of 1 element.  
[**5, 12**, 17, 9, 32, 18, 7]  - Next element is added, no shifting needed.  
[**5, 12, 17**, 9, 32, 18, 7]  - Next element is added, no shifting needed.  
[**5, 9, 12, 17**, 32, 18, 7]  - Next element is added, inserted between 5 and 12.  
[**5, 9, 12, 17, 32**, 18, 7]  - Next element is added, no shifting needed.  
[**5, 9, 12, 17, 18, 32**, 7]  - Next element is added, one shift needed.  
[**5, 7, 9, 12, 17, 18, 32**]  - Next element is added, five shifts needed.  
List sorted.  

Insertion sort also has a general runtime of **O(n<sup>2</sup>)** however, if the list is already somewhat - mostly sorted to begin, it is faster. If the list were sorted but in reverse order(worst case), it would take the same number of comparisons as every run of Selection Sort.

### Merge Sort
Merge sort uses recursion where a function calls itself as part of the process. The list is split in half, then each half is sorted. Once the two halves have been sorted, they are "merged" back into one list. Because it is recursive, each of the two halves are further split and so on until the lists are one element.

Example:  
data = [5, 12, 17, 9, 32, 18, 7]
Merge sort of the data.  
[5, 12, 17]  [9, 32, 18, 7]  - List is split and merge sort is preformed on each half.  
[ [5] [12, 17] ] [ [9, 32] [18, 7] ] - The remaining lists are each split and merge sort is preformed.  
[ [5] [ [12] [17] ] ] [ [9] [32] [ [18] [7] ] ] - Now the merging can begin.  
[ [5] [ **12, 17** ] ] [ **9, 32** ] [ **7, 18** ] ] - Now the next level of merging can happen.  
[ ***5, 12, 17*** ]  [ **7, 9, 18, 32** ]  - Now the final level of merging can happen.     
[ 5, 7, 9, 12, 17, 18, 32]  - The initial two (now sorted) halves are now merged and the list is in order.     

While more confusing than the earlier methods, it is also faster with an average runtime of **O(n * log(n)**)

### Further reading
- [Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)  
- [Selection Sort](https://www.geeksforgeeks.org/selection-sort/)
- [Insertion Sort](https://www.geeksforgeeks.org/insertion-sort/)
- [Merge Sort](https://www.geeksforgeeks.org/merge-sort/)
