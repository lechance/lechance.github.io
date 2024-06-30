---
title: Least-Recently-Used Cache
date: 2016-04-08 18:58:16
categories:
- Algorithm
tags:
- lruCache
- java
- algorithm
---


Implement LRU Cache

> Wikipedia https://en.wikipedia.org/wiki/Cache_algorithms 
>
In computing, cache algorithms (also frequently called cache replacement algorithms or cache replacement policies) are optimizing instructions—​​or algorithms—​​that a computer program or a hardware-maintained structure can follow in order to manage a cache of information stored on the computer. When the cache is full, the algorithm must choose which items to discard to make room for the new ones.


The LRU caching scheme is to remove the least recently used frame when the cache is full and a new page is referenced which is not there in cache.

We use two data structures to implement an LRU Cache.

1. A Queue which is implemented using a doubly linked list. 

2. A Hash with node number as key and value of the corresponding queue node as value.


Note: Initially no data is in the cache.

Below is Java implementation:
```java

import java.util.HashMap;

/**
 *  LruCacheDemo Created by lechance
 */

public class LruCacheDemo {

    public static void main(String[] args) {

        int maxSize = 5;
        LruCache<Integer, Person> lruCache = new LruCache<>(maxSize);

        //entity
        Person p1 = new Person("people 1");
        Person p2 = new Person("people 2");
        Person p3 = new Person("people 3");
        Person p4 = new Person("people 4");
        Person p5 = new Person("people 5");

        Person p6 = new Person("people 6");

        lruCache.put(1, p1);
        lruCache.put(2, p2);
        lruCache.put(3, p3);
        lruCache.put(4, p4);
        lruCache.put(5, p5);

        lruCache.put(6, p6);

        print(lruCache.get(1)); // it will print null because beyond the limit size

        //therefore, the following items can be acquired normally
        print(p2);
        print(p5);
        print(p6);

    }

    //generic method for convenience print test-info
    private static <T> void print(T t) {
        System.out.println(t);
    }

    static class Person {
        String name;

        Person(String name) {
            this.name = name;
        }
        @Override
        public String toString() {
            return "Person: " + name;
        }
    }
}

/**
 * We can keep two separate data structures, A <b>HashMap with (key, pointers)</b> pairs and a <b>
 * doubly linked list</b> which will work as the priority queue for deletion and store the <b>Values</b>
 * , From the <b>HashMap</b>, we can point to an element in the doubly linked list and update it's retrieval
 * time. Because we go directly from the <b>HashMap</b> to the item in the list, our time complexity
 * remains at O(1).
 * <p>
 * For example, our doubly linked list can look like:
 * <p>
 * Least-recently-used-link -> A <-> B <-> C <-> D <-> E -> most-recently-used-link
 * --------------------------------------------------------------------------------
 * left-side most ->                   middle                 <- right-side most
 *
 * @param <K> The type of key
 * @param <V> The type of pointers (doubly linked list)
 */
class LruCache<K, V> {

    private static final Object mLock = new Object();
    private int mCurrentSize;
    private int mMaxSize;
    private HashMap<K, Node<K, V>> mCache;
    private Node<K, V> mLeastRecentlyUsedNode;
    private Node<K, V> mMostRecentlyUsedNode;

    // the list is empty
    public LruCache(int maxSize) {
        this.mMaxSize = maxSize;
        mCurrentSize = 0;
        mCache = new HashMap<>();
        mLeastRecentlyUsedNode = new Node<>(null, null, null, null);
        mMostRecentlyUsedNode = mLeastRecentlyUsedNode;
    }

    public void put(K key, V value) {
        if (mCache.containsKey(key)) {
            return;
        }

        //put the new node at the right-most end of the linked list
        Node<K, V> tempNode = new Node<>(mMostRecentlyUsedNode, null, key, value);
        mMostRecentlyUsedNode.next = tempNode;
        mMostRecentlyUsedNode = tempNode;
        mCache.put(key, tempNode);

        //Delete the left-most entry if items exceed maxSize and update the LRU
        if (mCurrentSize == mMaxSize) {
            mCache.remove(mLeastRecentlyUsedNode.key);
            mLeastRecentlyUsedNode = mLeastRecentlyUsedNode.next;
            mLeastRecentlyUsedNode.previous = null;
        }
        // Update cache size, for the first added entry update the LRU pointer
        else if (mCurrentSize < mMaxSize) {
            if (mCurrentSize == 0) {
                mLeastRecentlyUsedNode = tempNode;
            }
            mCurrentSize++;
        }
    }

    public V get(K key) {

        Node<K, V> tempNode = mCache.get(key);
        if (tempNode == null) {
            return null;
        }
        // if MRU leave the list as it is
        else if (tempNode.key == mMostRecentlyUsedNode.key) {
            return tempNode.value;
        }


        // get the next and previous nodes
        Node<K, V> previousNode = tempNode.previous;
        Node<K, V> nextNode = tempNode.next;

        // If at the left-most(LRU), we update LRU
        if (tempNode.key == mLeastRecentlyUsedNode.key) {
            nextNode.previous = tempNode;
            previousNode.previous = null;
            mLeastRecentlyUsedNode = nextNode;
        }
        // if we are at in middle, we need to update the items before and after
        else if (tempNode.key != mMostRecentlyUsedNode.key) {
            previousNode.next = nextNode;
            //note that
            previousNode.previous = nextNode;
        }

        //finally  move the item to the MRU
        tempNode.previous = mMostRecentlyUsedNode;
        mMostRecentlyUsedNode.next = tempNode;
        mMostRecentlyUsedNode = tempNode;
        mMostRecentlyUsedNode.next = null;

        return tempNode.value;
    }


    /**
     * Define {@code Node} with pointers to the previous and next item and a key, value
     *
     * @param <T> the type of key
     * @param <U> the type of value
     */
    private class Node<T, U> {
        T key;
        U value;
        private Node<T, U> previous;
        private Node<T, U> next;

        Node(Node<T, U> previous, Node<T, U> next, T key, U value) {
            this.previous = previous;
            this.next = next;
            this.key = key;
            this.value = value;
        }
    }
}

```

Origanize post from Network.

[lrucache-wiki]: https://en.wikipedia.org/wiki/Cache_algorithms "Cache_algorithms"

