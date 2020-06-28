# **Expression Tree**

###Content
* Convert prefix/postfix expression to infix expression (and reverse)
* Convert infix/postfix/prefix expression into the expression tree
* Evaluate infix/prefix/postfix expression using stacks
* Ternary Expression Tree/Parse
* Expression (Tree) Applications
  
<pre>
                 -
               /   \
              *     6 
            /   \ 
           +     5
         /   \
        3     4

1. Full binary tree
2. n(non-leaf) = n(leaf) - 1
3. non-leaf & leaf
    a. all non-leaf: operator(运算符）
    b. all leaf: operand(运算数）
</pre>

prefix: - * + 3 4 5 6  -- Tip1: right to left scan  
infix: ((3 + 4) * 5) - 6  
postfix: 3 4 + 5 * 6 -  -- Tip2: left to right scan   
