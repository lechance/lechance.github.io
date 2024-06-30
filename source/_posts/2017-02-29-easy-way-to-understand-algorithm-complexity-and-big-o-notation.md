title: Easy way to understand algorithm complexity and big O notation
date: 2017-03-25 12:29:58
comments: false
categories: Algorithm
tags:
- java
- big 0
- time complexity
- space complexity

---
> Big O notation is the most widely used method which describes algorithm complexity-the execution time required or the space used in memory or in disk by an algorithm. Often in exams or intervews, you will be asked some questions about algorithm complexity in the following form.
For an algorithm that uses a data structure of size n, what is the runtime complexity or space complexity of the algorithm? The answer to sunch questions often uses big O notation to describe the alogrithm complexity, such as O(1), O(n), O(n^<sup>2</sup>) or O(log(n)).

#### Big O for time complexity

Here we don't want to discuss big O in a mathematical way. Basically, when analyzing the time complexity for an algorithm, big O notation is used to describe the rough estimae of the number of "steps" to complete the algorithm. Let's take the following example:

```java
void fun(int n) {

    //part 1
    doSomething();
    
    //part 2
    for(int j=0; j < n; j++) {
        doSomething();
    }

    //part 3
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
	        doSomething();
        }
     }

    //part 4
    return;
}
```
Here let's assume that `doSomething` takes C steps to complete. The whole <i>fun(n)</i> method has 4 parts. What is the time complexity of each part for different parameters `n` ?

For part1, it does doSomething() so it takes constant `C` steps. Here `C` is independent to the parameter <i>n</i>.  When the time complexity is independent to the parameter, we use *O(1)* to mark it.

For part2, it does `doSomething` n times. Each time it takes C steps. So in total, it takes `C x n` steps to complete part2. Then as described above, we use *O(1)* to complete the C steps. Then for `C x n`, the complexity becomes *n x O(1)*. Here, an imporant rule is that `a x O(n)` equals O(an). In such case, *n x O(1) = O(n)*. SO the time complexity of part2 is <i>O(n)</i>.

For part3, it has two loops. The inner loop is exactly like part2. The outer loop does part2 n times again so the time complexity for part3 is *n x O(n)* which is O(n^<sup>2</sup>)

#### Rules summary of big O notations

#### How to use big O notation to compare algorithm complexity and why

#### Big O for space cmplexity 

_Refer to network_