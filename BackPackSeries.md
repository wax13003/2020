# **BackPack Series**
### DP
>* 最优子结构  
   从小问题的最优解，推导出当前问题的最优解
>* 无后效性  
   即子问题的解一旦确定，就不再改变，不受在这之后、包含它的更大的问题的求解决策影响。
>* 重复子问题  
   子问题的重叠性质，对每一个子问题只计算一次，然后将其计算结果保存在一个表格中，当再次需要计算已经计算过的子问题时，只是在表格中简单地查看一下结果，从而获得较高的效率。

### Series 1 - Max Value/Weight/Height
Given N items with weight wi, i = 0, 1, ...N -1, an integer W denotes the weight of a backpack.
How full you can fill this backpack?  
 
Example 1:  
Input: [2,3,5,7], 11  
Output: 10  

Brute Force:   
对于每一个物品，都有两种选择，放或不放  
<pre>
       N  
    /    \  
  N-1放   N-1不放
 
time complexity: 2^N 
</pre>

DFS + Memorization:  
DFS(i, w) -> DFS(i - 1, w - wi) + wi, DFS(i - 1, w)

DP:  
DP[i][w]: means前i items所能组成的最大weight，并且小于w  

Base case:   
DP[0][w] for w = [0, W]

induction rule:   
case 1 - 放item i	  	→ DP[i - 1][w - wi] + wi  
case 2 - 不放item i	→ DP[i - 1][w]     
				⇒ Max(case1, case2)   
DP[i][w] = max(DP[i - 1][w], DP[i - 1][w - wi] + wi)  
  
Result: 
DP[N][W]  

Time:  O(NW)  
Space: O(NW)  

```java
public int maxValue(int W, int[] weights) {
    if (weights == null || weights.length == 0 || W <= 0) {
        return 0;
    }
    int N = weights.length; 
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = 1; w <= W; w++) {
            dp[i][w] = dp[i - 1][w];
            if (w - weights[i - 1] > 0) {
                dp[i][w] = Math.max(dp[i - 1][w - weights[i - 1]] + weights[i - 1], dp[i - 1][w]);
            } 
        }
    }
    return dp[N][W];
}
```


    DP vs DFS + Memorization
    1. DP可能会用rolling array去优化空间
    2. DFS + Memorization可能会更容易从DFS brute force得到
    3. DFS + Memorization可能会启发DP
    4. DFS + Memorization不一定计算所有小问题
    5. DP避免了recursion

空间优化：
1. 只要保留前一层结果，只要用到两个array.  
    DP[i][w] = max(DP[i - 1][w], DP[i - 1][w - wi] + wi)   
 
进一步优化，用一个array DP[w]:  
<pre>
    for i=1:N:   
        for w=W:0:  
            DP[w] = max(DP[w - wi] + wi, DP[w])  
</pre>

```java
public int maxValue(int W, int[] weights) {
    if (weights == null || weights.length == 0 || W <= 0) {
        return 0;
    }
    int N = weights.length; 
    int[] dp = new int[W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = W; w >= 0; w--) {
            if (w - weights[i - 1] > 0) {
               dp[w] = Math.max(dp[w - weights[i - 1]] + weights[i - 1], dp[w]);
            }
        }
    }
    return dp[W];
}
```
如果题目问最多装多少个物品进背包?
induction rule:   
case 1 - 放item i	  	→ DP[i - 1][w - wi] + 1  
case 2 - 不放item i	→ DP[i - 1][w]     
				⇒ Max(case1, case2)   
DP[i][w] = max(DP[i - 1][w], DP[i - 1][w - wi] + 1)  


## Series 2 - Max value with constraints
Given n items with size Ai and value Vi, and a backpack with size m.
What's the maximum value can you put into the backpack?

Input: 10, A=[2,3,5,7], V=[1,5,2,4]  
Output: 9

DFS:
```java
public void dfs(int[] A, int[] V, int id, int cur_size, int cur_V) {
    if (cur_size < 0) return;
    if (cur_size == 0 || id == A.length) {
        g_max = Math.max(g_max, cur_V);
        return;
    }
    
    g_max = Math.max(g_max, cur_V);
    
    // put this item into backpack
    dfs(A, V, id + 1, cur_size - A[id], cur_V + V[id]);  // -> dp[i - 1][j - A[i]] + V[i]
    // don’t put it
    dfs(A, V, id + 1, cur_size, cur_V);		     // -> dp[i - 1][j]
}

```
DP:   
之前的题目可以认为是这题的特例  
vi = wi  
vi = 1  

DP[i][w]: means前i items所能组成的最大weight，并且小于w  

Base case:   
DP[0][w] for w = [0, W]

