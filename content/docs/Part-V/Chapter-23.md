---
weight: 3400
title: "Chapter 23"
description: "All-Pairs Shortest Paths"
icon: "article"
date: "2024-08-24T23:42:29+07:00"
lastmod: "2024-08-24T23:42:29+07:00"
draft: false
toc: true
katex: true
---

<center>

# 📘 Chapter 23: All-Pairs Shortest Paths

</center>
{{% alert icon="💡" context="info" %}}
<strong>"<em>The greatest value of a picture is when it forces us to notice what we never expected to see.</em>" — John Tukey</strong>
{{% /alert %}}

{{% alert icon="📘" context="success" %}}
<p style="text-align: justify;">
<strong>Chapter 23 of the DSAR book delves into the all-pairs shortest paths problem, a fundamental challenge in graph theory where the goal is to compute the shortest paths between every pair of vertices in a weighted graph. The chapter introduces the Floyd-Warshall algorithm, a dynamic programming approach that provides a straightforward</strong> $O(V^3)$ <strong>solution by iteratively refining a distance matrix to account for all possible intermediate vertices. In contrast, Johnson’s algorithm offers an optimized approach for graphs with negative edge weights by combining edge reweighting with Dijkstra’s algorithm, achieving</strong> $O(V^2 \log V + VE)$ <strong>complexity. The chapter also explores practical use cases such as network routing, GIS, and operations research, and discusses various optimizations to enhance performance, including techniques for handling large graphs and leveraging efficient data structures. By integrating theoretical insights with practical implementations and optimizations, this chapter equips readers with a deep understanding of all-pairs shortest paths algorithms and their applications in real-world scenarios.</strong>
</p>
{{% /alert %}}


# 23.1. Introduction to All-Pairs Shortest Path Problem
<p style="text-align: justify;">
The all-pairs shortest path problem is a fundamental issue in graph theory and computer science, where the goal is to determine the shortest paths between every pair of vertices in a weighted graph. This problem is pivotal in various applications where knowledge of the shortest paths between all possible pairs of nodes is essential, such as in network routing, transportation planning, and optimization problems.
</p>

<p style="text-align: justify;">
To approach the problem, one can use two primary methods for representing the graph: adjacency matrices and adjacency lists. In an adjacency matrix, the graph is represented as a two-dimensional array where each entry $(i,j)$ corresponds to the weight of the edge from vertex $i$ to vertex $j$. If there is no edge between these vertices, the entry is typically set to infinity or a similarly large value. This representation is straightforward and allows for efficient edge weight retrieval, but it is memory-intensive, particularly for sparse graphs where many entries are zero or infinity. Conversely, an adjacency list representation consists of an array where each element corresponds to a vertex and contains a list of its adjacent vertices along with the edge weights. This method is more space-efficient for sparse graphs as it only stores existing edges, though accessing edge weights is slightly more complex.
</p>

<p style="text-align: justify;">
The all-pairs shortest path problem is applicable to both directed and undirected graphs and can handle graphs with both positive and negative edge weights. In directed graphs, edges have a direction, meaning that the shortest path from vertex A to vertex B is not necessarily the same as the shortest path from B to A. In undirected graphs, edges do not have a direction, so the shortest path between two vertices is the same in both directions. The presence of negative edge weights introduces additional complexity, as standard algorithms must be adapted to handle potential negative weight cycles, which can make paths arbitrarily short.
</p>

<p style="text-align: justify;">
The complexity of solving the all-pairs shortest path problem depends significantly on the chosen algorithm and the graph's density. A naive approach would involve running a single-source shortest path algorithm, such as Dijkstra's or Bellman-Ford, from each vertex in the graph. This method, while straightforward, is inefficient for large graphs due to its high computational cost. Specifically, running Dijkstra's algorithm from each vertex has a time complexity of $O(V^2 log V)$ with a priority queue, or $O(V^3)$ with a simple implementation, where $V$ is the number of vertices. Bellman-Ford, on the other hand, has a time complexity of $O(V^2 E)$ for each source, where E is the number of edges. More sophisticated algorithms, such as the Floyd-Warshall algorithm, can compute all-pairs shortest paths more efficiently in $O(V^3)$ time, regardless of the graph's density. This algorithm uses dynamic programming to iteratively update the shortest paths between all pairs of vertices based on intermediate vertices.
</p>

<p style="text-align: justify;">
The all-pairs shortest path problem is crucial in various real-world applications. In network routing, for instance, it enables efficient data packet routing by providing the shortest paths between all pairs of network nodes. In transportation planning, it helps in determining the most efficient routes between multiple locations, considering various constraints and costs. Moreover, in optimization problems, understanding the shortest paths between all node pairs can be critical for solving problems like the traveling salesman problem or optimizing logistics and distribution networks.
</p>

<p style="text-align: justify;">
In summary, the all-pairs shortest path problem is a key concept in graph theory with significant implications for practical applications. Its resolution involves careful consideration of graph representation, algorithm complexity, and specific use cases, highlighting its importance in efficiently managing and analyzing networked systems.
</p>

# 23.2. Floyd-Warshall Algorithm
<p style="text-align: justify;">
The Floyd-Warshall algorithm is a well-known dynamic programming approach used to compute the shortest paths between all pairs of vertices in a weighted graph. This algorithm is particularly useful in scenarios where you need to know the shortest paths between every pair of nodes and is suitable for graphs with relatively small vertex sets and dense connectivity.
</p>

<p style="text-align: justify;">
The core of the Floyd-Warshall algorithm lies in its use of a 2D array, <code>dist[][]</code>, where <code>dist[i][j]</code> represents the shortest path from vertex <code>i</code> to vertex <code>j</code>. The algorithm iteratively improves the estimated shortest paths by considering each vertex as an intermediate point and updating the distance matrix accordingly. This process continues until no further improvements can be made.
</p>

