---
sources:
  - https://www.youtube.com/playlist?list=PLDV1Zeh2NRsDGO4--qE8yH72HFL1Km93P
---

A graph is a non-linear data structure consisting of **nodes (vertices)** connected by **edges**. Graphs are used to represent relationships, networks, and connections between entities.

# Components

- **Vertex (Node)**: A fundamental unit representing an entity
- **Edge**: A connection between two vertices
- **Weight**: Optional value associated with an edge (in weighted graphs)
- **Path**: Sequence of vertices connected by edges
- **Cycle**: Path that starts and ends at the same vertex

# Types of Graphs

## By Edge Direction

- **Directed Graph (Digraph)**: Edges have a direction (A → B)
- **Undirected Graph**: Edges have no direction (A ↔ B)

## By Edge Weights

- **Weighted Graph**: Edges have associated weights/costs
- **Unweighted Graph**: All edges are treated equally

## By Connectivity

- **Connected Graph**: Path exists between every pair of vertices
- **Disconnected Graph**: Some vertices are unreachable from others
- **Strongly Connected**: Every vertex is reachable from every other (directed graphs)

## Special Types

- **Cyclic Graph**: Contains at least one cycle
- **Acyclic Graph**: Contains no cycles
- **DAG (Directed Acyclic Graph)**: Directed graph with no cycles
- **Tree**: Connected acyclic undirected graph
- **Complete Graph**: Every pair of vertices is connected

# Graph Representations

## 1. Adjacency Matrix

A 2D array where `matrix[i][j]` indicates if there's an edge from vertex i to vertex j.

```cpp
vector<vector<int>> adjMatrix(n, vector<int>(n, 0));

// Add edge from u to v
adjMatrix[u][v] = 1;
// For undirected graph
adjMatrix[v][u] = 1;

// For weighted graph
adjMatrix[u][v] = weight;
```

**Space Complexity**: [[O(V²)]]
**Good for**: Dense graphs, checking if edge exists [[O(1)]]

## 2. Adjacency List

An array of lists where each vertex has a list of its neighbors.

```cpp
vector<vector<int>> adjList(n);

// Add edge from u to v
adjList[u].push_back(v);
// For undirected graph
adjList[v].push_back(u);

// For weighted graph
vector<vector<pair<int, int>>> adjList(n);  // {neighbor, weight}
adjList[u].push_back({v, weight});
```

**Space Complexity**: [[O(V + E)]]
**Good for**: Sparse graphs, iterating through neighbors

## 3. Edge List

List of all edges in the graph.

```cpp
vector<pair<int, int>> edges;  // {from, to}
edges.push_back({u, v});

// For weighted graph
vector<tuple<int, int, int>> edges;  // {from, to, weight}
edges.push_back({u, v, weight});
```

# Common Algorithms

## Graph Traversal

### Breadth-First Search (BFS)

```cpp
void bfs(vector<vector<int>>& graph, int start) {
    vector<bool> visited(graph.size(), false);
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << node << " ";
        
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

Time: [[O(V + E)]], Space: [[O(V)]]

### Depth-First Search (DFS)

```cpp
void dfs(vector<vector<int>>& graph, int node, vector<bool>& visited) {
    visited[node] = true;
    cout << node << " ";
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(graph, neighbor, visited);
        }
    }
}
```

Time: [[O(V + E)]], Space: [[O(V)]]

## Shortest Path Algorithms

- **Dijkstra's Algorithm**: Single-source shortest path (non-negative weights)
- **Bellman-Ford**: Single-source shortest path (handles negative weights)
- **Floyd-Warshall**: All-pairs shortest path
- **A* Algorithm**: Heuristic-based shortest path

## Minimum Spanning Tree

- **Kruskal's Algorithm**: Greedy approach using [[Union Find]]
- **Prim's Algorithm**: Greedy approach using priority queue

## Other Important Algorithms

- **Topological Sort**: Linear ordering of vertices in DAG
- **Cycle Detection**: Find if graph contains cycles
- **Strongly Connected Components**: Kosaraju's or Tarjan's algorithm
- **Bipartite Check**: Determine if graph can be colored with 2 colors

# Applications

- Social networks (friends, followers)
- Maps and navigation (roads, routes)
- Computer networks (routers, connections)
- Dependency graphs (build systems, course prerequisites)
- Web crawling (pages, links)
- Recommendation systems
- Circuit design
- Biology (protein interactions, neural networks)

# Complexity Comparison

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|----------------|
| Add Edge | [[O(1)]] | [[O(1)]] |
| Remove Edge | [[O(1)]] | [[O(V)]] |
| Check Edge | [[O(1)]] | [[O(V)]] |
| Get Neighbors | [[O(V)]] | [[O(degree)]] |
| Space | [[O(V²)]] | [[O(V + E)]] |

# Related Topics

- [[BFS]] - Breadth-First Search
- [[DFS]] - Depth-First Search
- [[Topological Sort]]
- [[Union Find]]
- [[Dijkstra's Algorithm]]
- [[Minimum Spanning Tree]]
