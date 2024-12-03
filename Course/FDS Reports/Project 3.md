
<div align=center STYLE="page-break-after: always;">
<!--敲几个回车-->
    <br/><br/><br/><br/><br/><br/><br/><br/><br/>
<!--标题调整，在这里选择你想要的字号和字体-->
    <font size=15 face="微软雅黑">
        Dijkstra sequence<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/><br/>
    <font size=6 face="微软雅黑">
        陈宇涵<!--内容自己改-->
    </font>
<!--敲几个回车-->
    <br/><br/>
    <font size = 4>
        Date: 2024-04-24<!--内容自己改--><br/>
        <br/><br/><br/><br/><br/><br/>
    </font>
</div>

# Chapter 1:  Introduction

*Problem description and (if any) background of the algorithms.*

A **Dijkstra sequence**, is generated by Dijkstra's algorithm. In this algorithm, during each step, we find one vertex which is not yet included and has a minimum distance from the source, and collect it into the set. Hence step by step generate an ordered sequence of vertices. 

For a given graph, there could be more than one Dijkstra sequence. 

This algorithm first constructs a connected undirected graph by reading number of vertices and edges and then every edge of this graph.

After that, it checks whether several given sequences are Dijkstra sequences or not.

# Chapter 2:  Algorithm Specification

*Description (pseudo-code preferred) of all the algorithms involved for solving the problem, including specifications of main data structures.*

## Functions Description

### 1. `set_edge()`

- **Description**: This function adds a weighted edge between two vertices in the graph.
- **Parameters**:
    - `graph[]`: Array of vertex pointers representing the graph.
    - `v1`: Index of the first vertex.
    - `v2`: Index of the second vertex.
    - `weight`: Weight of the edge between `v1` and `v2`.
- **Pseudo-code**:
```
set_edge(graph[], v1, v2, weight):
    // Create a new edge from v1 to v2 with the given weight
    e = new Edge
    e.Weight = weight
    e.To = graph[v2]
    e.NextEdge = null
    
    // Insert the edge into the adjacency list of v1
    v = graph[v1]
    if v.Firstedge is null:
        set its firstedge to e
    else:
        find its last edge and set its next edge to e
```

### 2. `Dijkstra()`

- **Description**: This function computes the shortest paths from a specified source vertex using Dijkstra's algorithm.
- **Parameters**:
    - `source`: Index of the source vertex.
    - `G[]`: Array of vertex pointers representing the graph.
    - `T`: Table containing distance, checked status, and path information.
- **Pseudo-code**:
```
Dijkstra(source, G[], T):
    Initialize T.Distance[source] = 0
    Initialize T.Checked[] = 0 for all vertices
    Initialize T.Path[] = -1 for all vertices
    
    while there are unchecked vertices:
        min_i = vertex with the smallest T.Distance[] among unchecked vertices
        Mark min_i as checked
        
        For each vertex connected with this vertex:
	        If it's unchecked:
		        Set its distance to min{its distance, min_distance + edge weight}
```

### 3. `main()`

- **Description**: The main function responsible for reading input, constructing the graph, and processing queries.
- **Pseudo-code**:
```
main():
    Read Nv (number of vertices) and Ne (number of edges)
    
    Initialize graph[] with Nv+1 vertex pointers
    Initialize Tlist[] with Nv+1 table pointers
    
    for i from 1 to Nv:
        Create a new vertex v with index i
        Set v.Firstedge to null
        graph[i] = v
    
    for each edge (v1, v2, weight) from 1 to Ne:
        set_edge(graph, v1, v2, weight)
        set_edge(graph, v2, v1, weight)
    
    Read k (number of test cases)
    
    for each test case:
        Read source
        
        if Tlist[source] is null:
            Initialize T as a new table
            Perform Dijkstra(source, graph, T)
            Tlist[source] = T
        
        Read and process the path sequence
        Check if the given path sequence is a Dijkstra sequence
        Output the result (Yes/No)
```

## Main Data Structures Description

### 1. `Vertex` Structure

- **Description**: Represents a vertex in the graph with an index and a pointer to its first adjacent edge.
- **Fields**:
    - `Index`: Integer representing the vertex index.
    - `Firstedge`: Pointer to the first edge in the adjacency list of the vertex.

### 2. `Edge` Structure

- **Description**: Represents an edge connecting two vertices with a weight and a pointer to the next edge in the adjacency list.
- **Fields**:
    - `Weight`: Integer representing the weight of the edge.
    - `To`: Pointer to the vertex this edge points to.
    - `NextEdge`: Pointer to the next edge in the adjacency list.

