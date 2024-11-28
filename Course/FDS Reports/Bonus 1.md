<div align=center STYLE="page-break-after: always;">
<!--敲几个回车-->
    <br/><br/><br/><br/><br/><br/><br/><br/><br/>
<!--标题调整，在这里选择你想要的字号和字体-->
    <font size=15 face="微软雅黑">
        LRU-K<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/><br/>
    <font size=6 face="微软雅黑">
        陈宇涵<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/>
    <font size = 4>
        Date: 2024-06-03<!--内容自己改--><br/>
        <br/><br/><br/><br/><br/><br/>
    </font>
</div>

# Chapter 1: Introduction
*Problem description and (if any) background of the algorithms.*
**Problem Description:** In computer science, caching is a widely used technique to improve system performance by storing frequently accessed data in a faster storage medium. The Least Recently Used (LRU) cache scheme is a popular caching algorithm where the least recently used item is evicted from the cache when the cache is full and a new item needs to be added. However, in some scenarios, simply evicting the least recently used item may not be the most efficient approach, especially when certain items are accessed more frequently than others.

**Algorithm Background:** The LRU-K cache scheme is a variant of the LRU algorithm designed to address the limitations of traditional LRU caching. In LRU-K, where K represents the number of recent accesses, items are moved from the historical access queue to the cache queue only after being accessed K times. This modification ensures that items are only promoted to the cache if they are frequently accessed, thereby improving cache hit rates and overall system performance.

By maintaining two separate queues for historical accesses and the cache, the LRU-K algorithm effectively manages the cache contents based on the frequency of item accesses. Items that are accessed frequently are retained in the cache, while less frequently accessed items are evicted to make space for new entries. This dynamic approach to cache management offers a balance between recency and frequency of access, resulting in optimized cache utilization and improved system performance.
# Chapter 2: Algorithm Specification
*Description (pseudo-code preferred) of all the algorithms involved for solving the problem, including specifications of main data structures.*
**Description:**
The LRU-K algorithm manages a cache by maintaining two queues: one for historical accesses and one for the actual cache. An item is moved to the cache only after it has been accessed K times. This ensures that only frequently accessed items are stored in the cache, optimizing cache performance by distinguishing between frequently and infrequently accessed items.

**Function Descriptions and Pseudo-Code:**

1. **init Function**
   - **Description:** Initializes a queue.
   - **Pseudo-Code:**
```
FUNCTION init():
    CREATE a new queue and set its size, first, and last to initial values
    RETURN the initialized queue
```

2. **push Function**
   - **Description:** Adds a value to the queue.
   - **Pseudo-Code:**
```
FUNCTION push(Queue Q, int x):
    CREATE a new node with data x
    IF the queue is empty:
        SET the new node as the first and last node
    ELSE IF the queue is not full:
        SET the new node as the last node's next and update the last node
    ELSE:
        SET the new node as the last node's next
        REMOVE the oldest node from the queue
    INCREMENT the queue size
```

3. **show Function**
   - **Description:** Displays the values in the queue.
   - **Pseudo-Code:**
```
FUNCTION show(Queue Q):
    FOR each node in the queue:
        PRINT the node's data
    IF the queue is empty:
        PRINT "-"
```

4. **deleteAll Function**
   - **Description:** Removes all occurrences of a value from the queue.
   - **Pseudo-Code:**
```
FUNCTION deleteAll(Queue Q, int x):
    IF the queue has only one node and its data equals x:
        REMOVE the node
    FOR each node in the queue:
        IF the next node's data equals x:
            REMOVE the next node
    UPDATE the last node
```

5. **inQueue Function**
   - **Description:** Checks if a value exists in the queue.
   - **Pseudo-Code:**
```
FUNCTION inQueue(Queue Q, int x):
    FOR each node in the queue:
        IF the node's data equals x:
            RETURN true
    RETURN false
```

**Main Data Structures:**

- **Queue:**
    
    - Description: A doubly linked list data structure representing a queue. Each node in the queue holds the data value and pointers to the next and previous nodes.
    - Attributes:
        - `size`: Integer representing the number of elements in the queue.
        - `first`: Pointer to the first node in the queue.
        - `last`: Pointer to the last node in the queue.
    - Operations:
        - `init()`: Initializes an empty queue.
        - `push(x)`: Adds a new element with value x to the end of the queue.
        - `deleteAll(x)`: Removes all occurrences of value x from the queue.
        - `inQueue(x)`: Checks if value x exists in the queue.
        - `show()`: Displays the contents of the queue.