- <p style="text-align: justify;">Initialization: Begin by initializing the <code>dist[][]</code> matrix. For each pair of vertices <code>(i, j)</code>, set <code>dist[i][j]</code> to the weight of the edge from <code>i</code> to <code>j</code> if there is a direct edge between them. If there is no direct edge, set <code>dist[i][j]</code> to infinity. The distance from a vertex to itself is set to zero, i.e., <code>dist[i][i] = 0</code>.</p>
- <p style="text-align: justify;">Iterative Update: The algorithm then iterates over each vertex <code>k</code> and updates the <code>dist[][]</code> matrix by considering whether a path from vertex <code>i</code> to vertex <code>j</code> through <code>k</code> is shorter than the current known shortest path. Specifically, for each pair of vertices <code>(i, j)</code>, update <code>dist[i][j]</code> as follows:</p>
{{< prism lang="text">}}
  if dist[i][j] > dist[i][k] + dist[k][j]:
      dist[i][j] = dist[i][k] + dist[k][j]
{{< /prism >}}
- <p style="text-align: justify;">This update checks if the distance from <code>i</code> to <code>j</code> through <code>k</code> is less than the currently known distance and updates it if so.</p>
- <p style="text-align: justify;">Complexity Analysis: The time complexity of the Floyd-Warshall algorithm is $O(V^3)$, where $V$ is the number of vertices in the graph. This is due to the three nested loops that iterate over all vertices. The space complexity is $O(V^2)$ because of the storage required for the distance matrix.</p>
- <p style="text-align: justify;">Practical Considerations: Floyd-Warshall is efficient for dense graphs and small to moderate-sized vertex sets due to its $O(V^3)$ time complexity. It can handle graphs with negative weight edges, but it requires an additional step to detect negative weight cycles. If after completing the algorithm, <code>dist[i][i]</code> (for any vertex <code>i</code>) is negative, it indicates the presence of a negative weight cycle.</p>
<p style="text-align: justify;">
Here is a high-level pseudo code for the Floyd-Warshall algorithm:
</p>

