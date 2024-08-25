---
weight: 3000
title: "Chapter 19"
description: "Amortized Algorithms"
icon: "article"
date: "2024-08-24T23:42:26+07:00"
lastmod: "2024-08-24T23:42:26+07:00"
draft: false
toc: true
katex: true
---
<center>

# 📘 Chapter 19: Amortized Algorithms

</center>

{{% alert icon="💡" context="info" %}}
<strong>"<em>Never express yourself more clearly than you are able to think.</em>" — Niels Bohr</strong>
{{% /alert %}}

{{% alert icon="📘" context="success" %}}
<p style="text-align: justify;">
<strong>Chapter 19 of DSAR delves into the intricacies of amortized analysis, providing a thorough exploration of techniques and applications vital for understanding the performance of data structures over sequences of operations. It begins with an introduction to amortized analysis, highlighting its significance in assessing the average cost of operations where occasional costly operations are spread over a sequence. The chapter meticulously covers three primary amortized analysis techniques: aggregate analysis, which computes the average cost by aggregating total costs over multiple operations; accounting method, which assigns credits or debits to manage varying operational costs; and the potential method, which employs a potential function to balance cost fluctuations. Real-world applications of amortized algorithms are explored through dynamic arrays, hash tables, and self-balancing trees, illustrating how these concepts maintain efficient average-case performance. Additionally, the chapter examines the synergy between amortized and worst-case analysis, offering insights into achieving both efficiency and robustness. Advanced topics further extend the discussion to complex data structures and the evolving applications of amortized analysis, ensuring a comprehensive understanding of performance guarantees and optimization techniques in modern algorithms.</strong>
</p>
{{% /alert %}}

# 19.1. Introduction to Amortized Analysis
<p style="text-align: justify;">
Amortized analysis is a critical tool in the study of algorithms, particularly in understanding the long-term efficiency of operations in data structures. At its core, amortized analysis provides a performance measure that averages the cost of operations over a sequence rather than focusing on the worst-case or average-case cost of a single operation. This approach is especially useful in data structures where certain operations might be expensive occasionally, but their cost is offset by a series of cheaper operations. By distributing the cost of these expensive operations across multiple operations, amortized analysis allows for a more accurate depiction of the data structure's efficiency over time.
</p>

<p style="text-align: justify;">
To fully grasp the concept of amortized analysis, it is essential to differentiate it from average-case analysis. Average-case analysis examines the expected cost of an individual operation by considering all possible inputs and the probability distribution of these inputs. It provides a measure of performance under typical conditions but does not account for the sequence of operations. Amortized analysis, on the other hand, looks at a sequence of operations and considers how the costs are distributed among them. This approach is particularly valuable when a data structure undergoes occasional costly operations, such as re-sizing an array or rehashing in a hash table, which can significantly skew the average-case analysis if not properly accounted for. Amortized analysis smooths out these spikes in cost, providing a clearer picture of the overall efficiency.
</p>

<p style="text-align: justify;">
The purpose of amortized analysis extends beyond merely providing an average cost measure; it offers insights into the long-term behavior of data structures. When analyzing operations that might have occasional high costs, such as dynamic array resizing or splay tree adjustments, amortized analysis reveals that these high costs are not as detrimental as they might initially appear. By distributing the cost over multiple operations, it becomes evident that the average cost per operation remains low, ensuring that the data structure performs efficiently in practice. This understanding is crucial for designing algorithms and data structures that need to maintain high performance even when faced with occasional expensive operations.
</p>

<p style="text-align: justify;">
To perform amortized analysis, three primary techniques are commonly employed: the aggregate method, the accounting method, and the potential method. Each of these techniques offers a unique approach to analyzing the amortized cost and provides different insights into the data structure's behavior.
</p>

<p style="text-align: justify;">
The aggregate method is perhaps the most straightforward approach. It involves calculating the total cost of a sequence of operations and then dividing this total by the number of operations. This calculation yields the average cost per operation over the sequence. The aggregate method is particularly useful when the sequence of operations is uniform, and the expensive operations are spread evenly throughout the sequence. By focusing on the total cost, the aggregate method provides a clear and simple way to understand the overall efficiency of the data structure.
</p>

<p style="text-align: justify;">
The accounting method introduces a more nuanced approach by assigning different costs to operations—both actual and amortized. This method works by maintaining a balance or credit/debit system, where operations that are cheaper than average accumulate credits, and more expensive operations deplete these credits. The key to this method is to ensure that the total balance remains non-negative throughout the sequence of operations. This technique is especially powerful when analyzing data structures where certain operations can be seen as "paying forward" for future costly operations, thus spreading the cost more evenly over time.
</p>

<p style="text-align: justify;">
Finally, the potential method uses a potential function to represent the state of the data structure. The potential function assigns a value to the data structure based on its current configuration, and this value changes as operations are performed. The amortized cost of an operation is then determined by the actual cost of the operation plus the change in potential. If the potential increases, it indicates that future operations might be more costly, while a decrease in potential suggests that future operations will be cheaper. The potential method is particularly well-suited for data structures where the state changes significantly with each operation, as it captures the dynamic nature of the data structure more effectively than the other methods.
</p>

<p style="text-align: justify;">
In summary, amortized analysis is a powerful tool for understanding the long-term efficiency of data structures, particularly in scenarios where operations may have varying costs. By providing an average cost measure over a sequence of operations, amortized analysis offers insights that are not captured by average-case or worst-case analysis alone. The aggregate method, accounting method, and potential method each provide different perspectives on how to distribute and analyze the cost of operations, allowing for a comprehensive understanding of the data structure's performance.
</p>

# 19.2. Amortized Analysis Techniques
<p style="text-align: justify;">
In the study of algorithms, particularly when dealing with data structures that undergo sequences of operations, amortized analysis provides a more accurate reflection of performance over time. The goal is to understand not just the cost of individual operations but how these costs are distributed across a series of operations. This approach is especially useful in scenarios where some operations are costly, but their impact is mitigated when viewed in the context of the entire sequence. Three primary techniques for conducting amortized analysis are aggregate analysis, the accounting method, and the potential method. Each technique offers a different perspective on understanding and distributing the cost of operations in data structures.
</p>