### 3. `Table` Structure

- **Description**: Stores information related to Dijkstra's algorithm: distance from a source vertex, 'checked' status of vertices, and predecessor in the shortest path.
- **Fields**:
    - `Distance[Max + 1]`: Array storing the shortest distance from the source vertex to each vertex.
    - `Checked[Max + 1]`: Array marking whether a vertex has been visited during Dijkstra's algorithm.
    - `Path[Max + 1]`: Array storing the predecessor vertex in the shortest path to each vertex.

# Chapter 3:  Testing Results

*Table of test cases.  Each test case usually consists of a brief description of the purpose of this case, the expected result, the actual behavior of your program, the possible cause of a bug if your program does not function as expected, and the current status (“_pass_”, or “_corrected_”, or “_pending_”).

1. Same as Sample.

| Input                                                                                                                       | Output                  |
| --------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| 5 7<br>1 2 2<br>1 5 1<br>2 3 1<br>2 4 1<br>2 5 2<br>3 5 1<br>3 4 1<br>4<br>5 1 3 4 2<br>5 3 1 2 4<br>2 3 4 5 1<br>3 2 1 5 4 | Yes<br>Yes<br>Yes<br>No |
_pass_

2. A line-like graph

| Input                                                                                   | Output           |
| --------------------------------------------------------------------------------------- | ---------------- |
| 5 4<br>1 2 10<br>2 3 20<br>3 4 30<br>4 5 50<br>3<br>1 2 3 4 5<br>1 3 2 4 5<br>5 4 3 2 1 | Yes<br>No<br>Yes |
_pass_

3. A hand-like graph

| Input                                                                                            | Output                  |
| ------------------------------------------------------------------------------------------------ | ----------------------- |
| 5 4<br>1 2 1<br>1 3 1<br>1 4 1<br>1 5 1<br>4<br>1 2 3 4 5<br>1 3 2 5 4<br>5 1 2 3 4<br>5 2 3 4 1 | Yes<br>Yes<br>Yes<br>No |
_pass_

4. Max input

| Input           | Output |
| --------------- | ------ |
| see "input.txt" | No     |
_pass_

5. Minimum input

| Input                            | Output     |
| -------------------------------- | ---------- |
| 2 1<br>1 2 10<br>2<br>1 2<br>2 1 | Yes<br>Yes |
_pass_
# Chapter 4:  Analysis and Comments

*Analysis of the time and space complexities of the algorithms.  Comments on further possible improvements.

## Time Complexity

1. `set_edge`
O($1$) on average for inserting an edge into the adjacency list of a vertex. However, in the worst-case scenario (e.g., when adding edges to a vertex with many existing edges), the time complexity could be O($E$), where E is the number of edges.

2. `Dijsktra`
O($V$) for finding the unchecked vertex with smallest distance.
O($V$) for setting the distance of every vertex connected with that vertex.
O($V^2$) for total time complexity.

3. Check sequence
The algorithm uses the distance found by `Dijsktra` function of every vertex in the sequence, so the total time complexity is O($V \times K$) .

## Space Complexity

For every edge or vertex in the graph, create a struct node to store it, making it O($E + V$).
For every source vertex in each sequence, a table storing data is constructed, making it O($V \times K$).

# Appendix:  Source Code (in C)

*At least 30% of the lines must be commented.  Otherwise the code will NOT be evaluated.*

