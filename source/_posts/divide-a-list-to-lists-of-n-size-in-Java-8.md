---
title: Divide a list to lists of n size in Java 8
date: 2017-09-09 10:19:29
tags:
    - java
    - java-stream
categories: 
    - How to
excerpt: You have probably faced a few times a situation where you had to divide n-size list to lists of m-size. 
cover: /img/post-bg-1.jpg
---
You have probably faced a few times a situation where you had to divide n-size list to lists of m-size. Something like:

```java
[1,2,3,4,5,6,7] -> [[1,2], [3,4], [5,6], [7]]
```
    
You won't find a simple method in Java SDK for such operation, although there are some utility methods in 3rd party
libraries, e.g. [`Lists.partition(List list, int size)`](https://google.github.io/guava/releases/22.0/api/docs/com/google/common/collect/Lists.html#partition-java.util.List-int-) 
in Guava or [`ListUtils.partition(List list, int size)`](https://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/ListUtils.html#partition(java.util.List,%20int)) 
in Apache Commons Collections. But what if you don't have these libraries added to your project and you don't want to add 
them just for a single utility method?

### Use Java 8 Stream API

Luckily you can utilize Java 8 Stream API to do same thing:

```java
import java.util.Arrays;
import java.util.Collection;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.stream.Collectors;

final class Java8StreamPartitionExample {

    public static void main(String[] args) {
        final List<Integer> list = Arrays.asList(1,2,3,4,5,6,7);

        System.out.println(partition(list, 2));  // [[1, 2], [3, 4], [5, 6], [7]]
        System.out.println(partition(list, 3));  // [[1, 2, 3], [4, 5, 6], [7]]
    }

    private static  <T> Collection<List<T>> partition(List<T> list, int size) {
        final AtomicInteger counter = new AtomicInteger(0);

        return list.stream()
                .collect(Collectors.groupingBy(it -> counter.getAndIncrement() / size))
                .values();
    }
}
```