- **Node:**
    
    - Description: Represents a single node in the queue, containing the data value and pointers to the next and previous nodes.
    - Attributes:
        - `data`: The value stored in the node.
        - `next`: Pointer to the next node in the queue.
# Chapter 3: Testing Results
1. Same as sample

| Input                                       | Output           |
| ------------------------------------------- | ---------------- |
| 2 5 17<br>9 5 6 7 8 3 8 9 5 9 8 3 4 7 5 6 9 | 4 9<br>8 3 7 5 6 |
*pass*
2. Same as sample with empty queue

| Input                         | Output         |
| ----------------------------- | -------------- |
| 3 5 10<br>9 5 6 7 8 3 8 9 5 9 | 7 3 8 5 9<br>- |
*pass*
3. Small input

| Input      | Output |
| ---------- | ------ |
| 1 1 1<br>1 | 1<br>- |
*pass*
4. Large input without repetition

| Input                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Output                               |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| 2 5 500<br>32655 3336 28724 8347 19352 30774 774 9292 24653 24066 23593 5776 14520 20052 2225 22968 20794 10786 9104 9091 13541 2881 28181 17297 31270 24187 20914 11802 29351 25569 12034 7904 4723 2402 7690 26722 21084 21405 960 17235 4506 18676 8393 12874 15028 4834 32135 19899 12196 18830 22355 3110 16694 7584 29447 23182 13745 21587 30912 6445 8889 18891 15586 20582 7796 28074 6748 12795 3052 28126 16941 14976 1716 956 9255 1808 12751 5971 21746 13118 28109 11514 5461 21326 14903 20780 24241 24230 17965 30358 6350 7807 29774 31341 23770 29920 21976 17157 27055 4045 24848 11424 11605 23740 26341 14468 1992 20552 3024 12880 11003 22057 18430 16271 15388 32100 12977 3470 6682 13503 7396 4459 28005 11128 16174 3695 25845 10806 32488 22173 17549 30784 3730 31115 10305 8429 28019 21873 8502 11612 2688 16000 4009 9828 3711 12405 24114 29390 4460 15981 586 26050 16156 13563 26124 4480 12064 8824 7992 12881 29676 14746 6013 17852 9529 16968 2552 8700 27047 3245 31224 8626 17319 6796 1796 14296 9624 29395 16927 32758 4400 21362 26698 7832 17950 13677 9803 1535 17389 22320 18861 29463 9004 16676 776 13993 2508 30847 7554 7890 16073 21600 20374 5300 8461 10523 2324 17003 1425 15825 25193 21826 16581 16453 4385 12327 24370 10986 1450 8797 20829 20801 5723 30321 3823 1633 23476 28015 29972 4567 8680 31317 25633 26289 5016 20936 24697 20694 482 8710 15025 3225 28058 1820 754 31885 13783 3929 19359 29082 4018 25602 32568 14672 11082 27207 19839 26236 10764 28790 29657 2041 18993 15215 25846 16841 27527 2136 5044 31844 20853 22162 25921 7760 17977 3563 3712 1010 885 21781 24751 10660 1002 11351 18758 15368 8835 8322 2338 667 10901 10791 26338 22669 15717 3335 24722 10192 12523 19403 8327 6767 19413 1013 14729 30241 719 17943 28216 22046 6914 15938 1960 14129 21978 16578 32591 2574 2573 3848 33 16359 19929 26856 761 1338 5773 15877 2619 7917 29002 5001 16567 7580 14603 29209 32721 23346 28131 28680 223 30390 4486 11210 15827 17000 11531 19116 17690 22541 25584 20419 1220 18311 14431 24354 20708 5032 25775 16720 10554 29000 28206 15098 527 31893 21790 3300 5217 22699 1533 12325 31040 28005 28682 8852 5240 14194 25860 22398 4720 24235 9781 8346 32082 1699 17071 9022 11628 3456 10966 11730 6359 11530 7454 390 18006 12802 20502 25664 26496 15797 32166 3800 24098 17873 13204 19336 12511 18038 3405 22686 13081 23184 15075 25460 25621 11877 31058 12194 18777 106 24583 15986 18521 9307 10147 2709 17235 15498 19964 11284 32120 7544 15204 30857 2636 4002 19759 29582 19535 25501 26991 9078 18141 3173 2 7739 18259 13299 7375 25272 10611 32269 28492 21016 15114 28002 27460 23951 719 7894 26327 15432 28303 20251 22978 15774 11399 24193 8672 2958 13330 18191 28913 29036 16772 14546 18331 31446 18773 20597 8622 32487 9858 7186 14252 27564 2096 25160 31113 26076 25234 24462 9903 4291 26382 28102 10053 4736 | <br>4291 26382 28102 10053 4736<br>- |
*pass*
5. Large input with minimal K

