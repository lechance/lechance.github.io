---
title: Algorithm - Insertion Sort in Java
date: 2016-02-14 13:34:34
tags:
 - algorithm
 - java
 - insertion
 - sort
categories:
 - Algorithm
---

### Insertion Sort

_One of the simplest sorting algorithm is the **insertion sort**. Insertion sort is consists of N-1 **passes**. For pass p=1 through N-1, insertion sort ensures that the elements in positions 0 through p are in sorted order. Insertion sort makes use of the fact that elements in positions 0 through p-1 are already known to be in sorted order._

### Analysis of Insertion Sort

Because of the nested loops, each of which can take N iterations, Insertion sort is $O(N{2})$. Furthermore, this bound is tight, because input in reverse order can be achieve this bound.
<img src="http://latex.codecogs.com/gif.latex?\sum_{i=2}^{N}i=2+3+4+...+N=O(N^{2})$$" />
	On the other hand, if the input is presorted, the running time is $O(N)$, because the test in the inner **for** loop always fails immediately. Indeed, if the input is almost sorted(this term will be more rigorously defined in the next section),  insertion sort will run quickly. Because of this wide variation, it is worth analyzing the average-case behavior of this algorithm. It turns out that the average case is $O(N{2})$ for insertion sort, as well as for variety of other sorting algorithms, as the next section shows.

```java
/**
 * Simple insertion sort
 * @param a an  array of comparable itmes
 */

public static <AnyType extends Comparable<? super AnyType>> void insertionSort(AnyType [] a){
	int j;

	for ( int p = 1; p < a.length; p++ ){
		AnyType tmp = a[ p ];
		for ( j = p; j > 0 && tmp.compareTo( a[j - 1] ) < 0; j-- ){
			a[ j ] = a[ j - 1 ];
		}
		a [ j ] = tmp;
	}
}
```
