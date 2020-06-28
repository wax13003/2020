##Union Find Practice
###Q1: 
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