| Input                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Output                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 1 5 500<br>32655 3336 28724 8347 19352 30774 774 9292 24653 24066 23593 5776 14520 20052 2225 22968 20794 10786 9104 9091 13541 2881 28181 17297 31270 24187 20914 11802 29351 25569 12034 7904 4723 2402 7690 26722 21084 21405 960 17235 4506 18676 8393 12874 15028 4834 32135 19899 12196 18830 22355 3110 16694 7584 29447 23182 13745 21587 30912 6445 8889 18891 15586 20582 7796 28074 6748 12795 3052 28126 16941 14976 1716 956 9255 1808 12751 5971 21746 13118 28109 11514 5461 21326 14903 20780 24241 24230 17965 30358 6350 7807 29774 31341 23770 29920 21976 17157 27055 4045 24848 11424 11605 23740 26341 14468 1992 20552 3024 12880 11003 22057 18430 16271 15388 32100 12977 3470 6682 13503 7396 4459 28005 11128 16174 3695 25845 10806 32488 22173 17549 30784 3730 31115 10305 8429 28019 21873 8502 11612 2688 16000 4009 9828 3711 12405 24114 29390 4460 15981 586 26050 16156 13563 26124 4480 12064 8824 7992 12881 29676 14746 6013 17852 9529 16968 2552 8700 27047 3245 31224 8626 17319 6796 1796 14296 9624 29395 16927 32758 4400 21362 26698 7832 17950 13677 9803 1535 17389 22320 18861 29463 9004 16676 776 13993 2508 30847 7554 7890 16073 21600 20374 5300 8461 10523 2324 17003 1425 15825 25193 21826 16581 16453 4385 12327 24370 10986 1450 8797 20829 20801 5723 30321 3823 1633 23476 28015 29972 4567 8680 31317 25633 26289 5016 20936 24697 20694 482 8710 15025 3225 28058 1820 754 31885 13783 3929 19359 29082 4018 25602 32568 14672 11082 27207 19839 26236 10764 28790 29657 2041 18993 15215 25846 16841 27527 2136 5044 31844 20853 22162 25921 7760 17977 3563 3712 1010 885 21781 24751 10660 1002 11351 18758 15368 8835 8322 2338 667 10901 10791 26338 22669 15717 3335 24722 10192 12523 19403 8327 6767 19413 1013 14729 30241 719 17943 28216 22046 6914 15938 1960 14129 21978 16578 32591 2574 2573 3848 33 16359 19929 26856 761 1338 5773 15877 2619 7917 29002 5001 16567 7580 14603 29209 32721 23346 28131 28680 223 30390 4486 11210 15827 17000 11531 19116 17690 22541 25584 20419 1220 18311 14431 24354 20708 5032 25775 16720 10554 29000 28206 15098 527 31893 21790 3300 5217 22699 1533 12325 31040 28005 28682 8852 5240 14194 25860 22398 4720 24235 9781 8346 32082 1699 17071 9022 11628 3456 10966 11730 6359 11530 7454 390 18006 12802 20502 25664 26496 15797 32166 3800 24098 17873 13204 19336 12511 18038 3405 22686 13081 23184 15075 25460 25621 11877 31058 12194 18777 106 24583 15986 18521 9307 10147 2709 17235 15498 19964 11284 32120 7544 15204 30857 2636 4002 19759 29582 19535 25501 26991 9078 18141 3173 2 7739 18259 13299 7375 25272 10611 32269 28492 21016 15114 28002 27460 23951 719 7894 26327 15432 28303 20251 22978 15774 11399 24193 8672 2958 13330 18191 28913 29036 16772 14546 18331 31446 18773 20597 8622 32487 9858 7186 14252 27564 2096 25160 31113 26076 25234 24462 9903 4291 26382 28102 10053 4736 | -<br>4291 26382 28102 10053 4736 |
*pass*
# Chapter 4: Analysis and Comments

**Time Complexity:**

- **push:** The time complexity of pushing an element into the queue is $O(1)$ on average.
- **deleteAll:** The time complexity of removing all occurrences of a value from the queue is $O(n)$.
- **inQueue:** The time complexity of checking if a value exists in the queue is $O(n)$.
- **show:** The time complexity of displaying the contents of the queue is $O(n)$.

