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
public int maxValue(int W, int[] items) {
    if (items == null || items.length == 0 || W <= 0) {
        return 0;
    }
    int N = items.length; 
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = 1; w <= W; w++) {
            dp[i][w] = dp[i - 1][w];
            if (w - items[i - 1] > 0) {
                dp[i][w] = Math.max(dp[i - 1][w - items[i - 1]] + items[i - 1], dp[i - 1][w]);
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
public int maxValue(int W, int[] items) {
    if (items == null || items.length == 0 || W <= 0) {
        return 0;
    }
    int N = items.length; 
    int[] dp = new int[W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = W; w >= 0; w--) {
            dp[w] = Math.max(dp[w - items[i - 1]] + items[i - 1], dp[w]);
        }
    }
    return dp[W];
}
```

## Series 2 - Max value with constraints
Given n items with size Ai and value Vi, and a backpack with size m.
What's the maximum value can you put into the backpack?

Input: 10, A=[2,3,5,7], V=[1,5,2,4]  
Output: 9


        
    



