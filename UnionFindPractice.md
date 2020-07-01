## Union Find Practice
### Q1: 
Implement a synonym system, can support operations:  
associate(word1, word2) -- word1作为word2的同义词，同义词是双向的  
isSynonym(word1, word2) -- 检查两个词是否同义词  
getAllsynonym(word1) -- 将所有word1的同义词返回

associate --> union  
isSynonym --> find  
Map<String, String> ids; --> int[] ids  

```java
public class SynonymSystem {
    Map<String, String> ids;
    Map<String, Integer> sizes;
    public SynonymSystem() {
        ids = new HashMap<>();
        sizes = new HashMap<>();
    }

    //recursive
    private String rootR(String w) {
        if (!ids.containsKey(w)) {
            ids.put(w, w);
            sizes.put(w, 1);
            return w;
        }
        if (!ids.get(w).equals(w)) {
            ids.put(w, rootR(ids.get(w)));
        }
        return ids.get(w);
    }

    //iterative
    private String root(String word) {
        if (!ids.containsKey(word)) {
            ids.put(word, word);
            sizes.put(word, 1);
            return word;
        }
        String w = word;
        String root = ids.get(word);
        while (!root.equals(w)) {
            w = root;
            root = ids.get(root);
        }
        w = word;
        while (!root.equals(w)) {
            String tmp = ids.get(w);
            ids.put(w, root);
            w = tmp;
        }
        return root;
    }

    public void associate(String word1, String word2) {
        String root_word1 = root(word1);
        String root_word2 = root(word2);
        if (!root_word1.equals(root_word2)) {
            int size_word1 = sizes.get(word1);
            int size_word2 = sizes.get(word2);
            if (size_word1 < size_word2) {
                ids.put(root_word1, root_word2);
                sizes.put(root_word2, size_word1 + size_word2);
            } else {
                ids.put(root_word2, root_word1);
                sizes.put(root_word1, size_word1 + size_word2);
            }
        }
    }

    public boolean isSynonym(String word1, String word2) {
        return root(word1).equals(root(word2));
    }

    public List<String> getAllSynonym(String word) {
        List<String> res = new ArrayList<>();
        String root_word = root(word);
        for (String key : ids.keySet()) {
            String root = root(key);
            if (root.equals(root_word)) {
                res.add(key);
            }
        }
        return res;
    }
}

```
Time complexity:    
associate: O(1)    
isSynonym: O(1)    
getAllSynonym: O(1)    
 
Space: O(n)    
n is the number of operations or the number of unique words in this system   

### Q2: Number of Islands

DFS:  
if grid is not allowed to modify, use boolean[][] visited  
```java
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int count = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    
    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0') return;
        grid[i][j] = '0';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
```

BFS: 
```java
    private final static int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int m = grid.length;
        int n = grid[0].length;
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    bfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }

    private void bfs(char[][] grid, int x, int y){
        grid[x][y] = '0';
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{x,y});
        while(!q.isEmpty()){
            int size = q.size();
            int[] cur = q.poll();
            for(int i = 0; i < size; i++){
                for(int[] dir: dirs){
                    int x1 = cur[0] + dir[0];
                    int y1 = cur[1] + dir[1];
                    if(x1 >= 0 && y1 >= 0 && x1 < grid.length && y1 <grid[0].length && grid[x1][y1] == '1'){
                        grid[x1][y1] = '0';
                        q.offer(new int[]{x1, y1});
                    }
                }
            }
        }
    }
```

Union Find:
```java
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length, n = grid[0].length;
        UnionFind uf = new UnionFind(m * n);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0') continue;
                uf.count++;
                int id = i * n + j;
                //left: j != 0, up: i != 0
                if (j > 0 && grid[i][j -1] == '1') uf.union(id, id - 1);
                if (i > 0 && grid[i - 1][j] == '1') uf.union(id, id - n);
            }
        }
        return uf.count;
        
    }
    
    static class UnionFind {
        private int[] ids;
        private int[] sizes;
        public int count;

        public UnionFind(int n){
            //check
            ids = new int[n];
            sizes = new int[n];
            for (int i = 0; i < n; i++) {
                ids[i] = i;
                sizes[i] = 1;
            }
            count = 0;
        }

        public int root(int a) {
            int id = a;
            while (ids[id] != id) {
                ids[id] = ids[ids[id]];//path compression
                id = ids[id];
            }
            return id;
        }

        public boolean find(int a, int b) {
            return root(a) == root(b);
        }

        public void union(int a, int b) {
            int rootA = root(a);
            int rootB = root(b);
            if (rootA != rootB) count--;
            else return;
            if (sizes[rootA] >= sizes[rootB]) {
                //merge rootB to rootA
                ids[rootB] = rootA;
                sizes[rootA] += sizes[rootB];
            } else {
                //merge rootA to rootB
                ids[rootB] = rootA;
                sizes[rootB]  += sizes[rootA];
            }
        }
    }   
``` 