**Space Complexity:**

- The space complexity of the algorithm is $O(m)$, where $m$ is the maximum size of the queue. This accounts for the space required to store the queue elements.

**Possible Optimizations:**

1. **Data Structure Optimization:** Implementing the queue using a doubly linked list allows for efficient insertion and deletion operations, but further optimizations could be explored, such as using a circular buffer for better memory utilization and faster access times.
2. **Caching Strategies:** Fine-tuning the parameter K in the LRU-K algorithm could lead to better cache utilization, depending on the application's access patterns.
3. **Efficient Eviction Policies:** Implementing smarter eviction policies, such as considering both recency and frequency of access, could further improve cache performance.
4. **Memory Management:** Implementing memory pooling or using a memory allocator optimized for the specific use case could reduce memory fragmentation and improve overall memory utilization.
# Appendix:  Source Code (in C)
```c
#include <stdio.h>

#include <stdlib.h>

// Counter to track hits for each data in cache

int hitCnt[20002];

// Maximum size of cache

int maxSize;

  

// Define node structure

typedef struct node

{

    int data;

    struct node *next;

}*Node;

  

// Define queue structure

typedef struct queue

{

    int size;

    Node first;

    Node last;

}*Queue;

  

// Initialize a queue

Queue init(){

    Queue Q = (Queue)malloc(sizeof(Queue));

    Q->size = 0;

    Q->first = NULL;

    Q->last = NULL;

    return Q;

}

  

// Enqueue operation

void push(Queue Q, int x){

    Node new = (Node)malloc(sizeof(Node));

    new->data = x;

    new->next = NULL;

    // If the queue is empty

    if(Q->size == 0){

        Q->first = new;

        Q->last = new;

        Q->size++;

    } else if (Q->size < maxSize) { // If the queue is not full

        // Add the new node to the end of the queue

        Q->last->next = new;

        Q->last = new;

        Q->size++;

    } else if (Q->size == maxSize){ // If the queue is full

        // Add the new node to the end of the queue and remove the oldest node

        Q->last->next = new;

        Q->last = new;

        hitCnt[Q->first->data] = 0; // Reset hit count for the evicted data

        Q->first = Q->first->next;

    }

}

  

// Display the queue

void show(Queue Q){

    Node n = Q->first;

    int cnt = 0;

    while(n){

        if(cnt == Q->size - 1){ // end of the queue

            printf("%d", n->data);

        } else { // middle of the queue

            printf("%d ", n->data);

        }

        cnt++;

        n = n->next;

    }

    if(Q->size == 0){ // empty queue

        printf("-");

    }

    printf("\n");

}

  

// Delete all occurrences of a data from the queue

void deleteAll(Queue Q, int x){

    if(Q->size == 1 && Q->first->data == x) { // only element in queue

        Q->size = 0;

        Q->first = NULL;

        Q->last = NULL;

    }

    Node n = Q->first;

    while (n && n->data == x)

    {

        n = n->next;

        Q->first = n;

        Q->size--;

    }

    while (n)

    {

        if(!n->next){ // reach end of queue

            return;

        }

        if(n->next->data == x) { // delete this single node

            n->next = n->next->next;

            Q->size--;

        }

        n = n->next;

    }

    // rearrange the Q structure

    n = Q->first;

    while(n && n->next){

        n = n->next;

    }

    Q->last = n;

}

  

// Check if a data is in the queue

int inQueue(Queue Q, int x){

    Node n = Q->first;

    while(n){

        if(n->data == x){

            return 1;

        }

        n = n->next;

    }

    return 0;

}

  

// Main function

int main() {

    int K, numbers;

    // read data

    scanf("%d %d %d", &K, &maxSize, &numbers);

    Queue history = init();

    Queue cache = init();

    for (int i = 0; i < numbers; i++)

    {

        int x;

        scanf("%d", &x);

        if (inQueue(cache, x)) // Move to cache if already in cache

        {

            deleteAll(cache, x);

            push(cache, x);

        } else if(hitCnt[x] == K - 1) { // Move to cache if hit count reaches K - 1

            deleteAll(history, x);

            push(cache, x);

        } else { // Move to history and update hit cnt

            deleteAll(history, x);

            push(history, x);

            hitCnt[x]++;

        }

    }

    // print both queue

    show(history);

    show(cache);

}
```