# **Expression Tree**
### Content
>* Convert prefix/postfix expression to infix expression (and reverse)
>* Convert infix/postfix/prefix expression into the expression tree
>* Evaluate infix/prefix/postfix expression using stacks
>* Ternary Expression Tree/Parse
>* Expression (Tree) Applications
  
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


### Evaluate Postfix(i.e. Reverse Polish Notation, RPN)
Evaluate the value of an arithmetic expression in Reverse Polish Notation.  
Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Note:
Division between two integers should truncate toward zero.
The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

Example 1:  
Input: ["2", "1", "+", "3", "*"]  
Output: 9  
Explanation: ((2 + 1) * 3) = 9
<pre>
           *
         /   \
        +     3
      /   \
     2     1 
</pre>

Example 2:  
Input: ["4", "13", "5", "/", "+"]  
Output: 6  
Explanation: (4 + (13 / 5)) = 6
<pre>        
        +
      /   \
     4     /
         /   \
        13    5 
</pre>

left->right scan  
Tip3:  
If it's number/symbol, just push to stack
If it's operator, you need to do some compare(precedence) and calculate

```java
    public int evaluateRPN(String[] tokens) {
        //sanity check, null, length, format
        Deque<Integer> stack = new ArrayDeque<>();
        
        for (String token: tokens) {
            //check if it is operator
            if ("+-*/".indexOf(token) != -1) {
                int n1 = stack.pop();
                int n2 = stack.pop();
                switch(token.charAt(0)) {
                    case '+':
                        stack.push(n1 + n2);
                        break;
                    case '-':
                        stack.push(n2 - n1);
                        break;
                    case '*':
                        stack.push(n1 * n2);
                        break;
                    case '/':
                        stack.push(n2 / n1); //check n1 != 0
                        break;
                }
            } else {
                stack.push(Integer.valueOf(token));
            }
        }
        return stack.peek();
    }
```
