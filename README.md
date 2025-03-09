# DSA CheatSheet

---

## System Design

### Vertical Scaling
Updating the server like CPU, RAM, etc., but it eventually reaches a maximum point.

### Horizontal Scaling
Creating multiple servers and using a load balancer to distribute traffic. Example: After 100 users, switch to another service.

### Logging (External)
Tracing logs for every user. If there is an error, it should be fixed immediately.

### Metrics (External)
Observing how the system reacts to user actions:
- Can it handle requests without issues?
- Time series analysis.
- Identifying trends (positive/negative).
- Checking if CPU can handle requests or if there is a bottleneck.

### Alerts
When something bad happens, the alert system takes information from metrics and notifies developers via email.

### Design Requirements
1. Move Data
2. Store Data
3. Transform Data

### Availability
Should be **99.999% optimal**, meaning the system is down for only 5 minutes per year.
- **Service Level Objective (SLO)** < **Service Level Agreement (SLA)** (refund based on uptime/downtime ratio).

### Reliability
Probability that the system won’t fail.

### Fault Tolerance
If one server is down and another continues functioning, the system has fault tolerance.

### Redundancy
Keeping identical copies of servers to prevent failures.

### Throughput
Measured by requests per second:
- **For databases**: Queries per second (QPS).
- **For data**: Bytes per second.

### Latency
Total operation time, measuring how long each individual request takes. **Example: Content Delivery Network (CDN)**.

---

## Networking

### IP Header
Contains metadata such as source IP and destination IP.

### TCP/IP
- Uses sequence numbers.
- **Reliable** → Uses a 3-way handshake.

### Static and Dynamic IP Addresses
- Dynamic IPs change over time.
- Check **Dynamic DNS** for managing dynamic IPs.

### UDP/IP
- **Faster** but does not guarantee data integrity.
- Commonly used for streaming data, such as live videos.

## API

### RPC (Remote Procedure Call)
Example: **gRPC**

### HTTP Status Codes
- **Info**: 100-199
- **Success**: 200-299
- **Redirection**: 300-399
- **Client Error**: 400-499
- **Server Error**: 500-599

### WebSockets
Connects via HTTP and updates responses immediately.

### GraphQL
- Uses HTTP POST.
- Solves **overfetching** and **underfetching** (making too many requests).
- Single endpoint with **Query** and **Mutation**.

### gRPC
- Uses **HTTP/2**.
- Mostly used for **server-to-server** communications.
- Uses **Protocol Buffers**.

---

## API Design
- Do **not** add verbs to HTTP endpoints.
- Example: Instead of `/getVideos`, use `/videos` for REST APIs.

---

## Caching

### Write Strategies
- **Write-around cache**: Only updates the database.
- **Write-through cache**: Updates the cache first, then updates the database.
- **Write-back cache**: Fast but inconsistent. Updates only the cache and periodically updates the database.

### Eviction Policies
Deciding which part of the cache to remove when full:
- **Random**
- **FIFO** (First In, First Out)
- **LRU** (Least Recently Used)
- **LFU** (Least Frequently Used)

---

## CDN (Content Delivery Network)
- Used for **static content** (e.g., images, videos).
- **Push CDN vs. Pull CDN**:
    - **Pull CDN** is preferred since **Push CDN** may store unnecessary data.

---

## Proxies & Load Balancers

### Proxy Types
- **Forward Proxy**: Example: **VPNs** hide the client.
- **Reverse Proxy**: Example: **CDNs & Load Balancers** hide the actual server.

### Proxy Server
- If a client **cannot** access a server, a **proxy server** can access it on behalf of the client.
- A **corporate proxy** can **restrict** access (e.g., blocking Instagram at work).

### Load Balancer Algorithms
- **Round Robin**: Distributes requests sequentially across servers.
- **Weighted Round Robin**: Allocates requests based on a set percentage.
- **Least Connections**: Routes requests to the server with the fewest connections.
- **Location-based**: Directs users to the nearest server.
- **Hashing**: Uses a hash function to distribute requests.

### Layer 4 vs. Layer 7 Load Balancing
- **Layer 4**: Operates at the **network (TCP) layer**.
    - **Faster** but **less flexible** (cannot inspect application data).
- **Layer 7**: Operates at the **application layer**.
    - **More flexible** but slightly slower.

