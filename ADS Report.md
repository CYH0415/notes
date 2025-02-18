# 1 Abstract
Skip list is a data structure that supports both searching, insertion, and deletion in $O(logN)$ expected time. In this report, we implement insertion, deletion, and searching in skip lists. Skip lists achieve efficiency by introducing multiple levels of linked lists where higher levels serve as shortcuts for faster navigation.
# 2 Data structure
The skip list data structure consists of:

1. **Node**: A node stores a key and an array of forward pointers, where each pointer points to the next node at a specific level.
2. **Skip List**: A skip list maintains a header node, which serves as the starting point for all levels. The structure includes:
    - `level`: The maximum level currently in use by the skip list.
    - `header`: A dummy node that acts as the starting point of all linked lists at different levels.

The skip list allows insertion, deletion, and searching operations while using a **randomized level generator** to maintain an expected balanced height.
# 3 Algorithm analysis
## 3.1 Insert
To insert a key into a skip list:

1. Start from the top-most level of the header node.
2. Traverse forward until the key's correct position is found. Track the nodes at each level where pointers need to be updated.
3. Generate a random level for the new node. If the new level exceeds the current maximum, update the `header` node's pointers.
4. Insert the new node and update the pointers at each level.
### 3.1.1 Pseudocode
```
function insert(list, key):
create array update[MAX_LEVEL] to store nodes to update
current = list.header

for i from list.level - 1 down to 0:
    while current.forward[i] != null and current.forward[i].key < key:
        current = current.forward[i]
    update[i] = current

current = current.forward[0]

if current == null or current.key != key:
    level = random_level()
    if level > list.level:
        for i from list.level to level - 1:
            update[i] = list.header
        list.level = level

    new_node = create_node(level, key)
    for i from 0 to level - 1:
        new_node.forward[i] = update[i].forward[i]
        update[i].forward[i] = new_node
```
## 3.2 Search
To search for a key:

1. Start from the top-most level of the header node.
2. Move forward until the current node's key is greater than or equal to the target key. If the key is not found at the current level, move down to the next level.
3. Repeat until the bottom-most level is reached.
### 3.2.1 Pseudocode
```
function search(list, key):
current = list.header

for i from list.level - 1 down to 0:
    while current.forward[i] != null and current.forward[i].key < key:
        current = current.forward[i]

current = current.forward[0]

if current != null and current.key == key:
    return current

return null
```
## 3.3 Delete
To delete a key:

1. Start from the top-most level of the header node.
2. Traverse forward while recording the nodes where pointers need to be updated.
3. If the key is found, remove the node by updating the pointers. Adjust the skip list level if necessary.
### 3.3.1 Pseudocode
```
function delete_key(list, key):
create array update[MAX_LEVEL] to store nodes to update
current = list.header

for i from list.level - 1 down to 0:
    while current.forward[i] != null and current.forward[i].key < key:
        current = current.forward[i]
    update[i] = current

current = current.forward[0]

if current != null and current.key == key:
    for i from 0 to list.level - 1:
        if update[i].forward[i] != current:
            break
        update[i].forward[i] = current.forward[i]

    free(current)

    while list.level > 0 and list.header.forward[list.level - 1] == null:
        list.level--
```
# 4 Test Result and Analysis

## 4.1 Test result
The detailed runtime test result is shown in `runtime.txt`
![[Figure_1.png]]
## 4.2 Time complexity
### 4.2.1 Formal proof
To analyze the search path in a skip list, the process can be divided into two parts: climbing from the bottom level to the top level $L(n)$ and the subsequent operations. Before visiting a node, its detailed information (e.g., maximum level) is assumed to be unknown.

Suppose we are at a node $x$ on level $i$. We only know that x's maximum level is at least i. If $x$'s maximum level exceeds $i$, the next step is upward with probability $p$. Otherwise, the next step is leftward with probability $1-p$.

Let $C(i)$ represent the expected cost of climbing i levels in an infinite skip list. Then:

$$C(0) = 0, C(i) = (1-p)(1+C(i)) + p(1+C(i-1))$$

Solving this gives:

$$C(i) = \frac{i}{p}$$

Thus, in a skip list of length $n$, the expected cost of climbing from the bottom to level $L(n)$ is bounded by:

$$\frac{L(n) - 1}{p}$$

Next, consider the cost after reaching level $L(n)$:

- The expected number of leftward steps is bounded by the total number of nodes at level $L(n)$ and above, which is $1 / p$.
- Similarly, the expected number of upward steps is also bounded by $1 / p$.

Therefore, the total expected number of steps for a search is:

$$\frac{L(n) - 1}{p} + \frac{2}{p}$$

Since $L(n) = \log_{\frac{1}{p}} n$, the expected time complexity of a skip list search is $O(logN)$.

From the figure above, it can be seen that the runtime generally fits the $O(logN)$ complexity.
### 4.2.2 Result analysis
![[Pasted image 20241217210805.png]]
The results of the linear regression analysis are as follows:

- **Slope**: $3.34 \times 10^{-5}$
- **Intercept**: $-1.78 \times 10^{-4}$
- **R² (Coefficient of Determination)**: $0.9125$
    This indicates that 91.25% of the variation in the running time $T$ is explained by the logarithmic transformation of $N$, suggesting a strong correlation.
- **Standard Error of the Slope**: $1.36 \times 10^{-6}$