{{< prism lang="text" line-numbers="true">}}
function FloydWarshall(Graph):
    let dist be a 2D array with dimensions V x V
    for each vertex i in Graph:
        for each vertex j in Graph:
            if i == j:
                dist[i][j] = 0
            else if there is an edge from i to j:
                dist[i][j] = weight of edge from i to j
            else:
                dist[i][j] = ∞

    for each vertex k in Graph:
        for each vertex i in Graph:
            for each vertex j in Graph:
                if dist[i][j] > dist[i][k] + dist[k][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    for each vertex i in Graph:
        if dist[i][i] < 0:
            print "Negative weight cycle detected"
{{< /prism >}}
<p style="text-align: justify;">
Here’s how you can implement the Floyd-Warshall algorithm in Rust:
</p>

{{< prism lang="rust" line-numbers="true">}}
fn floyd_warshall(weights: &Vec<Vec<f64>>) -> Vec<Vec<f64>> {
    let v = weights.len();
    let mut dist = weights.clone();

    // Initialize the distance matrix
    for i in 0..v {
        for j in 0..v {
            if i == j {
                dist[i][j] = 0.0;
            } else if dist[i][j] == 0.0 {
                dist[i][j] = f64::INFINITY;
            }
        }
    }

    // Floyd-Warshall algorithm
    for k in 0..v {
        for i in 0..v {
            for j in 0..v {
                if dist[i][j] > dist[i][k] + dist[k][j] {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }

    // Check for negative weight cycles
    for i in 0..v {
        if dist[i][i] < 0.0 {
            println!("Negative weight cycle detected");
            return dist;
        }
    }

    dist
}

fn main() {
    let weights = vec![
        vec![0.0, 3.0, f64::INFINITY, f64::INFINITY],
        vec![2.0, 0.0, f64::INFINITY, 1.0],
        vec![f64::INFINITY, 7.0, 0.0, 2.0],
        vec![6.0, f64::INFINITY, f64::INFINITY, 0.0],
    ];

    let shortest_paths = floyd_warshall(&weights);

    for row in shortest_paths {
        for val in row {
            if val == f64::INFINITY {
                print!("∞ ");
            } else {
                print!("{:.1} ", val);
            }
        }
        println!();
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In this Rust implementation, the <code>floyd_warshall</code> function initializes a distance matrix based on the given edge weights. It then iteratively updates the distances using the Floyd-Warshall algorithm, considering each vertex as an intermediate point. After completing the distance updates, the function checks for negative weight cycles by inspecting the diagonal elements of the matrix. If any such cycle is detected, it prints a message and returns the distance matrix. Finally, the <code>main</code> function demonstrates how to use the <code>floyd_warshall</code> function with a sample graph and prints the resulting shortest path matrix.
</p>

<p style="text-align: justify;">
This approach is practical for dense graphs or graphs where all-pairs shortest path information is necessary and manageable within the computational constraints of the algorithm.
</p>

# 23.3. Johnson’s Algorithm
<p style="text-align: justify;">
Johnson’s Algorithm is a sophisticated method designed for finding the shortest paths between all pairs of vertices in a graph, accommodating graphs with potentially negative edge weights, provided there are no negative weight cycles. This algorithm combines techniques from reweighting edges and applying Dijkstra’s algorithm, offering a practical alternative to Floyd-Warshall, especially for sparse graphs.
</p>

<p style="text-align: justify;">
Johnson’s Algorithm addresses the challenge of graphs with negative edge weights by leveraging a dual approach: reweighting the graph to ensure non-negative edge weights and then applying Dijkstra’s algorithm to find shortest paths efficiently. This method is particularly effective for sparse graphs, where Floyd-Warshall’s $O(V^3)$ time complexity might be prohibitive.
</p>

- <p style="text-align: justify;">Add a New Vertex: Introduce a new vertex sss to the graph. Connect this vertex to every other vertex in the original graph with edges of zero weight. This step ensures that all vertices can be reached from the new vertex, which is crucial for the subsequent Bellman-Ford algorithm.</p>
- <p style="text-align: justify;">Compute Shortest Paths Using Bellman-Ford: Apply the Bellman-Ford algorithm starting from the new vertex $s$. This algorithm will compute the shortest path distances from $s$ to every other vertex. The Bellman-Ford algorithm is capable of handling negative edge weights and will also help detect negative weight cycles. If a negative weight cycle is detected, Johnson’s Algorithm should be terminated since it assumes the absence of such cycles.</p>
- <p style="text-align: justify;">Reweight the Original Edges: Using the distances computed by Bellman-Ford, adjust the weights of the original edges to ensure all edge weights are non-negative. Specifically, if $d[v]$ is the shortest path distance from $s$ to vertex $v$, the reweighted edge weight between two vertices $u$ and $v$ is computed as:</p>
{{< prism lang="text">}}
  w'(u, v) = w(u, v) + d[u] - d[v]
{{< /prism >}}
- <p style="text-align: justify;">where $w(u, v)$ is the original weight of the edge from uuu to $v$. This reweighting step ensures that all edge weights in the transformed graph are non-negative.</p>
- <p style="text-align: justify;">Apply Dijkstra’s Algorithm: For each vertex in the reweighted graph, use Dijkstra’s algorithm to compute the shortest paths to all other vertices. Since the edge weights are now non-negative, Dijkstra’s algorithm can be used efficiently with a time complexity of $O(V \log V + E)$ for each vertex. Combining this with the number of vertices, the overall complexity for Johnson’s Algorithm is $O(V^2 \log V + VE)$.</p>
<p style="text-align: justify;">
Johnson’s Algorithm exhibits a time complexity of $O(V^2 \log V + VE)$, where $V$ represents the number of vertices and $E$ represents the number of edges. This complexity arises from applying Bellman-Ford, which runs in $O(VE)$, and Dijkstra’s algorithm, which runs in $O(V \log V + E)$ for each vertex, yielding a combined time complexity of $O(V^2 \log V + VE)$. The space complexity is $O(V^2)$ due to the storage of the distance matrix for the reweighting process.
</p>

<p style="text-align: justify;">
Johnson’s Algorithm is particularly well-suited for sparse graphs where Floyd-Warshall’s $O(V^3)$ time complexity would be impractical. Its capability to handle negative weights efficiently makes it a valuable tool in scenarios where edge weights can vary in sign, provided there are no negative weight cycles. By reweighting the graph and leveraging Dijkstra’s algorithm, Johnson’s Algorithm balances efficiency and capability in dealing with diverse edge weights.
</p>

<p style="text-align: justify;">
Here is the pseudo code for Johnson’s Algorithm:
</p>

{{< prism lang="text" line-numbers="true">}}
function Johnson(Graph):
    let V be the number of vertices in Graph
    let NewVertex be a new vertex
    add NewVertex to Graph
    for each vertex v in Graph:
        add edge (NewVertex, v) with weight 0

    let h be a distance array of size V initialized to infinity
    h[NewVertex] = 0

    if BellmanFord(Graph, NewVertex, h) == False:
        print "Negative weight cycle detected"
        return

    for each edge (u, v) in Graph:
        weight[u][v] = weight[u][v] + h[u] - h[v]

    let shortest_paths be an empty list
    for each vertex u in Graph:
        let dist be the result of Dijkstra(Graph, u)
        shortest_paths[u] = dist

    return shortest_paths
{{< /prism >}}
<p style="text-align: justify;">
Here is a Rust implementation of Johnson’s Algorithm:
</p>

{{< prism lang="rust" line-numbers="true">}}
use std::cmp::Ordering;
use std::collections::BinaryHeap;

// A wrapper around f64 that allows it to be used in a BinaryHeap
#[derive(Copy, Clone, PartialEq, PartialOrd)]
struct FloatOrd(f64);

impl Eq for FloatOrd {}

impl Ord for FloatOrd {
    fn cmp(&self, other: &Self) -> Ordering {
        // Flip the order for max-heap emulation (smaller values come first)
        other.0.partial_cmp(&self.0).unwrap_or(Ordering::Equal)
    }
}

fn bellman_ford(graph: &Vec<Vec<f64>>, source: usize, dist: &mut Vec<f64>) -> bool {
    let v = graph.len();
    dist[source] = 0.0;

    for _ in 0..v-1 {
        for u in 0..v {
            for v in 0..graph[u].len() {
                if dist[u] < f64::INFINITY && dist[u] + graph[u][v] < dist[v] {
                    dist[v] = dist[u] + graph[u][v];
                }
            }
        }
    }

    for u in 0..v {
        for v in 0..graph[u].len() {
            if dist[u] < f64::INFINITY && dist[u] + graph[u][v] < dist[v] {
                return false; // Negative weight cycle detected
            }
        }
    }
    true
}

fn dijkstra(graph: &Vec<Vec<f64>>, source: usize) -> Vec<f64> {
    let v = graph.len();
    let mut dist = vec![f64::INFINITY; v];
    let mut visited = vec![false; v];
    let mut pq = BinaryHeap::new();

    dist[source] = 0.0;
    pq.push((FloatOrd(0.0), source));

    while let Some((FloatOrd(current_dist), u)) = pq.pop() {
        if visited[u] {
            continue;
        }
        visited[u] = true;

        for v in 0..v {
            if graph[u][v] < f64::INFINITY {
                let next_dist = current_dist + graph[u][v];
                if next_dist < dist[v] {
                    dist[v] = next_dist;
                    pq.push((FloatOrd(next_dist), v));
                }
            }
        }
    }
    dist
}

fn johnson(graph: &Vec<Vec<f64>>) -> Vec<Vec<f64>> {
    let v = graph.len();
    let mut extended_graph = graph.clone();

    // Add new vertex and edges with weight 0
    extended_graph.push(vec![0.0; v + 1]);
    for i in 0..v {
        extended_graph[i].push(0.0);
    }

    let mut h = vec![f64::INFINITY; v + 1];
    if !bellman_ford(&extended_graph, v, &mut h) {
        println!("Negative weight cycle detected");
        return vec![];
    }

    // Reweight edges
    let mut reweighted_graph = vec![vec![f64::INFINITY; v]; v];
    for u in 0..v {
        for v in 0..v {
            if graph[u][v] < f64::INFINITY {
                reweighted_graph[u][v] = graph[u][v] + h[u] - h[v];
            }
        }
    }

    // Apply Dijkstra's algorithm for each vertex
    let mut all_pairs_shortest_paths = Vec::with_capacity(v);
    for u in 0..v {
        let dist = dijkstra(&reweighted_graph, u);
        all_pairs_shortest_paths.push(dist);
    }

    all_pairs_shortest_paths
}

fn main() {
    let graph = vec![
        vec![0.0, 3.0, f64::INFINITY, f64::INFINITY],
        vec![2.0, 0.0, f64::INFINITY, 1.0],
        vec![f64::INFINITY, 7.0, 0.0, 2.0],
        vec![6.0, f64::INFINITY, f64::INFINITY, 0.0],
    ];

    let shortest_paths = johnson(&graph);

    for row in shortest_paths {
        for val in row {
            if val == f64::INFINITY {
                print!("∞ ");
            } else {
                print!("{:.1} ", val);
            }
        }
        println!();
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In the Rust implementation, the <code>bellman_ford</code> function computes shortest paths from a new vertex sss to all other vertices, detecting negative weight cycles. The <code>dijkstra</code> function performs the shortest path calculation for a vertex in the reweighted graph. The <code>johnson</code> function ties everything together: it extends the original graph, applies Bellman-Ford to compute shortest paths for reweighting, adjusts edge weights, and then applies Dijkstra’s algorithm to find shortest paths for each vertex. The <code>main</code> function demonstrates how to use Johnson’s Algorithm with a sample graph and prints the resulting shortest path matrix.
</p>

<p style="text-align: justify;">
This implementation efficiently handles sparse graphs and negative edge weights, making Johnson’s Algorithm a valuable tool for various applications requiring all-pairs shortest path computations.
</p>

# 23.4. Practical Use Cases and Optimizations
<p style="text-align: justify;">
In practical applications, algorithms for finding shortest paths between all pairs of vertices are crucial for various domains, including network optimization, geographic information systems (GIS), and operations research. The Floyd-Warshall and Johnson’s algorithms provide foundational solutions, but their efficiency can be significantly improved through optimization techniques tailored to specific use cases and the nature of the data.
</p>

- <p style="text-align: justify;">Network Optimization: In network optimization, such as routing and network flow analysis, finding the shortest paths between all pairs of nodes can help in optimizing data traffic and ensuring efficient utilization of network resources. For instance, network routing algorithms can leverage shortest path calculations to determine the most efficient routes for data packets, reducing latency and maximizing throughput. Network flow analysis also benefits from all-pairs shortest path algorithms by improving the allocation of resources and managing network congestion.</p>
- <p style="text-align: justify;">Geographic Information Systems (GIS): In GIS, shortest path algorithms are used for pathfinding and route planning. Applications such as GPS navigation and urban traffic management rely on efficiently computing the shortest paths between various locations. By integrating these algorithms, GIS systems can provide optimal routing solutions for transportation, logistics, and urban planning, enhancing overall operational efficiency and user experience.</p>
- <p style="text-align: justify;">Operations Research: Operations research employs all-pairs shortest path algorithms for scheduling and resource allocation. For example, in job scheduling, algorithms can determine the most efficient sequence of tasks to minimize completion time. Similarly, in resource allocation, these algorithms help in optimizing the use of resources across multiple tasks or projects, ensuring that resources are allocated effectively and efficiently.</p>
## 23.4.1. Optimizations
<p style="text-align: justify;">
Floyd-Warshall: The Floyd-Warshall algorithm, though straightforward, can be optimized to handle large graphs more efficiently. One approach is to use bitwise operations to speed up matrix updates. For example, in some implementations, bitwise operations can be used to parallelize the update of matrix entries, thus reducing the time complexity. Additionally, exploiting sparsity in the graph can lead to fewer updates by only processing non-zero entries in the adjacency matrix. This sparse matrix approach reduces unnecessary computations, making the algorithm more efficient in practice.
</p>

<p style="text-align: justify;">
Johnson’s Algorithm: Johnson’s Algorithm benefits from optimizations in both its reweighting and shortest path computation phases. Using priority queues in Dijkstra’s algorithm enhances performance by efficiently managing the vertex selection process based on the minimum distance. In Rust, priority queues can be implemented using data structures like binary heaps or Fibonacci heaps, which offer improved performance compared to simpler queue implementations. Additionally, implementing efficient data structures for edge reweighting and distance lookups is crucial. For instance, hash maps or balanced trees can be used to manage reweighted edges and retrieve shortest path estimates quickly, thus speeding up the overall computation.
</p>

<p style="text-align: justify;">
For extremely large graphs, several strategies can be employed to manage and process the data effectively. Graph partitioning involves dividing the graph into smaller, more manageable subgraphs that can be processed independently, often in parallel. This approach reduces the computational burden and makes it feasible to handle large datasets. Parallel processing leverages multi-core processors or distributed systems to simultaneously process different parts of the graph, significantly improving performance.
</p>

<p style="text-align: justify;">
When exact solutions are computationally infeasible, approximation algorithms or heuristics can be used. These methods provide near-optimal solutions with reduced computational effort. For instance, heuristic-based algorithms like A\* can be used for pathfinding in large graphs, offering good approximations while avoiding the computational complexity of exact algorithms.
</p>

## 23.4.2. Real-World Considerations
<p style="text-align: justify;">
In real-world applications, algorithms often need to be adapted to handle dynamic updates and large-scale graphs. Efficient implementation requires careful selection of data structures and computational resources. Dynamic updates, such as changes in edge weights or graph structure, necessitate algorithms that can efficiently accommodate these changes without complete recomputation. For large-scale graphs, optimizing memory usage and computation through advanced data structures and parallel processing techniques is essential to achieve practical performance.
</p>

<p style="text-align: justify;">
Floyd-Warshall Optimization Using Bitwise Operations and Sparse Matrix:
</p>

{{< prism lang="text" line-numbers="true">}}
function FloydWarshall(graph):
    let V be the number of vertices
    let dist be a 2D array of size V x V initialized to infinity
    for i from 0 to V-1:
        for j from 0 to V-1:
            if i == j:
                dist[i][j] = 0
            else if graph[i][j] != infinity:
                dist[i][j] = graph[i][j]

    for k from 0 to V-1:
        for i from 0 to V-1:
            for j from 0 to V-1:
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    return dist
{{< /prism >}}
<p style="text-align: justify;">
Rust Implementation of Floyd-Warshall with Optimizations:
</p>

{{< prism lang="rust" line-numbers="true">}}
fn floyd_warshall(graph: &Vec<Vec<f64>>) -> Vec<Vec<f64>> {
    let v = graph.len();
    let mut dist = vec![vec![f64::INFINITY; v]; v];
    
    for i in 0..v {
        for j in 0..v {
            if i == j {
                dist[i][j] = 0.0;
            } else if graph[i][j] < f64::INFINITY {
                dist[i][j] = graph[i][j];
            }
        }
    }

    for k in 0..v {
        for i in 0..v {
            for j in 0..v {
                if dist[i][k] < f64::INFINITY && dist[k][j] < f64::INFINITY {
                    dist[i][j] = dist[i][j].min(dist[i][k] + dist[k][j]);
                }
            }
        }
    }

    dist
}

fn main() {
    let graph = vec![
        vec![0.0, 3.0, f64::INFINITY, f64::INFINITY],
        vec![2.0, 0.0, f64::INFINITY, 1.0],
        vec![f64::INFINITY, 7.0, 0.0, 2.0],
        vec![6.0, f64::INFINITY, f64::INFINITY, 0.0],
    ];

    let shortest_paths = floyd_warshall(&graph);

    for row in shortest_paths {
        for val in row {
            if val == f64::INFINITY {
                print!("∞ ");
            } else {
                print!("{:.1} ", val);
            }
        }
        println!();
    }
}
{{< /prism >}}
<p style="text-align: justify;">
Johnson’s Algorithm with Priority Queues and Efficient Data Structures:
</p>

{{< prism lang="text" line-numbers="true">}}
function Johnson(graph):
    let V be the number of vertices
    let extended_graph be a new graph with V+1 vertices
    add edges from the new vertex to all other vertices with weight 0

    let h be a distance array of size V+1 initialized to infinity
    h[NewVertex] = 0

    if BellmanFord(extended_graph, NewVertex, h) == False:
        print "Negative weight cycle detected"
        return

    let reweighted_graph be a new graph
    for each edge (u, v) in graph:
        reweighted_graph[u][v] = graph[u][v] + h[u] - h[v]

    let all_pairs_shortest_paths be an empty list
    for each vertex u in graph:
        let dist be the result of Dijkstra(reweighted_graph, u)
        all_pairs_shortest_paths[u] = dist

    return all_pairs_shortest_paths
{{< /prism >}}
<p style="text-align: justify;">
Rust Implementation of Johnson’s Algorithm:
</p>

{{< prism lang="rust" line-numbers="true">}}
use std::collections::{BinaryHeap, HashMap};
use std::cmp::Ordering;

#[derive(Copy, Clone, PartialEq, PartialOrd)]
struct FloatOrd(f64);

impl Eq for FloatOrd {}

impl Ord for FloatOrd {
    fn cmp(&self, other: &Self) -> Ordering {
        other.0.partial_cmp(&self.0).unwrap_or(Ordering::Equal)
    }
}

fn bellman_ford(graph: &Vec<Vec<f64>>, source: usize, dist: &mut Vec<f64>) -> bool {
    let v = graph.len();
    dist[source] = 0.0;

    for _ in 0..v - 1 {
        for u in 0..v {
            for v in 0..graph[u].len() {
                if dist[u] < f64::INFINITY && dist[u] + graph[u][v] < dist[v] {
                    dist[v] = dist[u] + graph[u][v];
                }
            }
        }
    }

    for u in 0..v {
        for v in 0..graph[u].len() {
            if dist[u] < f64::INFINITY && dist[u] + graph[u][v] < dist[v] {
                return false; // Negative weight cycle detected
            }
        }
    }
    true
}

fn dijkstra(graph: &Vec<Vec<f64>>, source: usize) -> Vec<f64> {
    let v = graph.len();
    let mut dist = vec![f64::INFINITY; v];
    let mut visited = vec![false; v];
    let mut pq = BinaryHeap::new();

    dist[source] = 0.0;
    pq.push((FloatOrd(0.0), source));

    while let Some((FloatOrd(current_dist), u)) = pq.pop() {
        if visited[u] {
            continue;
        }
        visited[u] = true;

        for v in 0..v {
            if graph[u][v] < f64::INFINITY {
                let next_dist = current_dist + graph[u][v];
                if next_dist < dist[v] {
                    dist[v] = next_dist;
                    pq.push((FloatOrd(next_dist), v));
                }
            }
        }
    }
    dist
}

fn johnson(graph: &Vec<Vec<f64>>) -> Vec<Vec<f64>> {
    let v = graph.len();
    let mut extended_graph = vec![vec![f64::INFINITY; v + 1]; v + 1];
    
    for u in 0..v {
        for v in 0..v {
            extended_graph[u][v] = graph[u][v];
        }
    }
    for u in 0..v {
        extended_graph[v][u] = 0.0;
    }

    let mut h = vec![f64::INFINITY; v + 1];
    if !bellman_ford(&extended_graph, v, &mut h) {
        println!("Negative weight cycle detected");
        return vec![];
    }

    let mut reweighted_graph = vec![vec![f64::INFINITY; v]; v];
    for u in 0..v {
        for v in 0..v {
            reweighted_graph[u][v] = graph[u][v] + h[u] - h[v];
        }
    }

    let mut all_pairs_shortest_paths = vec![vec![f64::INFINITY; v]; v];
    for u in 0..v {
        all_pairs_shortest_paths[u] = dijkstra(&reweighted_graph, u);
    }
    all_pairs_shortest_paths
}

fn main() {
    let graph = vec![
        vec![0.0, 1.0, f64::INFINITY, 4.0],
        vec![f64::INFINITY, 0.0, 2.0, f64::INFINITY],
        vec![f64::INFINITY, f64::INFINITY, 0.0, 3.0],
        vec![f64::INFINITY, f64::INFINITY, f64::INFINITY, 0.0],
    ];

    let shortest_paths = johnson(&graph);

    for row in shortest_paths {
        for val in row {
            if val == f64::INFINITY {
                print!("∞ ");
            } else {
                print!("{:.1} ", val);
            }
        }
        println!();
    }
}
{{< /prism >}}
<p style="text-align: justify;">
The Floyd-Warshall and Johnson’s algorithms are essential tools for solving all-pairs shortest path problems. By optimizing these algorithms through bitwise operations, sparse matrix handling, and efficient data structures like priority queues, one can significantly enhance performance. Handling large graphs requires innovative approaches such as graph partitioning and approximation algorithms. Real-world considerations necessitate adaptations for dynamic and large-scale data, emphasizing the importance of efficient implementation and thoughtful resource management.
</p>

# 23.5. Conclusion
<p style="text-align: justify;">
To master "All-Pairs Shortest Paths" using Rust, you should adopt a methodical approach that intertwines theoretical understanding with practical Rust programming techniques. It's essential to explore both the theoretical aspects and practical implementations of the Floyd-Warshall and Johnson’s algorithms.
</p>

## 23.5.1. Advices
<p style="text-align: justify;">
Begin by thoroughly understanding the fundamental problem of finding shortest paths between all pairs of vertices in a weighted graph. This conceptual clarity will serve as the foundation for implementing the algorithms effectively in Rust.
</p>

<p style="text-align: justify;">
Start with Floyd-Warshall, a dynamic programming approach that iteratively updates a distance matrix to capture shortest paths through intermediate vertices. Implementing Floyd-Warshall in Rust involves creating and managing a 2D array to represent distances between vertices. Rust’s strong type system and memory safety features ensure that your matrix operations are both correct and efficient. Leverage Rust’s iterator traits and functional programming paradigms to write clear, concise code for the matrix updates. The language's rigorous compile-time checks will help you avoid common pitfalls, such as index out-of-bounds errors, which are crucial in maintaining the integrity of the distance matrix.
</p>

<p style="text-align: justify;">
Transition to Johnson’s algorithm, which is particularly useful for graphs with negative weights. This algorithm combines Bellman-Ford for edge reweighting with Dijkstra’s algorithm for finding shortest paths. Implement Bellman-Ford first in Rust to handle potential negative weight cycles and adjust edge weights to ensure all weights are non-negative. Rust’s <code>Result</code> type and error handling mechanisms will aid in managing the edge cases associated with negative weights and cycle detection.
</p>

<p style="text-align: justify;">
For Dijkstra’s algorithm, use Rust’s <code>BinaryHeap</code> from the <code>std::collections</code> module to implement an efficient priority queue. Rust’s ownership and borrowing system will help you manage the priority queue’s state safely, avoiding data races and ensuring correct priority operations. Optimize your Dijkstra’s implementation by focusing on minimizing heap operations and considering alternative data structures if necessary.
</p>

<p style="text-align: justify;">
Incorporate Rust’s concurrency features to handle large-scale graphs effectively. Rust’s concurrency model, which emphasizes safety and performance, allows you to parallelize computations where applicable. Utilize Rust’s async programming capabilities to manage asynchronous tasks efficiently, especially when working with large datasets or real-time applications.
</p>

<p style="text-align: justify;">
Apply these algorithms to practical scenarios such as network routing, where you can simulate various network topologies and analyze performance. By integrating real-world applications with theoretical models, you will gain deeper insights into the algorithms' behavior and performance under different conditions. Engage with the Rust community to gather feedback and explore advanced optimization techniques, which will enhance your implementation and understanding.
</p>

<p style="text-align: justify;">
By combining theoretical knowledge with Rust’s powerful programming features, students can develop a profound grasp of all-pairs shortest paths algorithms. Rust’s emphasis on safety and efficiency will not only aid in implementing these algorithms effectively but also prepare you for tackling complex, real-world problems with confidence.
</p>

## 23.5.2. Further Learning with GenAI
<p style="text-align: justify;">
The prompts below are crafted to cover a wide range of topics, including the fundamental principles, detailed algorithmic steps, optimizations, and real-world applications. They are designed to extract the most in-depth and technical insights from GenAI, helping you build a comprehensive understanding of these algorithms and their implementation in Rust. Each prompt encourages exploration of core concepts, algorithmic efficiency, edge cases, and practical coding techniques.
</p>

- <p style="text-align: justify;">Provide a detailed explanation of the all-pairs shortest paths problem, including its significance in graph theory and its applications in real-world scenarios. Discuss the theoretical importance of solving shortest paths between all pairs of vertices, its impact on various fields such as network design, logistics, and route planning.</p>
- <p style="text-align: justify;">Explain the Floyd-Warshall algorithm in detail, including its initialization, iterative updates, and final result computation. How does the $O(V^3)$ time complexity arise, and what are its limitations? Offer a step-by-step breakdown of the Floyd-Warshall algorithm, detailing how it uses a distance matrix to compute shortest paths. Include an explanation of its cubic time complexity and scenarios where it might be inefficient.</p>
- <p style="text-align: justify;">Demonstrate how to implement the Floyd-Warshall algorithm in Rust. Provide a complete code example that initializes the distance matrix, performs updates, and outputs the final shortest paths. Present a Rust implementation of the Floyd-Warshall algorithm with a focus on initializing a distance matrix, performing the iterative updates, and handling matrix bounds safely.</p>
- <p style="text-align: justify;">Analyze the scenarios where Floyd-Warshall might not be suitable. Discuss specific types of graphs (e.g., very large or sparse) and the performance trade-offs involved. Examine the practical limitations of Floyd-Warshall, such as its performance on sparse graphs or very large datasets, and compare it to other algorithms.</p>
- <p style="text-align: justify;">Describe Johnson’s algorithm, focusing on its two main components: Bellman-Ford for edge reweighting and Dijkstra’s algorithm for shortest path calculation. How does Johnson’s algorithm handle graphs with negative edge weights? Provide a detailed explanation of Johnson’s algorithm, including the role of Bellman-Ford in adjusting edge weights and Dijkstra’s algorithm in computing shortest paths. Discuss how Johnson’s algorithm manages negative weights through reweighting.</p>
- <p style="text-align: justify;">Implement Johnson’s algorithm in Rust. Include sample code for both the Bellman-Ford algorithm and the Dijkstra’s algorithm components, demonstrating how they work together to find all-pairs shortest paths. Offer Rust code examples for Bellman-Ford and Dijkstra’s algorithms, showing how to integrate these components into Johnson’s algorithm for handling all-pairs shortest path calculations.</p>
- <p style="text-align: justify;">What are the advanced optimization techniques for the Floyd-Warshall algorithm? Discuss approaches such as matrix sparsity exploitation and algorithmic improvements to enhance performance. Explore optimization techniques for improving Floyd-Warshall’s efficiency, including methods for leveraging matrix sparsity and other performance enhancements.</p>
- <p style="text-align: justify;">Discuss advanced data structures and optimization strategies for Johnson’s algorithm. How can these techniques improve performance for large or dense graphs? Analyze the use of advanced data structures, such as priority queues and hash maps, and optimization strategies that can enhance the performance of Johnson’s algorithm, particularly for large or dense graphs.</p>
- <p style="text-align: justify;">Provide strategies for efficiently handling large-scale graphs in Rust when implementing the shortest path algorithms. Include considerations for memory management and parallel processing. Discuss methods for managing large graphs in Rust, focusing on efficient memory usage and parallel processing techniques to handle extensive data sets effectively.</p>
- <p style="text-align: justify;">How can Rust’s concurrency features be applied to optimize the performance of Floyd-Warshall and Johnson’s algorithms? Provide examples of using Rust’s threading and asynchronous features. Explain how Rust’s concurrency model can be utilized to parallelize parts of the shortest path algorithms, including examples of threading and asynchronous programming to enhance performance.</p>
- <p style="text-align: justify;">What are the best practices for testing and validating the correctness of Floyd-Warshall and Johnson’s implementations in Rust? Describe strategies for unit testing, edge case handling, and performance validation. Outline effective testing strategies for ensuring the correctness and efficiency of your Rust implementations, including unit tests, handling edge cases, and validating performance with various graph sizes.</p>
- <p style="text-align: justify;">Discuss real-world applications of all-pairs shortest paths algorithms. Provide examples of how these algorithms are used in network routing, GIS, and other domains, and demonstrate how to simulate these scenarios in Rust. Explore practical uses of shortest path algorithms in different fields, and show how to simulate these applications using Rust code to gain a deeper understanding of their real-world relevance.</p>
- <p style="text-align: justify;">How do you detect and handle negative weight cycles in graphs using Floyd-Warshall and Johnson’s algorithms? Provide methods for cycle detection and the implications of negative cycles on shortest path calculations. Detail the techniques for identifying negative weight cycles in graphs, how these cycles affect the results of Floyd-Warshall and Johnson’s algorithms, and strategies for managing these cycles.</p>
- <p style="text-align: justify;">Compare and contrast the trade-offs between using Floyd-Warshall and Johnson’s algorithm for different types of graphs. Discuss factors such as graph density, edge weights, and computational efficiency.Analyze the relative advantages and disadvantages of Floyd-Warshall and Johnson’s algorithms based on graph characteristics such as density and edge weights, and their implications for computational efficiency.</p>
<p style="text-align: justify;">
Exploring these prompts will provide you with a comprehensive understanding of all-pairs shortest paths algorithms, both from a theoretical perspective and through practical Rust implementations. By tackling each prompt, you will not only grasp the nuances of the Floyd-Warshall and Johnson’s algorithms but also gain hands-on experience with Rust’s powerful features for handling complex graph problems. Embrace the challenge of implementing and optimizing these algorithms, as it will significantly enhance your problem-solving skills and prepare you for tackling real-world scenarios. Dive into these prompts, experiment with Rust code, and let your curiosity drive you toward mastery of these fundamental algorithms.
</p>

## 23.5.3. Self-Exercises
<p style="text-align: justify;">
Here are five comprehensive exercises designed to deepen your understanding of "All-Pairs Shortest Paths". These exercises combine theoretical analysis, practical implementation, and performance optimization to provide a well-rounded learning experience.
</p>

<p style="text-align: justify;">
<strong></strong>Exercise 23.1:<strong></strong> Implement and Compare Algorithms
</p>

- <p style="text-align: justify;"><strong></strong>Task:<strong></strong> Implement both the Floyd-Warshall and Johnson’s algorithms in Rust. Ensure your implementation includes initialization, distance matrix updates for Floyd-Warshall, and edge reweighting and shortest path calculations for Johnson’s algorithm.</p>
- <p style="text-align: justify;"><strong></strong>Requirements:<strong></strong></p>
- <p style="text-align: justify;">Provide a detailed explanation of your code, including comments on how each part of the algorithm works.</p>
- <p style="text-align: justify;">Compare the performance of both algorithms on different types of graphs (e.g., dense, sparse, with negative weights).</p>
- <p style="text-align: justify;">Include a set of test cases to validate correctness, including graphs with negative weight cycles for Johnson’s algorithm.</p>
- <p style="text-align: justify;"><strong></strong>Deliverable:<strong></strong> Submit your Rust code along with a report detailing the performance comparisons and results from your test cases.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 23.2:<strong></strong> Optimization and Performance Analysis
</p>

- <p style="text-align: justify;"><strong></strong>Task:<strong></strong> Explore and implement optimization techniques for both algorithms. For Floyd-Warshall, investigate matrix sparsity optimizations. For Johnson’s algorithm, apply advanced data structures or techniques to improve efficiency.</p>
- <p style="text-align: justify;"><strong></strong>Requirements:<strong></strong></p>
- <p style="text-align: justify;">Implement at least one optimization technique for each algorithm.</p>
- <p style="text-align: justify;">Measure and report the performance improvements with respect to time and space complexity.</p>
- <p style="text-align: justify;">Provide a detailed explanation of how the optimizations impact the performance.</p>
- <p style="text-align: justify;"><strong></strong>Deliverable:<strong></strong> Submit the optimized Rust code along with a performance analysis report that includes benchmarks and observations.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 23.3:<strong></strong> Concurrency and Large Graphs
</p>

- <p style="text-align: justify;"><strong></strong>Task:<strong></strong> Implement concurrent versions of both the Floyd-Warshall and Johnson’s algorithms in Rust to handle large graphs. Utilize Rust’s concurrency features, such as threads or async functions, to parallelize parts of the algorithms.</p>
- <p style="text-align: justify;"><strong></strong>Requirements:<strong></strong></p>
- <p style="text-align: justify;">Provide a description of how you used Rust’s concurrency features to enhance performance.</p>
- <p style="text-align: justify;">Test your concurrent implementations on large-scale graphs and compare the execution time with non-concurrent versions.</p>
- <p style="text-align: justify;">Include code samples and a discussion on the challenges and benefits of using concurrency for these algorithms.</p>
- <p style="text-align: justify;"><strong></strong>Deliverable:<strong></strong> Submit the concurrent Rust code along with a report detailing the concurrency implementation and performance results.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 23.4:<strong></strong> Real-World Application Simulation
</p>

- <p style="text-align: justify;"><strong></strong>Task:<strong></strong> Create a simulation of a real-world application that uses the all-pairs shortest paths algorithms. For example, simulate network routing for a set of routers or a GIS application for road networks.</p>
- <p style="text-align: justify;"><strong></strong>Requirements:<strong></strong></p>
- <p style="text-align: justify;">Develop a scenario that highlights the practical use of the algorithms.</p>
- <p style="text-align: justify;">Implement the simulation using Rust and demonstrate how the algorithms are applied to solve the problem.</p>
- <p style="text-align: justify;">Provide a discussion on how the results from the algorithms influence the application.</p>
- <p style="text-align: justify;"><strong></strong>Deliverable:<strong></strong> Submit the Rust code for the simulation along with a report describing the application scenario, algorithm usage, and results.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 23.5:<strong></strong> Negative Weight Cycles Analysis
</p>

- <p style="text-align: justify;"><strong></strong>Task:<strong></strong> Design and implement a Rust program to detect and handle negative weight cycles in graphs using the Floyd-Warshall and Johnson’s algorithms. Develop methods for cycle detection and assess how each algorithm deals with these cycles.</p>
- <p style="text-align: justify;"><strong></strong>Requirements:<strong></strong></p>
- <p style="text-align: justify;">Implement cycle detection in both algorithms and handle negative weight cycles appropriately.</p>
- <p style="text-align: justify;">Test the implementation with graphs containing negative weight cycles and provide a detailed analysis of how each algorithm addresses these cycles.</p>
- <p style="text-align: justify;">Include explanations of the cycle detection methods and their impact on the algorithm’s results.</p>
- <p style="text-align: justify;"><strong></strong>Deliverable:<strong></strong> Submit the Rust code for detecting and handling negative weight cycles, along with a report detailing your methods, test cases, and analysis of the algorithms’ handling of these cycles.</p>
<p style="text-align: justify;">
These exercises are designed to challenge students and encourage deep exploration of all-pairs shortest paths algorithms, combining theoretical knowledge with practical Rust programming skills.
</p>