📖 **See more:** [Maglev Paper by Google](https://research.google/pubs/pub44824/)

## Consistent Hashing
- According to IP address, we hash the value and redirect users by their IP address so every time users will be redirected to the same server.
- Helpful when working with in-memory Redis, etc.
- Ensures consistent data distribution.
- If one server crashes, the request goes to the nearest server according to the hashing algorithm.
- **Clockwise strategy:** Nearest server in the clockwise direction.
- **Rendezvous hashing** is another approach to consistent hashing.
- Consistent hashing consists of **3 main parts**: `HashKey`, `HashFunction`, `Nodes`.

## SQL
- **ACID** stands for:
    - **Atomicity:** A transaction is treated as a single atomic unit.
    - **Consistency:** A transaction must preserve the consistency of the underlying data.
    - **Isolation:** A transaction is isolated from all other transactions.
    - **Durability:** A committed transaction remains committed even in case of failures.

## NoSQL
- **Types of NoSQL Databases:**
    - **Document Database:** MongoDB
    - **Wide-Column Database:** Suitable for write-heavy scenarios (e.g., Cassandra, BigTable)
    - **Graph Database:** Uses nodes and relationships
- **Scaling NoSQL:** Easier than SQL due to the lack of strict ACID constraints.
- **BASE:** NoSQL equivalent of ACID -> Eventual Consistency (Leader-Follower replication)

## Replication & Sharding
- **Leader-Follower (Master-Slave) Replication:** Leader handles writes, Followers handle reads.
- **Multi-Master Replication:** Multiple masters for better availability.
- **Sharding:** Distributes a database into smaller shards for scalability.
    - **Shard Key:** Determines how data is split across shards.

## CAP Theorem
- **Consistency (C):** Data consistency across databases.
- **Availability (A):** System remains operational.
- **Partition Tolerance (P):** System continues to function even if network failures occur.
- **Trade-off:** Always choose `P`, then decide between `C` or `A`.

## Object Storage
- **AWS S3**
- Data stored in a flat structure.
- **BLOB (Binary Large Object):** Images, videos, etc.
- Files are **read-only** (no in-place updates).

## Message Queues
- **Pub/Sub (Publisher-Subscriber)** model.
- Uses **topics** for message distribution.

## MapReduce
- **Batch vs Streaming Processing:**
    - **Batch:** Works on pre-existing data (e.g., Hadoop, Spark)
    - **Streaming:** Processes data in real-time (e.g., Flink)
- **MapReduce Workflow:**
    1. **Map:** Splits and processes input data.
    2. **Shuffle:** Redistributes data.
    3. **Reduce:** Aggregates and processes results.

## System Design Interview
- **Approach:**
    1. **Scoping:** Define focus areas.
    2. **Clarify Requirements:** Avoid assumptions.
    3. **Functional Requirements:** What the app should do.
    4. **Non-Functional Requirements:** Scalability, latency, availability.
- **Back-of-the-envelope Calculations:**
    - Example: `10B / 100K = 100K reads/sec` and `100 writes/sec`.

## Design a Rate Limiter
- Limits user actions (e.g., uploading max 20 videos/day on YouTube).
- **Fail-open:** System works if limiter fails.
- **Fail-closed:** System crashes if limiter fails.
- **Algorithms:**
    - **Fixed Window:** Allow `X` requests per minute.
    - **Sliding Window, Token Bucket, Sliding Window Counter.**
- **Optimization:** Cache rules since they change infrequently.

## Design TinyURL
- **NoSQL** is preferred (no joins needed).
- **Collision Prevention:**
    - Key-based unique URL generation.
    - Expired URLs should be recycled.
- **SQL for ACID guarantees.**

## Design Twitter
- **Functional Requirements:** Follow users, post tweets, view feed.
- **Non-Functional Requirements:**
    - `500M` users, `200M` daily active users.
    - `20B` reads/day, `20PB` daily storage.
- **Sharding Strategy:** Use User ID as a shard key.

## Design Discord
- **Traffic Estimation:**
    - `5M` daily users, `50M` messages per day.
    - `20K` users per channel, `10K` messages per channel.
- **High-Level Design:**
    - Prefer **WebSockets** over polling.
    - Initially used MongoDB, later switched to **Cassandra**.

## Design YouTube
- **Traffic Estimation:**
    - `1B` daily users, `5B` views/day, `50M` uploads/day.
- **Priorities:**
    - **Availability > Consistency** (instant recommendations, delayed updates).
- **High-Level Design:**
    - **Load Balancer** to handle uploads.
    - **Object Storage** for videos.
    - **Queue Processing** for scalability.
    - **Rate Limiter** to prevent abuse.
    - **CDNs** for global delivery.
    - **LRU Cache** for frequent content access.

## Design Google Drive
- **File System (HDFS) vs Object Storage:**
  - File system is well organized but scaling is harder.
  - Object store can easily scale but files cannot be edited like in a file system.
  - Object stores hold data in a flat data structure.
- **Block-level Storage:**
  - The file is broken up into blocks like 4MB chunks.
  - If duplicate blocks exist, only one instance is stored (deduplication).
  - Useful for cases like resuming file uploads.
- **Optimizations:**
  - Garbage collection service checks KV store for unreferenced IDs and deletes them from object storage.
- **Heartbeat Mechanism:**
  - Load Balancer status can be checked using heartbeat signals (e.g., via Zookeeper).

## Design Google Maps
- **GeoCoding**
- **Spatial Indexing:** Quadtrees, Geohashing.
- **Hexagon-Based Indexing:** Check Uber’s article about H3: [Uber Blog](https://www.uber.com/en-TR/blog/h3/)

## Design Key-Value Store
- **Requirements:**
  - Must be durable, handle isolation, and resolve conflicts.
  - Must be scalable (Replication and Sharding).
- **High-Level Design:**
  - **Indexing Data:** B-Trees, B+ Trees, or LSM Trees.
  - **Replication:** Helps with scaling and fault tolerance.
  - **Partitioning:** Horizontal partitioning distributes data across nodes.
  - **Node Failure Handling:** Recovery strategies must be in place.
  - **Concurrent Writes:** Ensuring isolation.
- **LSM Tree (Log-Structured Merge Tree, used in Cassandra):**
  - Writes are batched in memory (memtable).
  - Periodically written to SSTable (Sorted Strings Table).
- **Quorum Mechanism:**
  - Two types: Write quorum and Read quorum.
- **Partitioning & Node Failure Handling:**
  - **Vnodes (Virtual Nodes):** Data distributed using consistent hashing.
  - If a node fails, the nearest clockwise node takes over its data.
  - Node failures detected via Zookeeper or Gossip Protocol (nodes exchange heartbeat messages).
- **Concurrent Writes Handling:**
  - **LWW (Last Write Wins):** Used in Cassandra.
  - **Dynamo Vector Clocks:** Maintains a version like `[node, version]`.

## Design Distributed Message Queue
- **PUB/SUB Model:**
  - Publisher sends messages to subscribers via topics (Producer-Consumer pattern).
- **Functional Requirements:**
  - Fanout capability.
  - Messages retained until delivery.
  - At-least-once delivery guarantee.
- **Non-functional Requirements:**
  - Scalability.
  - Persistent storage.
  - High throughput.
- **High-Level Design:**
  - **Publish Forwarders & Subscriber Forwarders:** Manage message routing.
  - **Persistent Storage:** Messages stored in a database.
  - **Database Sharding:** Data is partitioned by topic.
  - **Metadata Database:** Stores topic and subscription information.
  - **Acknowledgment Mechanism:** Receiver sends ACK to inform sender.
  - **Replication:** Message databases are replicated to prevent data loss.

## Design Patterns

### [Software Design Patterns - GeeksforGeeks](https://www.geeksforgeeks.org/software-design-patterns/)

---

## Creational Patterns

### Factory Method
Factory Method abstracts object creation details. Instead of a simple factory with multiple `if` or `switch` statements (which violates the Open/Closed Principle), the Factory Method is typically implemented using an interface or an abstract class. It includes a separate creator class, such as `CheeseBurgerCreator` or `DeluxeBurgerCreator` in a burger application.

### Singleton
The Singleton pattern ensures that only one instance of a class exists, sharing the same state. A well-known example is Redux, which uses the Singleton pattern. However, Singleton can sometimes be considered an anti-pattern due to its rigidity.

### Builder
The Builder pattern constructs an object step by step, providing flexibility in object creation. The `.build()` method is the final step, assembling the configured object. This is useful when an object requires multiple optional components, such as a meal with a drink, dessert, and main course.

### Prototype
The Prototype pattern involves creating new object instances by cloning an existing one using a `.clone()` method. This approach ensures that each client receives a fresh instance. Fields should be deep-copied to avoid unintended shared references. In Spring Boot, developers can choose between singleton and prototype scopes.

---

## Structural Patterns

### Adapter
The Adapter pattern enables compatibility between classes that have different interfaces. It often involves wrapper classes that transform one interface into another.

### Decorator
The Decorator pattern is used to dynamically alter or extend an object's behavior without modifying its structure. It prevents classes from becoming monolithic (god classes) and promotes composition over inheritance.

### Facade
The Facade pattern provides a simplified interface to a complex system by grouping functionalities. For example, a `SmartHomeFacade` class could offer predefined modes like "Movie Mode" and "Focus Mode" instead of requiring users to set brightness, temperature, and other parameters individually.

---

## Behavioral Patterns

### Strategy
The Strategy pattern defines a family of algorithms that can be selected at runtime. This promotes:
- Encapsulation of different behaviors into separate strategy classes.
- Dynamic behavior switching without modifying client code.
- Improved modularity by delegating execution to strategy objects.

### Observer
The Observer pattern facilitates notification-based systems. Observers subscribe to changes in a subject and receive updates automatically. A real-world example is a pub-sub system or a YouTube notification bell, where subscribers receive updates when new content is published.

### State
State pattern is used for state machines, reducing complex conditional logic by encapsulating state-specific behaviors in separate classes. For example, in a traffic light system, separate classes (`GreenState`, `YellowState`, `RedState`) manage transitions based on the previous state, avoiding excessive `if-else` statements.

---

## OOP

### Abstract Class
A class that can contain abstract methods and cannot be instantiated directly. It can have both abstract and regular methods.

### Interface
A structure that contains only method signatures. It supports multiple inheritance.

### Polymorphism
Allows the same method to behave differently. It is divided into Overriding and Overloading.

- **Overloading**: Methods defined in the same class with the same name but different parameters.

# Data Structures

## LinkedList

### Singly LinkedList

#### Insertion
- **At the beginning**
  ```plaintext
  node.next = head;
  head = node;
  ```
- **At the end**
  ```plaintext
  node.next = null;
  last.next = node;
  tail = node;
  ```
- **Somewhere in between**
  ```plaintext
  Find location with loop
  current.next = node;
  node.next = nextNode;
  ```

#### Time and Space Complexity

| Operation    | Time Complexity | Space Complexity |
|-------------|---------------|----------------|
| Creation    | O(1)          | O(1)          |
| Insertion   | O(n)          | O(1)          |
| Searching   | O(n)          | O(1)          |
| Traversing  | O(n)          | O(1)          |
| Delete 1    | O(n)          | O(1)          |
| Delete all  | O(1)          | O(1)          |

### Other Linked Lists
- **Circular Singly LinkedList**
- **Doubly LinkedList**
- **Circular Doubly LinkedList**

---

## Stack (LIFO)
Stack follows the **Last In First Out (LIFO)** principle.

### Implementation
- Can be implemented using **Array** or **LinkedList**.

### Methods
- `createStack`
- `push`
- `pop`
- `peek`
- `isEmpty`
- `isFull`
- `deleteStack`

### Usage
- LIFO functionality
- Reduces the chance of data corruption

### Limitations
- **Random access is not possible**

---

## Queue (FIFO)
Queue follows the **First In First Out (FIFO)** principle.

### Implementation
- Can be implemented using **Array** (Linear Queue, Circular Queue) or **LinkedList**.

### Methods
- `createQueue`
- `enqueue`
- `dequeue`
- `peek`
- `isEmpty`
- `isFull`
- `deleteQueue`

### Usage
- FIFO functionality
- Reduces the chance of data corruption

### Limitations
- **Random access is not possible**

---

## Recursion
### Definition
A way of solving a problem by having a function call itself.

### When to Use
- When a problem can be **broken down into similar subproblems**
- **Quick solutions** instead of optimized ones
- **Tree traversal**
- **Dynamic programming (memoization)**

### When to Avoid
- When **time and space complexity matters**
- Uses more memory
- Can be slow

### Recursion Steps
1. **Recursive case** - Define the flow
2. **Base case** - Define the stopping condition
3. **Unintentional case** - Define constraints

---

## Tree / Binary Tree
### Why Use Trees?
- Quicker and easier access to data
- Suitable for **hierarchical data** (e.g., folder structures, org charts, XML/HTML data)
- Unlike linear structures, no need to traverse all data

### Example
- **File system in computers**

### Common Types
- **Binary Search Tree (BST)** - Fast lookups
- **AVL Tree** - Self-balancing BST
- **Red Black Tree** - Balanced tree
- **Trie** - Used for search optimization

### Tree Terminology
- **Root**: Top node without a parent
- **Edge**: A link between a parent and child
- **Leaf**: A node without children
- **Sibling**: Nodes with the same parent
- **Ancestor**: Parent, grandparent, etc.
- **Depth of a node**: Length of path from root to node
- **Height of a node**: Length of path from node to deepest node
- **Depth of tree**: Depth of root node
- **Height of tree**: Height of root node

### Types of Binary Trees
- **Full Binary Tree**: Nodes have either 0 or 2 children (not 1)
- **Perfect Binary Tree**: Every node has exactly 2 children
- **Complete Binary Tree**: All levels are full except possibly the last
- **Balanced Binary Tree**: Left and right subtrees are nearly equal in height

### Binary Tree Traversal
#### Depth First Search (DFS)
- **Preorder**: Root → Left → Right
- **Inorder**: Left → Root → Right
- **Postorder**: Left → Right → Root

#### Breadth First Search (BFS)
- **Level Order**: Process nodes level by level

### Binary Tree using Arrays
- First index empty
- Left child = `2x`
- Right child = `2x+1`
- Parent = `i/2`

---

## Binary Search Tree (BST)
### Definition
- **Left subtree** contains values **≤ root**
- **Right subtree** contains values **> root**

### Why Use BST?
- **Faster search, insertion, and deletion** than an unstructured binary tree

---

## AVL Tree
### Definition
- A **self-balancing BST** where the height difference between left and right subtrees is **at most 1**.

### Insertion Cases
1. **Rotation Not Required**
2. **Rotation Required**
    - **LL Condition** → Right Rotation
    - **LR Condition** → Left Rotation + Right Rotation
    - **RR Condition** → Left Rotation
    - **RL Condition** → Right Rotation + Left Rotation

---

## Binary Heap
### Definition
- **Heap (Priority Queue)** → Min-Heap or Max-Heap
- **Binary Heap** is a complete tree with heap properties:
    - **Min-Heap**: Root is the **smallest** element
    - **Max-Heap**: Root is the **largest** element

### Properties
- **Complete Tree**: All levels are completely filled, except possibly the last level (filled left to right)

---

## Trie
### Definition
- A **tree-based** data structure that organizes information hierarchically

### Properties
- Efficient for **string storage and searching**
- Each node stores **links to next characters**
- Tracks **end of string**

### Why Use Trie?
- Solves many problems efficiently:
    - **Spelling checkers**
    - **Auto-completion**

# Hashing

Hashing is a method of sorting and indexing data. The idea behind hashing is to allow large amounts of data to be indexed using keys commonly created by formulas.

## Why

It is time efficient in case of SEARCH operations.

| Data Structure | Time Complexity for Search |
|---------------|-------------------------|
| Array        | O(logN) |
| Linked List  | O(N) |
| Tree         | O(logN) |
| Hashing      | O(1) / O(N) |

## Hashing Terminology

- **Hash function**: A function that maps data of arbitrary size to a fixed size.
- **Key**: Input data provided by a user.
- **Hash value**: A value returned by a hash function.
- **Hash table (Dictionary or HashMap)**: A data structure implementing an associative array that maps keys to values.
- **Collision**: Occurs when two different keys produce the same hash output.

## Collision Resolution Techniques

- **Direct Chaining**: Implements buckets as linked lists. If a collision occurs, the new element is added to the linked list.
- **Open Addressing**: Uses a larger table (usually double the current size) and rehashes elements.
    - **Linear Probing**: Places a new key into the next available slot.
    - **Quadratic Probing**: Uses a quadratic formula to find an open slot.
    - **Double Hashing**: Uses a second hash function to determine probe intervals.

---

# Sorting Algorithms

Sorting refers to arranging data in a particular order (ascending or descending).

## Sorting Algorithms

- **Bubble Sort**: Repeatedly compares adjacent elements and swaps them if necessary.
- **Selection Sort**: Finds the minimum element and moves it to the sorted part of the array.
- **Insertion Sort**:
    - Divides the array into sorted and unsorted parts.
    - Moves elements from the unsorted part to the correct position in the sorted part.
- **Bucket Sort**:
    - Distributes elements into buckets.
    - Sorts each bucket individually.
    - Merges sorted buckets.
- **Merge Sort**:
    - A divide and conquer algorithm.
    - Recursively divides the array and merges sorted halves.
- **Quick Sort**:
    - A divide and conquer algorithm.
    - Selects a pivot, places smaller elements on the left and larger elements on the right.
- **Heap Sort**:
    - Uses a binary heap to extract the smallest/largest element.

---

# Searching Algorithms

- **Linear Search**: Searches the entire list element by element.
- **Binary Search**:
    - Faster than linear search.
    - Eliminates half of the remaining elements at each step.
    - Works only on sorted arrays.

---

# Graphs

A graph consists of a finite set of vertices (nodes) and edges connecting them.

## Graph Terminology

- **Vertices (nodes)**: The elements of a graph.
- **Edge**: The connection between vertices.
- **Unweighted Graph**: A graph without edge weights.
- **Weighted Graph**: A graph with edge weights.
- **Undirected Graph**: A graph where edges have no direction.
- **Directed Graph**: A graph where edges have direction.
- **Cyclic Graph**: A graph containing at least one cycle.
- **Acyclic Graph**: A graph with no cycles.
- **Tree**: A special type of directed acyclic graph.

## Graph Representation

- **Adjacency Matrix**: Used for dense graphs.
- **Adjacency List**: Used for sparse graphs.

---

# BFS & DFS

## Breadth-First Search (BFS)
- Uses a queue.
- Explores all neighbors before moving to the next level.

## Depth-First Search (DFS)
- Uses a stack.
- Explores as far as possible before backtracking.

---

# Topological Sort

Sorts actions such that dependent actions always come after their dependencies.

---

# Single Source Shortest Path Problem (SSS_PP)

Can be solved using:

- BFS
- Dijkstra's Algorithm
- Bellman-Ford Algorithm

**Note:** BFS does not work with weighted graphs. Use Dijkstra's or Bellman-Ford instead.

---

# Dijkstra's Algorithm

Finds the shortest path in a weighted graph (e.g., road networks).

---

# Bellman-Ford Algorithm

Solves the single-source shortest path problem and detects negative cycles.

---

# Floyd Warshall Algorithm

Used for:

- Checking if a vertex is reachable.
- Finding direct or indirect connections between vertices.

---

# Minimum Spanning Tree (MST)

A subset of a graph's edges that:

- Connects all vertices.
- Forms no cycles.
- Minimizes the total edge weight.

## Disjoint Set

A data structure for partitioning elements into disjoint sets, using:

- `makeSet(N)`
- `union(x, y)`
- `findSet(x)`

---

# Kruskal and Prim's Algorithms

## Kruskal's Algorithm (Greedy Algorithm)

- Adds edges in increasing order of weight.
- Avoids cycles.

## Prim's Algorithm

- Starts with an arbitrary vertex.
- Adds the minimum edge that does not form a cycle.

---

# Greedy Algorithms

Examples:

- Insertion Sort
- Selection Sort
- Topological Sort
- Prim's Algorithm
- Kruskal's Algorithm

### Problems

- **Activity Selection Problem**: Maximize activities a person can perform given their start and finish times.
- **Coin Change Problem**: Count the number of ways to make a given sum using available denominations.
- **Fractional Knapsack Problem**: Maximize the total value in a knapsack with a given capacity.

---

# Divide and Conquer Algorithms

Examples:

- Merge Sort
- Quick Sort

### Problems

- Number Factor
- House Robber
- Convert one string to another
- Zero-one Knapsack
- Longest Common Subsequence
- Longest Palindromic Subsequence
- Minimum Cost to Reach Last Cell
- Number of Paths to Last Cell with Given Cost

---

# Dynamic Programming

An optimization technique that breaks a problem into subproblems and stores their results.

## Approaches

- **Top Down (Memoization)**: Solves subproblems recursively and caches results.
- **Bottom Up (Tabulation)**: Solves problems iteratively, filling a table to compute the final result.

---

# Problem Solving Recipe

1. Understand the problem.
2. Explore examples.
3. Break it down.
4. Solve.
5. Look back and refactor.

---

# Backtracking

A problem-solving technique that incrementally builds solutions and backtracks when an option fails.