The data shows a strong fit to an $O(log N)$ relationship based on the R² value. The graph also visually supports this conclusion, as the trend line closely follows the scatter plot of $T$ vs. $logN$. ​
## 4.3 Space complexity
For a node, the probability of the node having a highest level of $i$ is $p^{i-1}(1 - p)$. Therefore, the expected number of levels in a skip list is:
$$\sum_{i \geq 1} i p^{i-1}(1-p) = \frac{1}{1 - p}$$
And since $p$ is a constant, the expected space complexity of the skip list is $O (n)$.

In the worst-case scenario, each level of the ordered linked list equals the initial ordered linked list, resulting in the worst-case space complexity of $O (n \log n)$.
# 5 Appendix
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <math.h>
  
#define MAX_LEVEL 16  // Maximum level for the skip list
  
// Node structure for the skip list

typedef struct SkipNode {

    int key;

    struct SkipNode *forward[MAX_LEVEL];

} SkipNode;

  

// Skip list structure

typedef struct SkipList {

    int level;  // Current level of the skip list

    SkipNode *header;

} SkipList;

  

// Function to create a new node

SkipNode *create_node(int level, int key) {

    SkipNode *node = (SkipNode *)malloc(sizeof(SkipNode));

    node->key = key;

    for (int i = 0; i < level; i++) {

        node->forward[i] = NULL;

    }

    return node;

}

  

// Function to initialize a skip list

SkipList *create_skip_list() {

    SkipList *list = (SkipList *)malloc(sizeof(SkipList));

    list->level = 0;

    list->header = create_node(MAX_LEVEL, 0);

    return list;

}

  

// Random level generator for the skip list

int random_level() {

    int level = 1;

    while ((rand() & 1) && level < MAX_LEVEL - 1) {

        level++;

    }

    return level;

}

  

// Search for a key in the skip list

SkipNode *search(SkipList *list, int key) {

    SkipNode *current = list->header;

    for (int i = list->level - 1; i >= 0; i--) {

        while (current->forward[i] != NULL && current->forward[i]->key < key) {

            current = current->forward[i];  // Move forward in the current level

        }

    }

    current = current->forward[0];  // Move to the bottom-most level

    if (current != NULL && current->key == key) {

        return current;  // Key found

    }

    return NULL;  // Key not found

}

  

// Insert a key into the skip list

void insert(SkipList *list, int key) {

    SkipNode *update[MAX_LEVEL];  // Array to track nodes to update at each level

    SkipNode *current = list->header;

    for (int i = list->level - 1; i >= 0; i--) {

        while (current->forward[i] != NULL && current->forward[i]->key < key) {

            current = current->forward[i];  // Move forward in the current level

        }

        update[i] = current;  // Store the last node before insertion point at this level

    }

    // Move last level pointer to target node

    current = current->forward[0];

  

    if (current == NULL || current->key != key) {  // Key is not already in the skip list

        int level = random_level();  // Determine the random level for the new node

        if (level > list->level) {

            for (int i = list->level; i < level; i++) {

                update[i] = list->header;  // Update higher levels to point to header

            }

            list->level = level;  // Increase the current level of the skip list

        }

        SkipNode *new_node = create_node(level, key);

        for (int i = 0; i < level; i++) {

            new_node->forward[i] = update[i]->forward[i];  // Update pointers at each level

            update[i]->forward[i] = new_node;

        }

    }

}

  

// Delete a key from the skip list

void delete_key(SkipList *list, int key) {

    SkipNode *update[MAX_LEVEL];  // Array to track nodes to update at each level

    SkipNode *current = list->header;

    for (int i = list->level - 1; i >= 0; i--) {

        while (current->forward[i] != NULL && current->forward[i]->key < key) {

            current = current->forward[i];  // Move forward in the current level

        }

        update[i] = current;  // Store the last node before the target node

    }

    // Move last level pointer to target node

    current = current->forward[0];

  

    if (current != NULL && current->key == key) {  // Key exists in the skip list

        for (int i = 0; i < list->level; i++) {

            if (update[i]->forward[i] != current)

                break;  // Stop if the level does not point to the current node

            update[i]->forward[i] = current->forward[i];  // Remove the node by updating pointers

        }

        free(current);  // Free the memory allocated for the node

        while (list->level > 0 && list->header->forward[list->level - 1] == NULL) {

            list->level--;  // Reduce the skip list level if the top level is empty

        }

    }

}

  

// Print the skip list

void print_skip_list(SkipList *list) {

    printf("Skip List:\n");

    for (int i = list->level - 1; i >= 0; i--) {

        SkipNode *node = list->header->forward[i];

        printf("Level %d: ", i);

        while (node != NULL) {

            printf("%d ", node->key);

            node = node->forward[i];

        }

        printf("\n");

    }

}

  

// Free the skip list

void free_skip_list(SkipList *list) {

    SkipNode *current = list->header;

    while (current != NULL) {

        SkipNode *temp = current;

        current = current->forward[0];

        free(temp);

    }

    free(list);

}

  

// Test function to measure search time

void test(int scale, FILE *fp) {

    printf("Testing with %d nodes\n", scale);

    SkipList *list = create_skip_list();

    int cycle = 1000000;

    for(int i = 0; i < scale; i++) {

        insert(list, i);

    }

    srand(time(NULL));

    clock_t total = 0;

    clock_t start = clock();

    for(int i = 0; i < cycle; i++) {

        search(list, rand() % scale);

    }

    clock_t end = clock();

    total = end - start;

    free_skip_list(list);

    fprintf(fp, "%d %lf\n", scale, ((double)total / cycle));

}

  
  

int main() {

    FILE *fp = fopen("runtime_1.txt", "w");

    for(int i = 500; i <= 30000; i += 1000) test(i, fp);

    fclose(fp);

    return 0;

}
```