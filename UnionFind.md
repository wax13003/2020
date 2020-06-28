# **Union Find**
## **Background**
### Computer Connection Problem:

    Given many computers, and that you can connect two computers anytime, and that you want to know whether two computers are connected (directly or indirectly) anytime. What data structure and algorithm can you use?

### Example:

    You have three computers A, B, C  
    At first, A B are not connected, B C are not connected, A C are not connected  
    Then you connect A, B
    Now A B are connected, B C are not connected, A C are not connected  
    Then you connect B, C  
    Now A B are connected, B C are connected, A C are also connected (although indirectly).  

### Generalization (Abstract Model) - Connectivity problem:
    Given a set of objects (abstracted to a list of integers 0, 1, 2, …, n-1, without loss of generality)  
    You can connect two of them using void union(int a, int b) 
    You can query whether two of them are connected or not using boolean find(int a, int b)  

## Quick Union Find:
### <ins>**Key idea:**</ins>
    - Maintain an array int[] ids of size n, where ids[i] is the label of the i-th object
    - All the connected objects have the same label    
int array: id[i]  
For example:

Given objects 0, 1, 2, 3, 4, 5  
id[] = {0, 1, 2, 3, 4, 5}  

API: union(a, b) -> merge b into a  

union(0, 1)  
-> id[] = {0, 0, 2, 3, 4, 5}  

union(2, 0)  
-> id[] = {2, 0, 2, 3, 4, 5}  

You can connect two of them using `void union(int a, int b) ` 

query:  
```java
findMyClassNumber(int x) {  
	// sanity check  
	return id[x];  
} 
```
union(2, 0) -> merge ALL class 0 into class 2  
Requirements:  
no tree or other data structure underlying  
query is straight forward by returning the index value.  
-> id[] = {2, 2, 2, 3, 4, 5}  
-> id[] = {2, 0, 2, 3, 4, 5}  

union: O(n) -> search the whole array and find the same ids to union  
find: O(1) -> findMyClassNumber   

id[] = {2, 2, 2, 3, 4, 5}  
union(3, 0)  
index3 = 3  
index0 = 2 
-> id[] = {3,3,3,3,4,5}  



```java

class QuickUnionFind {
    private int[] ids;
    public QuickUnionFind(int n) {
	ids = new int[n];
	for (int i = 0; i < n; i++) {
	    ids[i] = i;
	}
    }

    public void union(int a, int b) {
	//merge B to A
        int idA = id[a];
        int idB = id[b];
	for (int i = 0; i < id.length; i++) {
	    if (id[i] == idB) {
	        id[i] = idA;
            }
	}
    }
    public boolean find(int a, int b) {
        return ids[a] == ids[b];
    }
}

```
#### <ins>**Complexity:**</ins>
    n := number of objects
    union: O(n)
    find: O(1)
    space: O(n)



## Tree Union Find:
### <ins>**Key idea:**</ins>
    - An int[] ids array of size n, whether ids[i] is i's parent
    - i and i's parent are connected
    - Root of i is ids[ids[ids[...ids[i]...]]].


ids = [0 1 2 3 4 5 6 7 8 9]  
union(3, 4) union(4, 9) union(8, 0) union(2, 3) union(5, 6) 
union(5, 9) union(7, 3) union(4, 8) union(6, 1) 

union(3, 4)  
<pre>
0 1 2 4 5 6 7 8 9  
      | 
      3
</pre>
ids = [0 1 2 **4** 4 5 6 7 8 9]

union(4, 9)  
<pre>
0 1 2 9 5 6 7 8  
      | 
      4
      | 
      3
</pre>
ids = [0 1 2 4 **9** 5 6 7 8 9]

union(8, 0)  
<pre>
0 1 2 9 5 6 7  
|     | 
8     4
      | 
      3
</pre>
ids = [0 1 2 4 9 5 6 7 **0** 9]

union(2, 3)  
<pre>
0 1 9 5 6 7  
|  / \ 
8  2 4
     | 
     3
</pre>
ids = [0 1 **9** 4 9 5 6 7 0 9]

union(5, 6)  
<pre>
0 1 9  6 7  
|  / \ |
8  2 4 5
     | 
     3
</pre>
ids = [0 1 9 4 9 **6** 6 7 0 9]

union(5, 9)  
<pre>
0 1  9   7  
|   / \ \
8   2 4 6
      | |
      3 5
</pre>
ids = [0 1 9 4 9 6 **9** 7 0 9]

union(7, 3)  
<pre>
0 1    9     
|   / / \ \
8   2 4 6 7
      | |
      3 5
</pre>
ids = [0 1 9 4 9 6 9 **9** 0 9]

union(4, 8)  
<pre>
   0
  / \
 8   9       1
  / / \ \
  2 4 6 7
      | |
      3 5
</pre>
ids = [0 1 9 4 9 6 9 9 0 **0**]

union(6, 1)  
<pre>
   1
   |
   0
  / \
 8   9       
  / / \ \
  2 4 6 7
      | |
      3 5
</pre>
ids = [**1** 1 9 4 9 6 9 9 0 0]