<p style="text-align: justify;">
Aggregate analysis is the simplest of these techniques, providing a straightforward approach to amortized analysis. The essence of aggregate analysis lies in computing the total cost of a sequence of operations and then dividing this total by the number of operations performed. This method yields an average cost per operation, smoothing out the occasional high-cost operations across the entire sequence. Aggregate analysis is particularly effective in scenarios where operations can be naturally grouped and analyzed collectively. For instance, in the analysis of dynamic arrays, the cost of operations such as insertion and resizing can be aggregated. While resizing an array is an expensive operation when it occurs, aggregate analysis reveals that, over time, the cost per insertion operation remains constant and low. By considering the sequence as a whole, aggregate analysis demonstrates that the overall efficiency of the dynamic array is maintained, despite the occasional high-cost resizing operations.
</p>

<p style="text-align: justify;">
The accounting method offers a more granular approach to amortized analysis, focusing on the cost of individual operations while maintaining an account balance to track these costs. In this method, each operation is assigned a “credit” or “debit” depending on whether it is inexpensive or requires additional work. The idea is to assign an amortized cost to each operation that may differ from its actual cost, thereby spreading out the expense of costly operations across the sequence. The accounting method is particularly useful in data structures where some operations are cheap, but others, though rare, are costly. A classic example is the analysis of stack operations. Most stack operations, such as pushing and popping elements, are inexpensive. However, occasionally, an operation might involve extra work, such as when a new block of memory needs to be allocated. By using the accounting method, credits can be assigned to the inexpensive operations, which are then used to "pay" for the more expensive ones. This ensures that the overall cost remains balanced and that the occasional high-cost operations do not skew the performance analysis.
</p>

<p style="text-align: justify;">
The potential method introduces a more sophisticated approach by incorporating a potential function that captures the state of the data structure at any given time. This potential function is a mathematical construct that reflects the "stored energy" or potential cost associated with the data structure's current state. The amortized cost of an operation is then calculated as the actual cost plus the change in potential. This method is particularly effective in scenarios where the cost of operations can vary significantly depending on the state of the data structure. For instance, in data structures like splay trees or Fibonacci heaps, the cost of operations can fluctuate dramatically depending on the sequence of previous operations. The potential method allows for a more nuanced analysis by considering how the state of the data structure evolves over time. By accounting for the change in potential, this method provides a clear picture of the long-term efficiency of the data structure, ensuring that even when costs vary widely, the overall performance remains stable and predictable.
</p>

<p style="text-align: justify;">
In summary, the techniques of aggregate analysis, the accounting method, and the potential method each offer unique insights into the amortized analysis of algorithms. Aggregate analysis provides a straightforward way to average costs across a sequence of operations, making it ideal for simple scenarios. The accounting method allows for a more detailed examination of individual operations, ensuring that even rare, costly operations do not disproportionately affect the overall performance. The potential method offers a dynamic approach, accounting for changes in the state of the data structure and providing a deeper understanding of how these changes impact long-term efficiency. Together, these techniques form a comprehensive toolkit for analyzing and optimizing the performance of data structures in a wide range of applications.
</p>

# 19.3. Real-World Applications of Amortized Algorithms
<p style="text-align: justify;">
Amortized analysis plays a crucial role in understanding and optimizing the performance of various data structures used in real-world applications. By examining the cost of operations over a sequence rather than individually, amortized analysis ensures that data structures remain efficient even when they involve occasional expensive operations. In this section, we will explore three practical applications of amortized algorithms: dynamic arrays, hash tables, and self-balancing trees. For each, we will delve into the concept, provide pseudo codes, and present sample implementation codes in Rust, along with an in-depth discussion of how amortized analysis is applied.
</p>

## 19.3.1. Dynamic Arrays
<p style="text-align: justify;">
Dynamic arrays, also known as resizable arrays, are a common data structure that uses amortized analysis to handle resizing operations efficiently. When the array reaches its capacity, it needs to be resized to accommodate additional elements. This resizing operation involves allocating a new, larger array and copying all elements from the old array to the new one. While the resizing operation is costly, its cost is amortized over the multiple insertions that occur between resizing operations, ensuring that the average cost of inserting an element remains constant over a sequence of operations.
</p>

#### Pseudo Code
{{< prism lang="text" line-numbers="true">}}
function insert(element):
    if size == capacity:
        resize()
    array[size] = element
    size += 1

function resize():
    new_capacity = 2 * capacity
    new_array = allocate new array with new_capacity
    for i from 0 to size-1:
        new_array[i] = array[i]
    array = new_array
    capacity = new_capacity
{{< /prism >}}
#### Rust Implementation
{{< prism lang="rust" line-numbers="true">}}
struct DynamicArray<T> {
    array: Vec<T>,
    size: usize,
    capacity: usize,
}

impl<T> DynamicArray<T> {
    fn new() -> Self {
        DynamicArray {
            array: Vec::with_capacity(4),
            size: 0,
            capacity: 4,
        }
    }

    fn insert(&mut self, element: T) {
        if self.size == self.capacity {
            self.resize();
        }
        self.array.push(element);
        self.size += 1;
    }

