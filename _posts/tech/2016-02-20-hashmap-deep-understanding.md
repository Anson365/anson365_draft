---
layout: post
title: 深入理解HashMap
category: 技术
tags: HashMap
description: 深入理解HashMap
---
深入理解HashMap？HashMap不就是一个键值对吗，有什么好深入理解的？
之前从来没有把HashMap放在心上，觉得这种数据结构相对list等其他是使用起来最为方便的一种数据结果，put一个键，返回一个值。但是直到有一天。。。    
```java
Map<String,String> map = new HashMap<String,String>(a.size()*4/3+1) //a.size()返回的是一个整数，a如ArrayList
```    
这段代码是什么意思？HashMap怎么还有这样的构造函数？传个整型进去是为什么？这样构造出来的HashMap有什么好处？

如果你看到标题后有同样的想法？那么我想这篇文章确实值得你好好看一下了！
正如刚才这篇文章就是解决上面的疑问的。

### HashMap数据结构分析
进入HashMap的Class能看到如下部分（为什么length必须是2的倍数呢？）    
```java
    /**
     * An empty table instance to share when the table is not inflated.
     */
    static final Entry<?,?>[] EMPTY_TABLE = {};
    .  .  .
    

    /**
     * The table, resized as necessary. Length MUST Always be a power of two.
     */
    transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;
    .  .  .
    
    
    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;
        .  .  .
    }

```    
由此我们可以看出HashMap由一个table数组组成，这个table数组中包含有Key，Value，Next的一个Entry。
再次抽象一下：
HashTable总体由一个数组组成。而这些数组的每一个元素又是一个单向链表。
于是当我们put数据时，程序根据HashCode索引到具体的数组位置，得到一个Entry 链表，然后放入链表的首位。同理查询、删除也是如此。    
```java
 public V put(K key, V value) {
        if (table == EMPTY_TABLE) {
            inflateTable(threshold);
        }
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key);
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```    

### Hash索引
  大家注意看HashMap的put代码，出现了｀int i = indexFor(hash, table.length);｀ 而之后，利用返回结果取出了table中对应的Entry。
由此我们可知，该方法是根据HashCode的值计算对应的数组中的值。    
```java
    /**
     * Returns index for hash code h.
     */
    static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
        return h & (length-1);
    }

```    
  方法中，传入的HashCode与table的长度有一个按位与的操作，从而计算出table数组中的索引。前面已经提到，操作HashMap中的元素时先获取位置
然后遍历链表，执行相应的操作。遍历链表此时显得尤为的耗时。所以，为了保证HashMap的高效，要求每个键上的链表长度尽可能的小即为1。[table长度为2时
重复概率最小、且空间浪费最少](http://stackoverflow.com/questions/22935616/why-hash-method-in-hashmap)。
我们再来看看构造函数。。。发现HashMap初始化时均需要指定table的长度，未指定的则默认为16。大家会发现HashMap初始化的时候，还有一`loadFactor`的参数
该参数的默认值为0.75f。这个参数有什么用呢？

### HashMap的长度调整
随着HashMap存储数据的增多，由于table[]的定长，索引重复率提高。所以为了提高执行效率，此时需要扩容！    
扩容需要重新数组长度，然后再将现在的数据重新分配进新的数组。想想就知道此时的效率是多么的低下，由此初始化一个合理的长度是多么的重要！
那么什么时候进行扩容操作呢？这时候就要用到上面提到的那个参数了。    
扩容及计算新HashMap的扩容阀值    
```java    
    /**
     * Inflates the table.
     */
    private void inflateTable(int toSize) {
        // Find a power of 2 >= toSize
        int capacity = roundUpToPowerOf2(toSize);

        threshold = (int) Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);
        table = new Entry[capacity];
        initHashSeedAsNeeded(capacity);
    }

```    
当HashMap的size大于threhold时及对HashMap进行扩容操作（代码见*public void putAll(Map<? extends K, ? extends V> m)*
及*void addEntry(int hash, K key, V value, int bucketIndex)*）。threhold的最大值为$2^30+1$

