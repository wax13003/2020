# **Union Find**
## **Background**
### Computer Connection Problem:

    Given many computers, and that you can connect two computers anytime, and that you want to know whether two computers are connected (directly or indirectly) anytime. What data structure and algorithm can you use?

### Example:

    You have three computers A, B, C  
    At first, A B are not connected, B C are not connected, A C are not connected  
    Then you connect **A, B**  
    Now A B are connected, B C are not connected, A C are not connected  
    Then you connect **B, C**  
    Now A B are connected, B C are connected, A C are also connected (although indirectly).  

### Generalization (Abstract Model) - Connectivity problem:
    Given a set of objects (abstracted to a list of integers 0, 1, 2, …, n-1, without loss of generality)  
    You can connect two of them using `void union(int a, int b) ` 
    You can query whether two of them are connected or not using `boolean find(int a, int b)`  

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
