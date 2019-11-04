# Hopcroft--Karp-Algorithm-in-Java
A vertex that is not the endpoint of an edge in some partial matching  M M is called a free vertex. The basic concept that the algorithm relies on is that of an augmenting path, a path that starts at a free vertex, ends at a free vertex, and alternates between unmatched and matched edges within the path. It follows from this definition that, except for the endpoints, all other vertices (if any) in augmenting path must be non-free vertices. An augmenting path could consist of only two vertices (both free) and single unmatched edge between them.  If  M M is a matching, and  P P is an augmenting path relative to  M M, then the symmetric difference of the two sets of edges,  M ⊕ P M \oplus P, would form a matching with size  | M | + 1 |M| + 1. Thus, by finding augmenting paths, an algorithm may increase the size of the matching.  Conversely, suppose that a matching  M M is not optimal, and let  P P be the symmetric difference  M ⊕ M ∗ M \oplus M^* where  M ∗ M^* is an optimal matching. Because  M M and  M ∗ M^* are both matchings, every vertex has degree at most 2 in  P P. So  P P must form a collection of disjoint cycles, of paths with an equal number of matched and unmatched edges in  M M, of augmenting paths for  M M, and of augmenting paths for  M ∗ M^*; but the latter is impossible because  M ∗ M^* is optimal. Now, the cycles and the paths with equal numbers of matched and unmatched vertices do not contribute to the difference in size between  M M and  M ∗ M^*, so this difference is equal to the number of augmenting paths for  M M in  P P. Thus, whenever there exists a matching  M ∗ M^* larger than the current matching  M M, there must also exist an augmenting path. If no augmenting path can be found, an algorithm may safely terminate, since in this case  M M must be optimal.  An augmenting path in a matching problem is closely related to the augmenting paths arising in maximum flow problems, paths along which one may increase the amount of flow between the terminals of the flow. It is possible to transform the bipartite matching problem into a maximum flow instance, such that the alternating paths of the matching problem become augmenting paths of the flow problem.[3] In fact, a generalization of the technique used in Hopcroft–Karp algorithm to arbitrary flow networks is known as Dinic's algorithm.
The algorithm may be expressed in the following pseudocode.

Input: Bipartite graph 
G
(
U
∪
V
,
E
)
G(U \cup V, E)
Output: Matching 
M
⊆
E
M \subseteq E
M
←
∅
M \leftarrow \empty
repeat
P
←
{
P
1
,
P
2
,
…
,
P
k
}
\mathcal P \leftarrow \{P_1, P_2, \dots, P_k\} maximal set of vertex-disjoint shortest augmenting paths
M
←
M
⊕
(
P
1
∪
P
2
∪
⋯
∪
P
k
)
M \leftarrow M \oplus (P_1 \cup P_2 \cup \dots \cup P_k)
until 
P
=
∅
\mathcal P = \empty
In more detail, let 
U
U and 
V
V be the two sets in the bipartition of 
G
G, and let the matching from 
U
U to 
V
V at any time be represented as the set 
M
M. The algorithm is run in phases. Each phase consists of the following steps.

A breadth-first search partitions the vertices of the graph into layers. The free vertices in 
U
U are used as the starting vertices of this search and form the first layer of the partitioning. At the first level of the search, there are only unmatched edges, since the free vertices in 
U
U are by definition not adjacent to any matched edges. At subsequent levels of the search, the traversed edges are required to alternate between matched and unmatched. That is, when searching for successors from a vertex in 
U
U, only unmatched edges may be traversed, while from a vertex in 
V
V only matched edges may be traversed. The search terminates at the first layer 
k
k where one or more free vertices in 
V
V are reached.
All free vertices in 
V
V at layer 
k
k are collected into a set 
F
F. That is, a vertex 
v
v is put into 
F
F if and only if it ends a shortest augmenting path.
The algorithm finds a maximal set of vertex disjoint augmenting paths of length 
k
k. (Maximal means that no more such paths can be added. This is different from finding the maximum number of such paths, which would be harder to do. Fortunately, it is sufficient here to find a maximal set of paths.) This set may be computed by depth first search (DFS) from 
F
F to the free vertices in 
U
U, using the breadth first layering to guide the search: the DFS is only allowed to follow edges that lead to an unused vertex in the previous layer, and paths in the DFS tree must alternate between matched and unmatched edges. Once an augmenting path is found that involves one of the vertices in 
F
F, the DFS is continued from the next starting vertex. Any vertex encountered during the DFS can immediately be marked as used, since if there is no path from it to 
U
U at the current point in the DFS, then that vertex can't be used to reach 
U
U at any other point in the DFS. This ensures 
O
(
|
E
|
)
O(|E|) running time for the DFS. It is also possible to work in the other direction, from free vertices in 
U
U to those in 
V
V, which is the variant used in the pseudocode.
Every one of the paths found in this way is used to enlarge 
M
M.
The algorithm terminates when no more augmenting paths are found in the breadth first search part of one of the phases.
