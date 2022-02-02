# **INTRODUCTION TO HEAP**
A Heap is a special Tree-based data structure in which the tree is a complete binary tree.


**Binary Heap Properties**.
1) It’s a complete tree (All levels are completely filled except possibly the last level and the last level has all keys as left as possible). This property of Binary Heap makes them suitable to be stored in an array.
2) A Binary Heap is either Min Heap or Max Heap. In a Min Binary Heap, the key at root must be minimum among all keys present in Binary Heap. The same property must be recursively true for all nodes in Binary Tree. Max Binary Heap is similar to MinHeap.


**Apllications Of Heap**
1. Heap sort (nlogn)
2. Priority Queue (It used in some Graph Algo like Dijkstra’s Shortest Path and Prim’s Minimum Spanning Tree.
3. Used where we have maintain max or min in window or arr (Regular interval need min or max)


**Operations On Heap**
1. getMin() or getMax() -  It returns the root element of Min Heap. Time Complexity of this operation is O(1).
2. extractMin() or extractMax() - Removes the root element from Heap. O(Logn) Complexity as this operation needs to maintain the heap property (by calling heapify()) after removing root.
3. insert(val) - Inserting a new key takes O(Logn) time. We add a new key at the end of the tree. If it voilates heap property then we need to traverse up to fix the violated heap property.
4. downHeapify(index) - Maintain heap property on index for down tree. O(logn)
5. upHeapify(index) - Maintain heap property on index for up tree. O(logn)
6. changeValue(index, value)  or decreaseKey(i,v) or increaseKey(i,v) - O(Logn) complexity. If new value grater than previous value then run `upHeapify` otherwise run `downHeapify` in case of max heap.
7. delete(index) - Delete value at index and maintain its property O(Logn) complexity
 
