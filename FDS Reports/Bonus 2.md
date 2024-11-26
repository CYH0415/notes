
<div align=center STYLE="page-break-after: always;">
<!--敲几个回车-->
    <br/><br/><br/><br/><br/><br/><br/><br/><br/>
<!--标题调整，在这里选择你想要的字号和字体-->
    <font size=15 face="微软雅黑">
        Replace Algorithm<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/><br/>
    <font size=6 face="微软雅黑">
        陈宇涵<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/>
    <font size = 4>
        Date: 2024-05-28<!--内容自己改--><br/>
        <br/><br/><br/><br/><br/><br/>
    </font>
</div>

# Chapter 1: Introduction
*Problem description and (if any) background of the algorithms.*

**Problem Description:** When dealing with datasets that exceed the available internal memory, external sorting becomes necessary. One effective method for external sorting is to generate sorted runs using limited internal memory. This report describes the implementation of a Replacement Selection algorithm for external sorting, which allows for the efficient generation of these sorted runs.

**Background:** The Replacement Selection algorithm, introduced by Donald Knuth in 1965, takes advantage of the memory freed up as records are written to the output. This approach helps in producing longer runs compared to the simplest method of sorting fixed-size chunks of data independently.

# Chapter 2: Algorithm Specification
*Description (pseudo-code preferred) of all the algorithms involved for solving the problem, including specifications of main data structures.*

**Description:**
The Replacement Selection algorithm sorts data in ascending order, producing runs that are as long as possible. When the smallest record is written to the output, the space it occupied in memory becomes available for a new record. If this new record is larger than the last output record, it belongs to the current run; otherwise, it starts a new run.

**Function Descriptions and Pseudo-Code:**

1. **Insert Function**
   - **Description:** Inserts a new number into the heap while maintaining the heap property.
   - **Pseudo-Code:**
```c
int insert(heap, size, value):
    i = size
    size = size + 1
    while i > 0:
        parent = (i - 1) / 2
        if heap[parent] <= value:
            break
        heap[i] = heap[parent]
        i = parent
    heap[i] = value
```

2. **DeleteMin Function**
   - **Description:** Removes and returns the smallest number from the heap, maintaining the heap property.
   - **Pseudo-Code:**
```c
int deleteMin(heap, size):
    result = heap[0]
    value = heap[size - 1]
    size = size - 1
    i = 0
    while i * 2 + 1 < size:
        leftChild = i * 2 + 1
        rightChild = i * 2 + 2
        minChild = smaller of existing child
        if heap[minChild] >= value:
            break
        heap[i] = heap[minChild]
        i = minChild
    heap[i] = value
    return result
```

3. **Main Function**
   - **Description:** Manages the process of reading input, sorting runs using heaps, and switching between heaps when runs change.
   - **Pseudo-Code:**
```c
int main():
read N and M
for i from 1 to N:
    read A[i]
    if i <= M:
        insert(heap1, size1, A[i])
cnt = M + 1
while size1 > 0:
    curNum = deleteMin(heap1, size1)
    output curNum
    if cnt <= N:
        nextNum = A[cnt]
        cnt = cnt + 1
        if nextNum < curNum:
            insert(heap2, size2, nextNum)
        else:
            insert(heap1, size1, nextNum)
    if size1 > 0:
        output " "
    else:
        output newline
        swap(heap1, heap2)
        swap(size1, size2)

```

**Main Data Structures:**

- `heap1` and `heap2`: Two min-heaps to manage the current and next runs.
- `size1` and `size2`: Sizes of `heap1` and `heap2` respectively.
- `A`: Array to hold input data.
# Chapter 3: Testing Results
1. Same as sample

| Input                                          | Output                                       |
| ---------------------------------------------- | -------------------------------------------- |
| 13 3<br>81 94 11 96 12 99 17 35 28 58 41 75 15 | 11 81 94 96 99<br>12 17 28 35 41 58 75<br>15 |
*pass*
2. Large input

