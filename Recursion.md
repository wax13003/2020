# **Recursion**
### **Recursion** 需要掌握的知识点：
> 1. 表象上: function call itself
> 2. 实际上: Boil down a big problem to smaller ones (size n depends on size n-1, or n-2, or...n/2)
> 3. **Implementation**上:    
> a. **Base case**: smallest problem to solve   
> b. **Recursion rule**: how to make the problem smaller (if we can resolve the same problem but with a smaller size, then what is left to do for the current size n)

**How to analyze time and space complexity of a recursive function**
* Time complexity: The sum of time complexity of all nodes in the recursion tree. -->Recursion tree里所有节点的时间复杂度之和
* Space complexity: The sum of space complexity of all nodes on a pink path(a path from top to bottom) -->Recursion tree里直上直下的粉色路径上所有节点的空间复杂度之和

TODO:
Recursion和各种数据结构结合
1. Recursion 和 Math相关
2. Recursion 与 1D or 2D array的结合
    1. 1D array 二分法比较常见
        1. Merge sort
        2. Quick sort
    2. 2D array
        1. 8 queen -> n queen
        2. spiral matrix
3. Recursion 与 LinkedList 的结合
    1. reverse a LinkedList 
    2. reverse a LinkedList in pairs
4. Recursion 与 String 的结合
5. Recursion 与 Tree 的结合
