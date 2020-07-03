## Basic Calculator
### Q1: Basic Calculator
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

Example 1:

Input: "1 + 1"
Output: 2
Example 2:

Input: " 2-1 + 2 "
Output: 3
Example 3:

Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
Note:
You may assume that the given expression is always valid.
Do not use the eval built-in library function. 

```java
    public int calculate(String s) {
        if (s == null || s.length() == 0) return 0;
        Deque<Integer> deque = new ArrayDeque<>();
        int res = 0;
        int sign = 1;
        int num = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                num = num * 10 + c - '0';
            }
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                if (c == '+') {
                    res += sign * num;
                    num = 0;
                    sign = 1;
                } else if (c == '-') {
                    res += sign * num;
                    num = 0;
                    sign = -1;
                } else if (c == '(') {

                    // pushing current result as well as the sign onto the stack
                    deque.addFirst(res);
                    deque.addFirst(sign);
                    sign = 1;
                    res = 0;
                } else if (c == ')') {
                    res += sign * num;
                    num = 0;
                    res *= deque.pollFirst(); //the sign before the parenthesis
                    res += deque.pollFirst(); //now is the result calculated before the parenthesis
                }
            }
        }
        if(num != 0) res += sign * num;
        return res;
    }
```
### Q2: Basic CalculatorII
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

Example 1:

Input: "3+2*2"
Output: 7
Example 2:

Input: " 3/2 "
Output: 1
Example 3:

Input: " 3+5 / 2 "
Output: 5
Note:

You may assume that the given expression is always valid.
Do not use the eval built-in library function.
    
```java
    public int calculate(String s) {
        if (s == null || s.length() == 0) return 0;
        Deque<Integer> deque = new ArrayDeque<>();
        char sign = '+';
        int res = 0;
        int num = 0;
        for (int i = 0; i < s.length(); i++) {
            if (Character.isDigit(s.charAt(i))) {
                num = 10 * num + s.charAt(i) - '0';
            }
            if (!Character.isDigit(s.charAt(i)) && s.charAt(i) != ' ' || i == s.length() - 1) {
                if (sign == '+') {
                    deque.offerFirst(num);
                } else if (sign == '-') {
                    deque.offerFirst(-num);
                } else if (sign == '*') {
                    deque.offerFirst(deque.pollFirst() * num);
                } else if (sign == '/') {
                    deque.offerFirst(deque.pollFirst() / num);
                }
                sign = s.charAt(i);
                num = 0;
            }
        }

        for (int i : deque) {
            res += i;
        }
        return res;
    }
```
### Q3: Basic CalculatorIII
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

The expression string contains only non-negative integers, +, -, *, / operators , open ( and closing parentheses ) and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of [-2147483648, 2147483647].

Some examples:

"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
 

Note: Do not use the eval built-in library function.

```java
    public int calculate(String s) {
        if (s == null || s.length() == 0) return 0;
        Queue<Character> queue = new LinkedList<>();
        for (char c : s.toCharArray()) {
            queue.offer(c);
        }
        return cal(queue);
    }

    private int cal(Queue<Character> queue) {
        Deque<Integer> deque = new ArrayDeque<>();
        int res = 0;
        int num = 0;
        char sign = '+';
        while (!queue.isEmpty()) {
            char c = queue.poll();
            if (Character.isDigit(c)) {
                num = num * 10 + c - '0';
            }
            if (!Character.isDigit(c) && c != ' ' || queue.isEmpty()) {
                if (c == '(') num = cal(queue);
                if (sign == '+') deque.offerFirst(num);
                if (sign == '-') deque.offerFirst(-num);
                if (sign == '*') deque.offerFirst(deque.pollFirst() * num);
                if (sign == '/') deque.offerFirst(deque.pollFirst() / num);
                if (c == ')') break;
                sign = c;
                num = 0;
            }
            
        }
        for (int i : deque) {
            res += i;
        }
        return res;
    }
```



