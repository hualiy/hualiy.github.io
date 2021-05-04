---
layout: post
title: ArrayList的扩容机制
date: 2021-4-20
categories: [Java]
tags: [ArrayList,Java]
excerpt: 通过源码来分析
---

### Arraylist成员变量 
```java
//序列化ID
private static final long serialVersionUID = 8683452581122892189L;

//数组的默认初始容量
private static final int DEFAULT_CAPACITY = 10;

//共用的空数组实例对象被用作空实例
private static final Object[] EMPTY_ELEMENTDATA = {};

//共用的空数组实例对象被用作默认size的空实例，与EMPTY_ELEMENTDATA 的区别是第一个元素被添加会知道扩容到10
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

//数组元素被存储在数组缓存。ArrayList的容量是array buffer 的长度。
transient Object[] elementData;

//ArrayList 的 size （它包含的元素的个数）
private int size;
```   
<br> 

### 3种new方式
1. 构造一个初始容量为10的空list
```java
public ArrayList()
```
2.构造一个自定初始容量的空list
```java
public ArrayList(int initialCapacity)
```
3. 通过传入集合数组构造一个带元素的list
```java
public ArrayList(Collection<? extends E> c) 
```
<br>  

### add()方法
以add(E e)为例：
在添加e到ArrayList之前，先检查容量
```java
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```
minCapacity=size+1为当前数组要求的最小容量
```java
private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
```
若elementData指向DEFAULTCAPACITY_EMPTY_ELEMENTDATA，说明是无参构造创建的，没有自定初始容量，取minCapacity与DEFAULT_CAPACITY最大值
否则返回mincapacity
```java
 private static int calculateCapacity(Object[] elementData, int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
```  
modCount 是关于线程安全 minCapacity大于数组的长度则扩容
```java
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
``` 
进行扩容，数组长度扩容为原来的1.5倍，若新数组容量比当前要求的最小容量还要小，则新数组容量newCapacity 为minCapacity；
若比最大允许分配的数组长度还大，minCapacity说明内存溢出，三目运算符取其最大值为newCapacity
最后将原来数组的元素拷入新数组，用elementData指向
```java
 private void grow(int minCapacity) {
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```