```c
#include <stdio.h>
#include <stdlib.h>

#define Max 1000    // Max total number of vertices
#define Inf 114514  // Represent the infinite edge weight

typedef struct edge *Edge;
typedef struct vertex *Vertex;
typedef struct table *Table;
int Nv, Ne;
int mode = 0; // If you want to see detailed information while the program runs, change this to 1

// a vertex in the graph
struct vertex {
    int Index;
    Edge Firstedge; // Pointer to the first edge from this vertex
};

// an edge connecting two vertices
struct edge {
    int Weight;
    Vertex To;       // Pointer to the vertex this edge points to
    Edge NextEdge;   // Pointer to the next edge in the adjacency list
};

// a table to store distance, checked status, and paths
struct table {
    int Distance[Max + 1]; // store shortest distances
    int Checked[Max + 1];  // mark vertices as visited
    int Path[Max + 1];     // store previous vertex in the shortest path
};

// Function to add an edge between two vertices with a specified weight
void set_edge(Vertex graph[], int v1, int v2, int weight) {
    Edge e = (struct edge *)malloc(sizeof(struct edge));
    e->Weight = weight;
    e->To = graph[v2];
    e->NextEdge = NULL;
    Vertex v = graph[v1];
    if (!v->Firstedge) {
        v->Firstedge = e;
    } else {
        Edge t = v->Firstedge;
        while (t->NextEdge) {
            t = t->NextEdge;
        }
        t->NextEdge = e;
    }
}

// Dijkstra's algorithm to find shortest paths from a specific source vertex
void Dijkstra(int source, Vertex G[], Table T) {
    int min_i = 0, min_distance = 0;
    T->Distance[source] = 0;
    while (1) {
        // Find the unchecked vertex with the minimum distance and mark it as checked
        min_i = 0;
        min_distance = Inf;
        for (int i = 1; i <= Nv; i++) {
            if (!T->Checked[i] && T->Distance[i] <= min_distance) {
                min_distance = T->Distance[i];
                min_i = i;
            }
        }
        if (!min_i)
            return; // No more vertices to process

        Vertex V = G[min_i];
        T->Checked[min_i] = 1; // Mark this vertex as visited
        if (mode)
            printf("Checking v%d:\n", min_i);

        if (V->Firstedge) {
            Edge E = V->Firstedge;
            while (E) {
                int to = E->To->Index;
                int weight = E->Weight;
                if (mode)
                    printf("Checking v%d to v%d: ", min_i, to);
                
                if (!T->Checked[to]) { // Update distance if a shorter path is found
                    if (T->Distance[min_i] + weight < T->Distance[to]) {
                        T->Distance[to] = T->Distance[min_i] + weight;
                        T->Path[to] = min_i;
                        if (mode)
                            printf("Update distance to %d\n", T->Distance[to]);
                    } else { // No need to update the distance
                        if (mode)
                            printf("No shorter path than before (%d)\n", T->Distance[to]);
                    }
                } else { // The vertex is already visited
                    if (mode)
                        printf("Already processed\n");
                }
                E = E->NextEdge; // Move to the next edge
            }
        }
    }
    return;
}

// Main function to read input, build the graph, and execute Dijkstra's algorithm
int main() {
    scanf("%d %d", &Nv, &Ne); // Read number of vertices and edges

    Vertex graph[Nv + 1]; // Array of vertex pointers (1-based indexing)
    for (int i = 1; i <= Nv; i++) { // Initialize each vertex in the graph
        Vertex v = (struct vertex *)malloc(sizeof(struct vertex));
        v->Firstedge = NULL;
        v->Index = i;
        graph[i] = v;
    }
    
    // Read edge information and construct the graph
    for (int i = 0; i < Ne; i++) {
        int v1, v2, weight;
        scanf("%d %d %d", &v1, &v2, &weight);
        set_edge(graph, v1, v2, weight);
        set_edge(graph, v2, v1, weight); // Set both edges for the undirected graph
    }

    Table Tlist[Nv + 1]; // Array of tables to store Dijkstra's results for each source vertex
    for (int i = 1; i <= Nv; i++) {
        Tlist[i] = NULL;
    }
    
    int k, source, valid = 1;
    scanf("%d", &k); // Read number of test cases
    for (int i = 0; i < k; i++) {
        scanf("%d", &source); // Read source vertex for this test case
        if (!Tlist[source]) {
            if (mode)
                printf("Generating Dijkstra Table for v%d\n", source);
            Table T = (struct table *)malloc(sizeof(struct table));
            for (int i = 1; i <= Max; i++) {
                T->Checked[i] = 0;
                T->Distance[i] = Inf;
                T->Path[i] = -1;
            }
            Dijkstra(source, graph, T); // Compute shortest paths from the source vertex
            Tlist[source] = T;
        }

        int d1, d2, v1, v2;
        v1 = source;
        valid = 1;
        /*
        Notice that if a Dijkstra sequence is changed to the distance sequence,
        then it must be non-decreasing.
        */
        for (int j = 0; j < Nv - 1; j++) {
            scanf("%d", &v2); // Read next vertex in the test path
            d1 = Tlist[source]->Distance[v1];
            d2 = Tlist[source]->Distance[v2];
            // Compare the distance of two vertices starting from source vertex
            if (d1 <= d2) {
                // The distance is non-decreasing, so the sequence part is valid
            } else {
                valid--; // Invalid sequence
            }
            v1 = v2;
        }
        if (valid) {
            printf("Yes\n"); // The entire path is valid
        } else {
            printf("No\n"); // The path contains invalid segments
        }
    }
    return 0;
}
```

# Declaration

**_I hereby declare that all the work done in this project titled "Dijkstra Sequence" is of my independent effort._**