```java
class TreeUnionFind{
    private int[] ids;
    public TreeUnionFind(int n) {
        ids = new int[n];
        for (int i = 0; i < n; i++) {
            ids[i] = i;
        }
    }
    
    public int root(int a) {
        int root = a;
        while (ids[root] != root) {
            root = ids[root];
        }
        return root;
    }
    
    public boolean find(int a, int b) {
        return root(a) == root(b);
    }
    
    public void union(int a, int b) {
        int rootA = root(a);
        int rootB = root(b);
        ids[rootA] = rootB;
    }

}
```

#### <ins>**Complexity:**</ins>
    n := number of objects
    in the worst case, tree can be very unbalanced
    union: O(n)
    find: O(n)
    space: O(n)

#### Quick Union Find defect:
* Union too expensive (N array accesses)
* Trees are flat, but too expensive to keep them flat

#### Tree Union Find defect:
* Trees can get tall
* Find too expensive(could be N array accesses)

## Weighted Union Find:

ids = [1, 1, 1, 3, 5, 5]
<pre>
 5           1
  \         / \
   6       2   3
                \
                 4
</pre>

union(1, 5)  
ids[1, 1, 1, 3, **1**, 5]
<pre>
            1
          / | \
         5  2  3
        /       \
       6         4
</pre>




### <ins>**Key idea:**</ins>
    - Modify tree union find to avoid tall trees
    - Keep track of size of each tree(number of objects)
    - Balance by linking root of smaller tree to root of larger tree
    
### Data Structure
    Same as Tree Union Find, but maintain an extra array sizes[i] to count the number of objects in the tree rooted at i.

### Find:
    identical to tree union find
    return root(a) == root(b)
    
### Union:
    Link root of smaller tree to root of larger tree
    update the sizes[] array
    

```java
class WeightedUnionFind{
    private int[] ids;
    private int[] sizes;
    public TreeUnionFind(int n) {
        ids = new int[n];
        sizes = new int[n];
        for (int i = 0; i < n; i++) {
            ids[i] = i;
            sizes[i] = 1;
        }
    }
    
    public int root(int a) {
        int root = a;
        while (ids[root] != root) {
            root = ids[root];
        }
        return root;
    }
    
    public boolean find(int a, int b) {
        return root(a) == root(b);
    }
    
    public void union(int a, int b) {
        int rootA = root(a);
        int rootB = root(b);
        if (sizes[rootA] >= sizes[rootB]) {
            ids[rootB] = rootA;
            sizes[rootA] += sizes[rootB];
        } else {
            ids[rootA] = rootB;
            sizes[rootB] += sizes[rootA];
        }
        
    }

}
```

#### <ins>**Complexity:**</ins>
    Assume maximum height of tree is O(logn)
    union: O(logn)
    find: O(logn)
    space: O(n)

Proof that height is O(log n):
-> need to at least loosely prove and estimate it as O(logn)

By induction
Denote, h(n) is the height of a tree with n nodes;

Hypothesis:
h(n) <= log_2(n) + 1;

initially:
h(1) = 1 <= log_2(1) + 1 = 1;

induction:  
Suppose h(m)<=log_2(m)+1, h(k)<=log_2(k)+1. Without loss of generality, also assume m >= k    
Let's prove that h(m+k)<=log_2(m+k)+1(union two trees)  

log_2(k) + 1 = log_2(k) + log_2(2) = log_2(2k)  
h(m + k)  
<= max{h(m), h(k) + 1}  
 = max{log_2(m) + 1, log_2(k) + 2}  
 = max{log_2(2m), log_2(4k)}  
<= max{log_2(2m), log_2(2m + 2k)}  
 = log_2(m + k) + 1  
 
End of Proof.


## Path Compression
<pre>
            1
           /           1
          2           / \
         /           2   3
        3                 \
       /                   4
      4
      
      root(3) 
        
          1
         /            1
        2           / | \
       /           2  4  3
      3                 
     /                   
    4 
      root(4)    
       
</pre>
### <ins>**Key idea:**</ins>
    - In the int root(int a) function, always connect each node in the path from a to root directly

```java
interation:
public int root(int a) {
    int root = a;
    while (ids[root] != root) {
        root = ids[root]; 
    }
    while (a ! = root) {  
        int t = ids[a];   
        ids[a] = root;   
        a = t;
    }
    return root;
}

recursion:
public int root(int a) {
    if (a != ids[a]) {
       ids[a] = root(ids[a]);
    }
    return ids[a];
}

```

	root(4)
	              root
                  /                 \         
               ids[root]       
              /
           ids[ids[root]]
         /
      …..

    ids = {0, 1, 1, 2, 3}  
    root(4):4 != ids[4], return ids[4] (1)  
        ids[4] = root(3) = 1
        root(3):3 != ids[3], return 1
            ids[3] = root(2) = 1
            root(2):2 != ids[2], return 1
                ids[2] = root(1) = 1
                root(1): 1 == ids[1], return 1
    ids = {0, 1, 1, 1, 1}
    
    ids[4] = 1
    ids[3] = 1
    ids[2] = 1
    ids[1] = 1

#### <ins>**Complexity:**</ins>
    The tree is guaranteed to be very flat in this case!
    
    complexity is nearly
    union: O(1)
    find: O(1)
    space: O(n)