induction rule:   
case 1 - 放item i	  	→ DP[i - 1][w - wi] + vi  
case 2 - 不放item i	→ DP[i - 1][w]     
				⇒ Max(case1, case2)   
DP[i][w] = max(DP[i - 1][w], DP[i - 1][w - wi] + vi)  
  
Result: 
DP[N][W]  

Time:  O(NW)  
Space: O(NW)  

如果问最多多少价值的物品能exactly装满背包？  
区别是，constraint不一样，一个是最多，一个需要完全装满。
如果要求完全装满，会出现一种情况，当前i个物品的情况下，无论怎么组合都不能等于w constraint   

-> invalid case    
DP[i][w] means前i个物品，装满w的背包，最多能放多少价值的物品。    

Base case:    
DP[i][0] = 0  
DP[0][w] = -1 for w != 0 ->用一个特殊的值来表示invalid case  

induction rule:  
DP[i - 1][w - wi] 和 DP[i - 1][w] 不一定valid   
case1：放item i，DP[i - 1][w - wi] + vi  
case2：不放item i，DP[i - 1][w]   
需要判断case1和case2是不是valid的case。只能在valid case中选取max  
如果说两个case都是invalid，那么DP[i][w]也就是invalid的case，should be -1。因为当前状态不可能得到  


有多少种方法能放满？
Base case:    
DP[i][0] = 1  
DP[0][w] = 0 for w != 0

induction rule:    
case1：放item i，DP[i - 1][w - wi]  
case2：不放item i，DP[i - 1][w]   
DP[i][w] = DP[i - 1][w - wi] + DP[i - 1][w]  

上面所有DP都可以通过rolling array进行空间优化  

weight可能是负的，value不能为负。满足constraint的情况下，最多能放多少value？  
把所有负的weight的物品找出来，相当于给原始背包扩容，并且把value之和加到结果之中。  

weight可能是负的，value也可能为负的。满足constraint的情况下，最多能放多少value？  
原始的DP解法能不能来解？？？ 不能 
DP[i][w] means能不能
背包容量：原始问题w的有效区间[0, W]    
这一道题w的有效区间：[0, W+|所有负数weight之和|]  


## Series 3 - Max value with two (multiple) constraints
Given n kind of items with size Ai and value Vi (each item has an infinite number available)
and a backpack with size m.  
What's the maximum value can you put into the backpack?

Example 1:  
Input:   
m = 10,  
A = [2,3,5,7],  
V = [1,5,2,4]  

Output: 15  

两种方式：取或不取  
和S2区别在于，去了的物品还可以再取  
<pre>
              第x层，第i个物品。DFS(i, w)
        /不取                          \取这个物品
    第x+1层，第i-1个物品 DFS(i-1,w）  第x+1层，第i个物品 DFS(i,w-wi) + vi
    
DFS(i,w):
    if(i,w)已经计算过了：
        return mem(i, w)
    return max(DFS(i - 1, w), DFS(i, w - wi) + vi)
</pre>

DP[i][w]: means 前i个物品，容量为w, 最大的价值  
DP[i][w] = max(DP[i - 1][w], DP[i][w - wi] + vi)   

base case：  
DP[0][w] = 0  

induction rule：  
case1：不取 DP[i - 1][w]  
case2：取 DP[i][w - wi] + vi  

```java
public int maxValue(int[] weights, int[] vals, int W) {
    //sanity check
    if (weights == null || weights.length == 0 || W <= 0) {
        return 0;
    }
    int N = weights.length; 
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 0; i <= W; i++) {
        dp[i][W] = -1;
    }
    dp[0][0] = 0;
    for (int i = 1; i <= N; i++) {
        for (int w = weights[i]; w <= W; w++) {
            dp[i][w] = dp[i - 1][w];
            if (dp[i][w - weights[i]] != -1) {
                dp[i][w] = Math.max(dp[i][w], dp[i][w - weights[i]] + vals[i]);
            } 
        }
    }
    return dp[N][W];
}
```
空间优化

```java
public int maxValue(int[] weights, int[] vals, int W) {
    //sanity check
    if (weights == null || weights.length == 0 || W <= 0) {
        return 0;
    }
    int N = weights.length; 
    int[]dp = new int[W + 1];
    for (int i = 0; i <= W; i++) {
        dp[i] = -1;
    }
    dp[0] = 0;
    for (int i = 1; i <= N; i++) {
        for (int w = weights[i]; w <= W; w++) {
            if (dp[w - weights[i]] != -1) {
                dp[w] = Math.max(dp[w], dp[w - weights[i]] + vals[i]);
            } 
        }
    }
    return dp[W];
}
```






        
    