    fn resize(&mut self) {
        self.capacity *= 2;
        let mut new_array = Vec::with_capacity(self.capacity);
        new_array.extend_from_slice(&self.array);
        self.array = new_array;
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In this implementation, the <code>DynamicArray</code> struct in Rust manages a dynamic array. The <code>insert</code> function checks if the array has reached its capacity. If it has, the <code>resize</code> function is called to double the capacity of the array. The costly <code>resize</code> operation is amortized over the multiple <code>insert</code> operations, resulting in an average constant time complexity, $O(1)$, for each insertion.
</p>

## 19.3.2. Hash Tables
<p style="text-align: justify;">
Hash tables are a widely used data structure for implementing associative arrays, allowing for efficient insertion, deletion, and lookup operations. However, as more elements are inserted, the load factor (the ratio of the number of elements to the number of buckets) increases, potentially leading to more collisions and degraded performance. To maintain efficiency, hash tables are typically resized by increasing the number of buckets and rehashing all existing elements. This rehashing operation is costly, but amortized analysis ensures that its cost is spread over future operations, allowing the hash table to maintain efficient average-case performance.
</p>

#### Pseudo Code
{{< prism lang="text" line-numbers="true">}}
function insert(key, value):
    if load_factor > threshold:
        rehash()
    index = hash(key)
    if table[index] is empty:
        table[index] = create new bucket
    table[index].insert(key, value)

function rehash():
    new_table = allocate new table with increased capacity
    for each bucket in table:
        for each (key, value) in bucket:
            new_index = hash(key)
            new_table[new_index].insert(key, value)
    table = new_table
{{< /prism >}}
#### Rust Implementation
{{< prism lang="rust" line-numbers="true">}}
use std::collections::hash_map::DefaultHasher;
use std::hash::{Hash, Hasher};

struct HashTable<K, V> {
    table: Vec<Vec<(K, V)>>,
    size: usize,
    capacity: usize,
    threshold: f64,
}

impl<K: Hash + Eq + Clone, V: Clone> HashTable<K, V> {
    fn new() -> Self {
        HashTable {
            table: vec![Vec::new(); 4],
            size: 0,
            capacity: 4,
            threshold: 0.75,
        }
    }

    fn insert(&mut self, key: K, value: V) {
        if self.load_factor() > self.threshold {
            self.rehash();
        }
        let index = self.hash(&key);
        self.table[index].push((key, value));
        self.size += 1;
    }

    fn hash(&self, key: &K) -> usize {
        let mut hasher = DefaultHasher::new();
        key.hash(&mut hasher);
        (hasher.finish() as usize) % self.capacity
    }

    fn load_factor(&self) -> f64 {
        self.size as f64 / self.capacity as f64
    }

    fn rehash(&mut self) {
        self.capacity *= 2;
        let mut new_table = vec![Vec::new(); self.capacity];
        for bucket in self.table.drain(..) {
            for (key, value) in bucket {
                let new_index = self.hash(&key);
                new_table[new_index].push((key, value));
            }
        }
        self.table = new_table;
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In this implementation, the <code>HashTable</code> struct in Rust manages a hash table with separate chaining. The <code>insert</code> function first checks if the load factor exceeds the threshold, triggering a <code>rehash</code> if necessary. The <code>rehash</code> function doubles the table's capacity and redistributes all existing elements into the new table. The cost of rehashing is amortized over subsequent insertions, ensuring that the average time complexity for insertion remains $O(1)$.
</p>

## 19.3.3. Self-Balancing Trees
<p style="text-align: justify;">
Self-balancing trees, such as AVL trees and red-black trees, use amortized analysis to manage rebalancing operations. These trees maintain a balanced structure to ensure efficient performance for operations like insertion, deletion, and lookup. However, after certain operations, the tree may become unbalanced and require rotations or recoloring to restore balance. Although these rebalancing operations can be costly, their cost is amortized over a sequence of insertions and deletions, ensuring that the tree maintains efficient performance in the long run.
</p>

#### Pseudo Code (AVL Tree Example)
{{< prism lang="text" line-numbers="true">}}
function insert(node, value):
    if node is null:
        return create new node with value
    if value < node.value:
        node.left = insert(node.left, value)
    else:
        node.right = insert(node.right, value)
    update_height(node)
    return balance(node)

function balance(node):
    balance_factor = height(node.left) - height(node.right)
    if balance_factor > 1:
        if value < node.left.value:
            return rotate_right(node)
        else:
            node.left = rotate_left(node.left)
            return rotate_right(node)
    if balance_factor < -1:
        if value > node.right.value:
            return rotate_left(node)
        else:
            node.right = rotate_right(node.right)
            return rotate_left(node)
    return node
{{< /prism >}}
#### Rust Implementation (AVL Tree Example)
{{< prism lang="rust" line-numbers="true">}}
use std::cmp::max;

struct AVLNode<T> {
    value: T,
    height: i32,
    left: Option<Box<AVLNode<T>>>,
    right: Option<Box<AVLNode<T>>>,
}

struct AVLTree<T> {
    root: Option<Box<AVLNode<T>>>,
}

impl<T: Ord> AVLTree<T> {
    fn new() -> Self {
        AVLTree { root: None }
    }

    fn insert(&mut self, value: T) {
        self.root = Self::insert_node(self.root.take(), value);
    }

    fn insert_node(node: Option<Box<AVLNode<T>>>, value: T) -> Option<Box<AVLNode<T>>> {
        if let Some(mut node) = node {
            if value < node.value {
                node.left = Self::insert_node(node.left.take(), value);
            } else {
                node.right = Self::insert_node(node.right.take(), value);
            }
            node.height = 1 + max(Self::height(&node.left), Self::height(&node.right));
            Some(Self::balance(node))
        } else {
            Some(Box::new(AVLNode {
                value,
                height: 1,
                left: None,
                right: None,
            }))
        }
    }

    fn height(node: &Option<Box<AVLNode<T>>>) -> i32 {
        node.as_ref().map_or(0, |n| n.height)
    }

    fn balance(mut node: Box<AVLNode<T>>) -> Box<AVLNode<T>> {
        let balance_factor = Self::height(&node.left) - Self::height(&node.right);
        if balance_factor > 1 {
            if Self::height(&node.left.as_ref().unwrap().left) >= Self::height(&node.left.as_ref().unwrap().right) {
                node = Self::rotate_right(node);
            } else {
                node.left = Some(Self::rotate_left(node.left.take().unwrap()));
                node = Self::rotate_right(node);
            }
        } else if balance_factor < -1 {
            if Self::height(&node.right.as_ref().unwrap().right) >= Self::height(&node.right.as_ref().unwrap().left) {
                node = Self::rotate_left(node);
            } else {
                node.right = Some(Self::rotate_right(node.right.take().unwrap()));
                node = Self::rotate_left(node);
            }
        }
        node
    }

    fn rotate_right(mut node: Box<AVLNode<T>>) -> Box<AVLNode<T>> {
        let mut left_node = node.left.take().unwrap();
        node.left = left_node.right.take();
        left_node.right = Some(node);
        left_node
    }

    fn rotate_left(mut node: Box<AVLNode<T>>) -> Box<AVLNode<T>> {
        let mut right_node = node.right.take().unwrap();
        node.right = right_node.left.take();
        right_node.left = Some(node);
        right_node
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In this Rust implementation of an AVL tree, the <code>AVLTree</code> struct manages a self-balancing binary search tree. The <code>insert_node</code> function handles the insertion of new elements, ensuring that the tree remains balanced by checking the balance factor after each insertion. If the tree becomes unbalanced, the <code>balance</code> function performs the necessary rotations to restore balance. These rotations, though potentially costly, are amortized over the sequence of insertions and deletions, ensuring that the tree maintains efficient performance with logarithmic time complexity for operations like insertion, deletion, and lookup.
</p>

<p style="text-align: justify;">
In conclusion, dynamic arrays, hash tables, and self-balancing trees all utilize amortized analysis to maintain efficient performance despite the occasional need for costly operations. By spreading the cost of these operations over a sequence, amortized analysis ensures that these data structures remain practical and efficient in real-world applications. Through the use of pseudo codes and Rust implementations, we have demonstrated how these concepts are applied in practice, providing a deeper understanding of the power and utility of amortized algorithms.
</p>

# 19.4. Combining Amortized and Worst-Case Analysis
<p style="text-align: justify;">
In the analysis of algorithms, different techniques are employed to understand and optimize performance. Amortized analysis, which averages the cost of operations over a sequence, provides insights into the typical performance of data structures in the long run. On the other hand, worst-case analysis focuses on the maximum possible cost of an operation, ensuring that performance remains within acceptable bounds even in the most demanding scenarios. While each of these methods has its strengths, combining amortized and worst-case analysis—referred to as hybrid analysis—offers a more comprehensive understanding of an algorithm's performance. This approach balances worst-case guarantees with the efficiency insights provided by amortized analysis, yielding a more robust perspective on both safety and efficiency.
</p>

<p style="text-align: justify;">
Hybrid analysis is particularly valuable because it addresses the limitations inherent in using either amortized or worst-case analysis alone. Amortized analysis excels at providing an average-case performance measure over a sequence of operations, smoothing out the occasional high-cost operations. However, it does not necessarily guarantee performance in the worst-case scenario, which is where worst-case analysis comes into play. Worst-case analysis, while ensuring that an algorithm performs within specified bounds even in the most challenging cases, can sometimes be overly pessimistic, especially in scenarios where the worst-case is rare. By combining these two approaches, hybrid analysis allows us to capture the benefits of both: the practical, often lower average costs identified by amortized analysis, and the safety nets provided by worst-case guarantees.
</p>

<p style="text-align: justify;">
One classic example of hybrid analysis can be found in the study of dynamic arrays. Dynamic arrays, such as those implemented using vectors in Rust, rely on amortized analysis to manage resizing operations. When a dynamic array reaches its capacity, it must be resized, typically by doubling its capacity. This resizing operation is expensive, as it involves allocating a new array and copying all elements from the old array to the new one. Amortized analysis shows that, despite the occasional high cost of resizing, the average cost of inserting an element remains constant, $O(1)$, when considered over a long sequence of operations. However, this does not mean that the worst-case scenario—when resizing is necessary—should be ignored. By incorporating worst-case analysis, we can understand that during the resizing operation, the cost is $O(n)$, where nnn is the number of elements in the array. Hybrid analysis thus provides a dual perspective: the typical, low average cost of insertion and the higher, but bounded, cost during resizing. This combined view ensures that the dynamic array remains efficient in practice while also preparing for the worst-case scenario when resizing is triggered.
</p>

<p style="text-align: justify;">
Another application of hybrid analysis is seen in stack operations. In a typical stack, operations like push and pop are inexpensive, with each operation generally taking constant time, $O(1)$. Amortized analysis can be used to understand these operations over a sequence, ensuring that the stack operates efficiently under typical conditions. However, other operations, such as peak or reset, might have different cost characteristics. For instance, if the stack needs to be reset to an initial state, this could involve clearing a large number of elements, which might have a worst-case cost of $O(n)$. By combining amortized analysis of the frequent push/pop operations with worst-case analysis of the less frequent but potentially expensive operations, hybrid analysis provides a comprehensive understanding of the stack’s performance. This combined analysis ensures that while the stack operates efficiently most of the time, the performance during less common, expensive operations is also well understood and bounded.
</p>

<p style="text-align: justify;">
The purpose of hybrid analysis is to provide a balanced view of an algorithm's performance. It allows algorithm designers and engineers to optimize for efficiency in the common case while still safeguarding against poor performance in the worst-case scenario. This approach is particularly valuable in systems where both efficiency and reliability are critical. For example, in real-time systems, where predictable performance is essential, worst-case analysis ensures that even in the most demanding situations, the system will not fail to meet its deadlines. At the same time, amortized analysis ensures that the system remains efficient during normal operation, avoiding unnecessary pessimism that could lead to over-provisioning of resources.
</p>

<p style="text-align: justify;">
In summary, hybrid analysis is a powerful tool that combines the strengths of amortized and worst-case analysis to provide a comprehensive understanding of an algorithm’s performance. By applying this approach to dynamic arrays, stack operations, and other data structures, we can ensure that our algorithms are not only efficient on average but also robust in the face of the worst-case scenarios. This balanced perspective is crucial for designing data structures and algorithms that perform well in both everyday use and in the most challenging conditions, ensuring that they are both practical and reliable in real-world applications.
</p>

# 19.5. Advanced Topics in Amortized Analysis
<p style="text-align: justify;">
In the realm of data structures and algorithms, amortized analysis serves as a crucial tool for understanding the long-term performance of operations. While it is commonly applied to fundamental data structures like dynamic arrays and hash tables, its true power becomes evident when examining more advanced data structures and adapting its principles to new, emerging paradigms. In this section, we explore the application of amortized analysis to advanced data structures such as Fibonacci heaps and van Emde Boas trees, discuss how amortized analysis can be adapted to modern algorithmic challenges, and examine the importance of deriving tight complexity bounds to refine performance guarantees.
</p>

## 19.5.1. Advanced Data Structures
<p style="text-align: justify;">
Fibonacci heaps and van Emde Boas trees represent sophisticated data structures where amortized analysis reveals the underlying efficiency that might not be immediately apparent through traditional worst-case analysis. These structures are designed to optimize specific operations, making them highly effective in certain algorithmic contexts, such as graph algorithms and priority queue management.
</p>

<p style="text-align: justify;">
Fibonacci heaps are a type of priority queue that supports a variety of operations efficiently. The strength of Fibonacci heaps lies in their ability to perform decrease-key and delete operations in logarithmic amortized time while supporting other operations like insertion and find-min in constant amortized time. This efficiency is achieved by allowing a more relaxed structure compared to binary heaps, leading to fewer structural modifications during operations.
</p>

##### Pseudo Code
{{< prism lang="text" line-numbers="true">}}
function insert(heap, x):
    add x to root list of heap
    if x.key < heap.min.key:
        heap.min = x
    heap.size += 1

function decrease_key(heap, x, k):
    if k > x.key:
        error "new key is greater than current key"
    x.key = k
    y = x.parent
    if y != null and x.key < y.key:
        cut(heap, x, y)
        cascading_cut(heap, y)
    if x.key < heap.min.key:
        heap.min = x
{{< /prism >}}
##### Rust Implementation
{{< prism lang="rust" line-numbers="true">}}
struct FibonacciHeapNode<T> {
    key: T,
    degree: usize,
    mark: bool,
    parent: Option<Box<FibonacciHeapNode<T>>>,
    child: Option<Box<FibonacciHeapNode<T>>>,
    left: Option<Box<FibonacciHeapNode<T>>>,
    right: Option<Box<FibonacciHeapNode<T>>>,
}

struct FibonacciHeap<T> {
    min: Option<Box<FibonacciHeapNode<T>>>,
    size: usize,
}

impl<T: Ord + Clone> FibonacciHeap<T> {
    fn new() -> Self {
        FibonacciHeap { min: None, size: 0 }
    }

    fn insert(&mut self, key: T) {
        let node = Box::new(FibonacciHeapNode {
            key,
            degree: 0,
            mark: false,
            parent: None,
            child: None,
            left: None,
            right: None,
        });
        if self.min.is_none() {
            self.min = Some(node);
        } else {
            // Add node to root list and update min if necessary
            if node.key < self.min.as_ref().unwrap().key {
                self.min = Some(node);
            }
        }
        self.size += 1;
    }

    fn decrease_key(&mut self, node: &mut FibonacciHeapNode<T>, new_key: T) {
        if new_key > node.key {
            panic!("New key is greater than current key");
        }
        node.key = new_key.clone();
        if let Some(ref parent) = node.parent {
            if node.key < parent.key {
                self.cut(node);
                self.cascading_cut(parent);
            }
        }
        if node.key < self.min.as_ref().unwrap().key {
            self.min = Some(Box::new(node.clone()));
        }
    }

    fn cut(&mut self, node: &mut FibonacciHeapNode<T>) {
        // Logic to remove node from its parent and add it to the root list
    }

    fn cascading_cut(&mut self, node: &mut FibonacciHeapNode<T>) {
        // Logic for cascading cut operation
    }
}
{{< /prism >}}
<p style="text-align: justify;">
In this Rust implementation, the <code>FibonacciHeap</code> struct manages a Fibonacci heap with methods for insertion and decreasing a key. The <code>decrease_key</code> function demonstrates how the heap’s structure is manipulated while ensuring that the amortized time complexity remains efficient. The expensive operations, such as cascading cuts, are amortized over multiple operations, ensuring that the average performance remains optimal.
</p>

<p style="text-align: justify;">
The van Emde Boas (vEB) trees are advanced data structures that support a variety of operations on a dynamic set of integers in sub-logarithmic time, specifically in $O(\log \log u)$, where uuu is the universe size. This efficiency is achieved by recursively dividing the universe into smaller blocks, allowing for fast operations like minimum, maximum, insertion, and deletion.
</p>

##### Pseudo Code
{{< prism lang="text" line-numbers="true">}}
function insert(vEB, x):
    if vEB.min is null:
        vEB.min = x
        vEB.max = x
    else:
        if x < vEB.min:
            swap(x, vEB.min)
        if vEB.cluster[high(x)] is empty:
            insert(vEB.summary, high(x))
            vEB.cluster[high(x)].min = low(x)
        else:
            insert(vEB.cluster[high(x)], low(x))
        if x > vEB.max:
            vEB.max = x
{{< /prism >}}
##### Rust Implementation
{{< prism lang="rust" line-numbers="true">}}
struct VebTree {
    u: usize,
    min: Option<usize>,
    max: Option<usize>,
    summary: Option<Box<VebTree>>,
    cluster: Vec<Option<Box<VebTree>>>,
}

impl VebTree {
    fn new(u: usize) -> Self {
        VebTree {
            u,
            min: None,
            max: None,
            summary: None,
            cluster: vec![None; (u as f64).sqrt() as usize],
        }
    }

    fn high(&self, x: usize) -> usize {
        x / (self.u as f64).sqrt() as usize
    }

    fn low(&self, x: usize) -> usize {
        x % (self.u as f64).sqrt() as usize
    }

    fn insert(&mut self, x: usize) {
        if self.min.is_none() {
            self.min = Some(x);
            self.max = Some(x);
        } else {
            if x < self.min.unwrap() {
                std::mem::swap(&mut x, &mut self.min.unwrap());
            }
            let high = self.high(x);
            let low = self.low(x);
            if self.cluster[high].is_none() {
                let mut new_cluster = VebTree::new(self.u.sqrt());
                new_cluster.min = Some(low);
                self.cluster[high] = Some(Box::new(new_cluster));
                self.summary.as_mut().unwrap().insert(high);
            } else {
                self.cluster[high].as_mut().unwrap().insert(low);
            }
            if x > self.max.unwrap() {
                self.max = Some(x);
            }
        }
    }
}
{{< /prism >}}
<p style="text-align: justify;">
This Rust implementation demonstrates the structure and insertion method for a vEB tree. The <code>insert</code> function handles the insertion of elements by recursively placing them into appropriate clusters, ensuring the tree remains balanced. The complexity of this structure is managed through amortized analysis, revealing its efficiency despite the seemingly intricate recursive operations.
</p>

## 19.5.2. Adapting Amortized Analysis
<p style="text-align: justify;">
As data structures and algorithms evolve, amortized analysis must be adapted to new and emerging paradigms. Modern computing environments, such as distributed systems and concurrent data structures, present unique challenges that require tailored amortized analysis techniques.
</p>

<p style="text-align: justify;">
For example, in distributed systems, operations might involve communication costs across network nodes, which can vary significantly depending on the network's state. Amortized analysis in this context might involve balancing the cost of data synchronization or consistency checks over multiple operations to ensure that the system remains efficient despite occasional network delays.
</p>

<p style="text-align: justify;">
In concurrent data structures, where multiple threads may simultaneously access and modify the same data, amortized analysis can be adapted to account for the cost of synchronization mechanisms, such as locks or atomic operations. By amortizing these costs over many operations, we can ensure that the data structure performs well even in highly concurrent environments.
</p>

<p style="text-align: justify;">
Deriving tight bounds on the amortized costs of algorithms and data structures is essential for refining performance guarantees. While amortized analysis provides an average-case view, establishing precise complexity bounds ensures that these guarantees are meaningful and applicable in real-world scenarios.
</p>

<p style="text-align: justify;">
For instance, in the context of Fibonacci heaps, the tight amortized bound for decrease-key operations is $O(1)$, which is crucial for applications like Dijkstra's algorithm, where frequent decrease-key operations are required. By deriving and validating these bounds, we provide stronger assurances of an algorithm’s performance, enabling more predictable and reliable software.
</p>

<p style="text-align: justify;">
Similarly, for van Emde Boas trees, the $O(\log \log u)$ bound on operations like insertion and deletion ensures that these structures remain highly efficient for applications requiring fast access to a dynamic set of integers. Establishing these bounds through rigorous analysis allows developers to confidently utilize these data structures in performance-critical applications.
</p>

<p style="text-align: justify;">
In conclusion, advanced data structures like Fibonacci heaps and van Emde Boas trees illustrate the power of amortized analysis in revealing the hidden efficiencies of complex algorithms. As new paradigms emerge, adapting amortized analysis to these contexts ensures that we continue to optimize and refine our understanding of algorithmic performance. By deriving tight complexity bounds, we can provide rigorous performance guarantees, enabling the development of more efficient and reliable software systems. Through the combination of practical examples, pseudo codes, and Rust implementations, this section highlights the relevance and applicability of amortized analysis in modern computing.
</p>

# 19.6. Conclusion
<p style="text-align: justify;">
Rust's strong typing system and performance guarantees make it an excellent choice for experimenting with amortized analysis concepts. Exploring amortized algorithms through the following prompts and self-exercises will not only deepen your theoretical understanding but also enhance your practical coding skills in Rust.
</p>

## 19.6.1. Advices
<p style="text-align: justify;">
Start by familiarizing yourself with the foundational concepts of amortized analysis, including aggregate analysis, the accounting method, and the potential method. Rust's ability to enforce strict type safety and provide detailed compile-time errors can help you understand these techniques deeply. Implement basic data structures, such as dynamic arrays and hash tables, to observe how amortized costs are calculated and managed. For dynamic arrays, create a custom implementation that dynamically resizes based on capacity. Use Rust’s powerful standard library features, such as <code>Vec</code>, to experiment with resizing strategies and analyze the costs associated with these operations. By doing so, you’ll gain insights into how amortized analysis ensures that the average cost per operation remains efficient despite occasional costly resizing events.
</p>

<p style="text-align: justify;">
Next, delve into hash tables by implementing a simple version yourself or by modifying existing Rust libraries. Pay special attention to how rehashing is handled and analyze the amortized cost of this operation. Rust's built-in data structures and their methods can be used to verify and compare your implementation against established libraries, offering a practical understanding of how theoretical analysis translates into real-world performance.
</p>

<p style="text-align: justify;">
In addition to basic data structures, explore more advanced data structures like self-balancing trees or Fibonacci heaps. Implementing these structures in Rust will not only deepen your understanding of amortized analysis but also enhance your skills in managing complex data structures with Rust's ownership and borrowing rules. For instance, implementing an AVL tree or a red-black tree in Rust will require careful management of tree balancing operations, which are key to understanding amortized costs.
</p>

<p style="text-align: justify;">
Combine your practical experiments with theoretical analysis by evaluating and comparing the amortized costs of different operations in your implementations. Use Rust’s profiling and benchmarking tools, such as <code>cargo bench</code>, to gather performance metrics and validate your theoretical calculations. This approach will help bridge the gap between theory and practice, giving you a comprehensive understanding of amortized algorithms in a real-world context.
</p>

<p style="text-align: justify;">
Finally, engage with the Rust community through forums or contribute to open-source projects that involve amortized analysis. This interaction can provide additional insights and expose you to diverse applications and optimizations. By integrating these practical experiences with your theoretical knowledge, you will gain a robust and in-depth understanding of amortized algorithms in Rust, preparing you to tackle complex algorithmic challenges effectively.
</p>

## 19.6.2. Further Learning with GenAI
<p style="text-align: justify;">
These prompts aim to explore the depths of amortized algorithms, focusing on fundamental principles, advanced techniques, and practical applications. They encourage a detailed examination of the theory behind amortized analysis, its application in various data structures, and how to implement these concepts in Rust. Each prompt is crafted to provide comprehensive, technically-rich responses that include Rust code samples and practical insights.
</p>

- <p style="text-align: justify;">Provide a thorough explanation of amortized analysis, including its theoretical foundations and key differences from average-case analysis. Discuss why amortized analysis is preferred in certain scenarios and how it provides a more nuanced view of an algorithm's performance. Include a Rust code example that demonstrates an amortized analysis approach for a specific data structure.</p>
- <p style="text-align: justify;">Detail the aggregate analysis technique in amortized analysis, including its mathematical underpinnings and how it applies to a sequence of operations. Implement a dynamic array in Rust and show how aggregate analysis is used to evaluate the cost of operations like resizing. Provide a detailed walkthrough of the Rust code and the corresponding amortized cost calculations.</p>
- <p style="text-align: justify;">Explain the accounting method in amortized analysis, focusing on how credits and debits are assigned to operations. Provide a comprehensive Rust example implementing a stack data structure with an accounting method. Detail how you determine the credit/debit for each operation and how it affects the amortized cost.</p>
- <p style="text-align: justify;">Define the potential method in amortized analysis and discuss its theoretical basis. Implement a binary search tree in Rust and use the potential method to analyze the amortized cost of operations like insertion and deletion. Provide a detailed explanation of the potential function used and how it affects the overall amortized cost.</p>
- <p style="text-align: justify;">Discuss the application of amortized analysis to dynamic arrays, including the impact of resizing operations on performance. Create a dynamic array in Rust and show how resizing is managed through amortized analysis. Include performance benchmarks and explain how they reflect the amortized cost of the operations.</p>
- <p style="text-align: justify;">Examine how amortized analysis is applied to hash tables, focusing on rehashing and its impact on performance. Implement a hash table in Rust with rehashing logic and analyze the amortized cost of insertions and deletions. Provide a detailed discussion of how hash collisions and rehashing affect the amortized cost.</p>
- <p style="text-align: justify;">Analyze the use of amortized analysis in self-balancing trees, such as AVL trees. Implement an AVL tree in Rust and discuss how amortized analysis is used to maintain balance and ensure efficient operations. Include detailed Rust code for insertion and balancing operations, and explain how amortized costs are calculated.</p>
- <p style="text-align: justify;">Explore the integration of amortized analysis with worst-case analysis in data structures. Provide a Rust implementation of a data structure where both types of analysis are relevant, such as a dynamic array with occasional costly operations. Discuss how combining these analyses provides a comprehensive performance evaluation.</p>
- <p style="text-align: justify;">Delve into advanced topics in amortized analysis, such as the analysis of Fibonacci heaps or van Emde Boas trees. Implement a Fibonacci heap in Rust and discuss its amortized cost characteristics, focusing on operations like insertions, deletions, and decrease-key. Explain how the advanced amortized analysis techniques apply to these structures.</p>
- <p style="text-align: justify;">Discuss the application of Rust’s <code>Vec</code> for dynamic arrays in the context of amortized analysis. Provide a detailed Rust implementation that demonstrates resizing and performance characteristics of <code>Vec</code>. Explain how <code>Vec</code> manages capacity and amortized costs, and compare it with theoretical predictions.</p>
- <p style="text-align: justify;">Implement a basic hash table in Rust, including the details of handling collisions and rehashing. Analyze the amortized cost of operations such as insertions, deletions, and lookups. Provide a detailed explanation of the hashing strategy used and how it affects the amortized performance of the hash table.</p>
- <p style="text-align: justify;">Provide a comprehensive implementation of a red-black tree in Rust, including insertion, rotation, and balancing operations. Explain how amortized analysis helps maintain the balanced properties of the tree and discuss the amortized cost of these operations. Include detailed code snippets and performance analysis.</p>
- <p style="text-align: justify;">Examine the concept of amortized cost in priority queues, particularly those implemented with binary heaps. Provide a Rust implementation of a priority queue and analyze the amortized cost of insertion and extraction operations. Explain how the heap property affects the amortized performance.</p>
- <p style="text-align: justify;">Compare different amortized analysis techniques—aggregate, accounting, and potential methods—by implementing them in Rust for the same data structure. Discuss the trade-offs and scenarios where each method is most applicable. Provide detailed Rust code examples and analyze the results for each technique.</p>
- <p style="text-align: justify;">Utilize Rust’s benchmarking tools, such as <code>cargo bench</code>, to validate amortized cost calculations for various data structures. Provide a comprehensive Rust example that measures and analyzes the performance of a data structure with amortized analysis. Discuss how benchmarking results align with theoretical predictions and what insights they provide.</p>
<p style="text-align: justify;">
By implementing and analyzing various data structures, you’ll gain valuable insights into how theoretical concepts translate into real-world performance. Embrace this opportunity to bridge the gap between theory and practice, experiment with the provided Rust code, and refine your understanding of amortized analysis. Your commitment to mastering these techniques will significantly enrich your algorithmic expertise and prepare you for tackling complex computational challenges with confidence. Dive into these prompts with curiosity and enthusiasm, and let your exploration lead to profound technical insights and advanced problem-solving skills.
</p>

## 19.6.3. Self-Exercises
<p style="text-align: justify;">
Here are five in-depth and comprehensive exercises designed to deepen students' understanding of amortized algorithms as covered in Chapter 19. Each exercise includes detailed requirements and objectives to ensure robust learning and practical application.
</p>

<p style="text-align: justify;">
<strong></strong>Exercise 19.1:<strong></strong> Implement and Analyze a Dynamic Array with Detailed Performance Metrics
</p>

- <p style="text-align: justify;"><strong></strong>Task<strong></strong>: Implement a dynamic array in Rust, similar to <code>Vec</code>, including methods for insertion, deletion, and resizing. Ensure that the resizing operation involves doubling the capacity to manage growth.</p>
- <p style="text-align: justify;"><strong></strong>Objective<strong></strong>: Perform aggregate analysis to calculate the amortized cost of the insertion operation. Implement functionality to track and log the number of operations performed and the frequency of resizing. Analyze how the resizing impacts the performance of the dynamic array.</p>
- <p style="text-align: justify;"><strong></strong>Deliverables<strong></strong>: Submit the complete Rust code for the dynamic array, including methods for insertion and resizing. Provide a detailed report that includes:</p>
- <p style="text-align: justify;">Theoretical amortized cost calculations and their alignment with your implementation.</p>
- <p style="text-align: justify;">Performance benchmarks showing the impact of resizing.</p>
- <p style="text-align: justify;">A discussion of how the aggregate analysis applies to the resizing operations and its practical implications.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 19.2:<strong></strong> Design and Benchmark a Custom Hash Table with Collision Handling and Rehashing
</p>

- <p style="text-align: justify;"><strong></strong>Task<strong></strong>: Develop a custom hash table in Rust using open addressing or separate chaining for collision handling. Implement methods for insertion, deletion, and searching, and include dynamic rehashing to manage load factors and ensure efficiency.</p>
- <p style="text-align: justify;"><strong></strong>Objective<strong></strong>: Use amortized analysis to evaluate the cost of hash table operations, focusing on rehashing. Implement a system to track the frequency of rehashing and its impact on performance.</p>
- <p style="text-align: justify;"><strong></strong>Deliverables<strong></strong>: Provide the Rust code for the hash table with detailed implementations of collision handling and rehashing. Submit a comprehensive report that includes:</p>
- <p style="text-align: justify;">Analysis of the amortized cost of operations with and without rehashing.</p>
- <p style="text-align: justify;">Performance benchmarks using <code>cargo bench</code> that illustrate how rehashing affects the hash table's efficiency.</p>
- <p style="text-align: justify;">A discussion on how different collision handling strategies impact amortized cost and overall performance.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 19.3:<strong></strong> Develop and Analyze a Self-Balancing Tree (AVL or Red-Black Tree)
</p>

- <p style="text-align: justify;"><strong></strong>Task<strong></strong>: Implement an AVL tree or red-black tree in Rust, ensuring that your implementation handles insertion, deletion, and tree balancing. Include methods for rotations and balancing to maintain tree properties.</p>
- <p style="text-align: justify;"><strong></strong>Objective<strong></strong>: Apply the potential method of amortized analysis to evaluate the cost of operations. Analyze how tree balancing (rotations) affects the amortized cost of insertions and deletions.</p>
- <p style="text-align: justify;"><strong></strong>Deliverables<strong></strong>: Submit the Rust code for the self-balancing tree, including methods for balancing and rotations. Provide a detailed report that includes:</p>
- <p style="text-align: justify;">An explanation of the potential function used and how it applies to amortized analysis.</p>
- <p style="text-align: justify;">Performance benchmarks showing the cost of balancing operations.</p>
- <p style="text-align: justify;">A discussion on the impact of tree balancing on the amortized cost and overall efficiency of the data structure.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 19.4:<strong></strong> Implement a Priority Queue with a Binary Heap and Perform Amortized Cost Analysis
</p>

- <p style="text-align: justify;"><strong></strong>Task<strong></strong>: Create a priority queue in Rust using a binary heap. Implement insertion, extraction of the minimum (or maximum), and heapification operations.</p>
- <p style="text-align: justify;"><strong></strong>Objective<strong></strong>: Analyze the amortized cost of priority queue operations, with a focus on insertion and extraction. Compare the performance of your implementation with theoretical amortized costs and provide insights into how the binary heap maintains efficiency.</p>
- <p style="text-align: justify;"><strong></strong>Deliverables<strong></strong>: Submit the Rust code for the priority queue, including detailed implementations of insertion and extraction. Provide a comprehensive report that includes:</p>
- <p style="text-align: justify;">Theoretical amortized cost calculations and performance benchmarks.</p>
- <p style="text-align: justify;">A comparison of theoretical and practical results.</p>
- <p style="text-align: justify;">Insights into how the binary heap's structure influences the amortized cost of operations.</p>
<p style="text-align: justify;">
<strong></strong>Exercise 19.5:<strong></strong> Comparative Study of Amortized Analysis Techniques Using a Common Data Structure
</p>

- <p style="text-align: justify;"><strong></strong>Task<strong></strong>: Implement a basic data structure (such as a stack or queue) in Rust. Apply aggregate, accounting, and potential methods of amortized analysis to evaluate the cost of operations.</p>
- <p style="text-align: justify;"><strong></strong>Objective<strong></strong>: Compare and contrast the results from each amortized analysis technique. Discuss the applicability and limitations of each technique in the context of your data structure.</p>
- <p style="text-align: justify;"><strong></strong>Deliverables<strong></strong>: Submit the Rust code for the data structure along with implementations of amortized analysis using aggregate, accounting, and potential methods. Provide a detailed comparative report that includes:</p>
- <p style="text-align: justify;">Theoretical analysis and practical results for each technique.</p>
- <p style="text-align: justify;">Performance data and insights into the strengths and weaknesses of each analysis method.</p>
- <p style="text-align: justify;">A discussion on which technique provides the most meaningful insights for different types of data structures and operations.</p>
<p style="text-align: justify;">
These exercises are designed to provide a thorough understanding of amortized algorithms through hands-on implementation and in-depth analysis. By completing these assignments, students will gain practical experience and a deep appreciation for the nuances of amortized analysis in algorithm design.
</p>