| Input                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Output           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| 100 8<br>23287 3994 13690 18349 22543 6825 23944 786 23920 27924 8933 20585 9064 32381 31991 28301 15917 24530 4164 10746 23074 5760 27867 12699 3414 14307 3056 11741 25165 22785 20678 19976 4 9849 4306 16335 4911 14518 26397 17702 23749 30824 22250 23635 2494 10037 409 11848 14364 24747 29643 14128 19752 10106 19911 23880 254 21216 19228 27120 17227 28772 28040 6402 23894 28103 20249 12898 30013 7992 24466 25046 22145 30151 4368 16664 9157 31260 14317 18474 21971 24425 8677 6122 11232 17053 10359 21222 28945 27099 17038 30833 15166 6186 22881 30199 1781 18798 25752 25229 | see output 2.txt |
*pass*
3. Small input

| Input         | Output |
| ------------- | ------ |
| 1 1<br>114514 | 114514 |
*pass*
# Chapter 4: Analysis and Comments

**Time Complexity:**

- Insertion and deletion operations on the heap take $O(log⁡M)$ time, where $M$ is the memory size.
- Each of the $N$ elements is processed once, resulting in a total time complexity of $O(Nlog⁡M)$.

**Space Complexity:**

- The space complexity is dominated by the storage of the heaps, which is $O(N)$.

**Comments:**

- The algorithm efficiently produces long runs, minimizing the number of runs and, consequently, the number of merge passes required in the subsequent external sort phase.
- Potential improvements could include optimizing the heap operations or exploring different data structures to manage the runs.
# Appendix:  Source Code (in C)
```c
#include <stdio.h>

#define MAX_N 114514

// Declare values to be used globally
int N, M, cnt, curNum, nextNum;
int size1 = 0, size2 = 0;

// Function to insert a new number into the heap
void insert(int heap[], int *size, int value) {
    int i = (*size)++; // Increase heap size and get the current index
    while (i > 0) {
        int parent = (i - 1) / 2; // Calculate parent index
        if (heap[parent] <= value) break; // If parent value is less than or equal to the value to be inserted, break
        heap[i] = heap[parent]; // Move parent value down
        i = parent; // Move to parent index
    }
    heap[i] = value; // Insert value at the correct position
}

// Function to remove and return the minimum number from the heap
int deleteMin(int heap[], int *size) {
    int result = heap[0]; // The minimum value is at the root
    int value = heap[--(*size)]; // Decrease size and get the last value in the heap
    int i = 0;
    while (i * 2 + 1 < *size) { // While the current node has at least one child
        int leftChild = i * 2 + 1;
        int rightChild = i * 2 + 2;
        int minChild = (rightChild < *size && heap[rightChild] < heap[leftChild]) ? rightChild : leftChild; // Find the smaller child
        if (heap[minChild] >= value) break; // If the smaller child is greater than or equal to the value, break
        heap[i] = heap[minChild]; // Move smaller child up
        i = minChild; // Move to the index of the smaller child
    }
    heap[i] = value; // Insert value at the correct position
    return result; // Return the minimum value
}

int main() {
    // Read the values of N and M
    scanf("%d %d", &N, &M);
    
    int A[N + 5], heap1[M + 5], heap2[N + 5];
    
    // Read the input array and build the initial heap
    for (int i = 1; i <= N; i++) {
        scanf("%d", &A[i]);
        if (i <= M) insert(heap1, &size1, A[i]); // Only insert the first M numbers into the heap
    }
    
    cnt = M + 1; // Initialize the counter to point to the next element after the first M elements
    
    // Process the heaps and output the sorted numbers
    while (size1 > 0) {
        curNum = deleteMin(heap1, &size1); // Get the minimum number from the heap
        printf("%d", curNum); // Print the minimum number
        
        if (cnt <= N) { // If there are still numbers left in the input array
            nextNum = A[cnt++]; // Get the next number
            if (nextNum < curNum) { // If the next number is smaller than the current minimum
                insert(heap2, &size2, nextNum); // Insert it into the second heap
            } else { // If the next number is larger or equal to the current minimum
                insert(heap1, &size1, nextNum); // Insert it into the first heap
            }
        }
        
        if (size1 > 0) { // If the first heap still has elements
            printf(" ");
        } else { // If the first heap is empty, switch to the second heap
            printf("\n");
            for (int i = 0; i < N; i++) {
                int t = heap1[i]; // Temporary variable for swapping
                heap1[i] = heap2[i]; // Move elements from heap2 to heap1
                heap2[i] = t; // Clear heap2
            }
            int temp = size1; // Swap the sizes of the heaps
            size1 = size2;
            size2 = temp;
        }
    }
    return 0;
}
```
