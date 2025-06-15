# Counting Triangles in Graphs

---

## Problem

Given a graph with `n` nodes, count the number of triangles (3-node cycles) in the graph.

---

## Using Adjacency Matrix

Let **A** be the adjacency matrix of the graph, where:

- `A[i][j] = 1` if there is an edge from node `i` to node `j`
- `A[i][j] = 0` otherwise

---

## Method

1. Compute the cube of the adjacency matrix, \( A^3 \).
2. The diagonal entries of \( A^3 \) count the number of walks of length 3 starting and ending at the same node.

---

## Triangle Counting Formulas

| Graph Type   | Triangle Count Formula      | Explanation                               |
|--------------|-----------------------------|-------------------------------------------|
| Undirected   | \(\frac{trace(A^3)}{6}\)    | Each triangle counted 6 times due to 3 nodes × 2 directions |
| Directed     | \(\frac{trace(A^3)}{3}\)    | Each triangle counted 3 times due to 3 cyclic permutations |

---

## Summary

- **Undirected Graphs**: divide trace of \( A^3 \) by 6 to get the number of triangles.
- **Directed Graphs**: divide trace of \( A^3 \) by 3 to get the number of triangles.

---

## Code Placeholder

```cpp
int countTriangle(int graph[V][V], 
                  bool isDirected)
{
    // Initialize result
    int count_Triangle = 0;

    // Consider every possible
    // triplet of edges in graph
    for (int i = 0; i < V; i++)
    {
        for (int j = 0; j < V; j++)
        {
            for (int k = 0; k < V; k++)
            {
               // Check the triplet if
               // it satisfies the condition
               if (graph[i][j] && graph[j][k] 
                               && graph[k][i])
                  count_Triangle++;
             }
        }
    }

    // If graph is directed , 
    // division is done by 3,
    // else division by 6 is done
    isDirected? count_Triangle /= 3 :
                count_Triangle /= 6;

    return count_Triangle;
}

```

