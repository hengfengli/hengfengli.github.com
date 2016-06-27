---
layout: post
title: "[HL-1] Basic Data Structures and Algorithms"
categories:
    - algorithm
tags:
    - data structures
    - algorithm
published: true
---

This table comes from page 47 in the book "Cracking the coding interview 
(5th ed.)". 

| Data Structures               | Algorithms                     | Concepts                 |
|:-----------------------------:|:------------------------------:|:------------------------:|
| [Linked Lists](#linkedlists)  | Breadth First Search           | Bit Manipulation         |
| [Binary Trees](#bst)          | Depth First Search             | Singleton Design Pattern |
| Tries                         | [Binary Search](#bs)           | Factory Design Pattern   |
| Stacks                        | Merge Sort                     | Memory (Stack vs. Heap)  |
| Queues                        | Quick Sort                     | Recursion                |
| Vectors/ArrayLists            | [Tree Insert/Find/etc.](#bst)  | Big-O Time               |
| Hash Tables                   |                                |                          |


#### <a name="linkedlists"></a>Linked Lists

The content comes from the Page 142-146 in the 
book "Algorithms 4th" by Sedgewick, Wayne. 

For a singly linked-list: 

| Operations                            | Time Complexity |
| --------------------------------------|:---------------:|
| insert at the beginning               |  O(1)           |
| remove from the beginning             |  O(1)           |
| insert at the end                     |  O(1) or O(n)   |
| remove a given node                   |  O(n)           |
| Insert a new node before a given node |  O(n)           |

Java version: 

~~~java
import java.util.ArrayList;
import java.util.List;

public class LinkedList<Item>
{
    private class Node
    {
        Item item;
        Node next;
    }
    
    private int N;
    private Node first;
    
    public LinkedList() {
        first = null;
        N = 0;
    }
    
    public void insertAtBeginning(Item item)
    {
        // save a link to the list
        Node oldfirst = first;
        // create a new node for the beginning
        first = new Node();
        // set the instance variables in the new node
        first.item = item;
        first.next = oldfirst;
        N++;
    }
    
    public void removeFromBeginning()
    {
        if (first == null) return;
        // remove the first node in a linked list
        first = first.next;
    }
    
    public void insertAtEnd(Item item)
    {
        // Time complexity: O(n)
        Node last = new Node();
        last.item = item;
        if (first == null) { first = last; return; }
        
        Node oldlast = first;
        while (oldlast.next != null)
            oldlast = oldlast.next;
        
        assert oldlast.next == null;
        oldlast.next = last;
    }
    
    public List<Item> array()
    {
        List<Item> items = new ArrayList<Item>();
        
        for (Node x = first; x != null; x = x.next)
            items.add(x.item);
        
        return items;
    }
}
~~~


#### <a name="bst"></a>Binary Search Tree

The content comes from the Page 396-414 in the 
book "Algorithms 4th" by Sedgewick, Wayne. 

Worst case:

| algorithm <br> (data structure)                | search   | insert |
| ------------------------------------------------ |:--------:|:------:|
| sequential search <br> (unordered linked list) |  O(N)    | O(N)   |
| binary search <br> (ordered array)             |  O(lgN)  | O(N)   |
| binary tree search <br> (BST)                  |  O(N)    | O(N)   |

Average case:

| algorithm <br> (data structure)  | search |  insert |  support ordered <br> operations? |
| ------------------------------------------------ |:-----------:|:-----------:|:----------:|
| sequential search <br> (unordered linked list) |  O(N/2)     | O(N)       | no  |
| binary search <br> (ordered array)             |  O(lgN)     | O(N/2)     | yes |
| binary tree search <br> (BST)                  |  O(1.39lgN) | O(1.39lgN) | yes |

Java version: 

~~~java
public class BST<Key extends Comparable<Key>, Value>
{
    private Node root;
    
    private class Node
    {
        private Key key;
        private Value val;
        private Node left, right;
        private int N;
        
        public Node(Key key, Value val, int N)
        {
            this.key = key;
            this.val = val;
            this.N   = N;
        }
    }
    
    public int size()
    {
        return size(root);
    }
    
    private int size(Node x)
    {
        if (x == null) return 0;
        else           return x.N;
    }
    
    public Value search(Key key)
    {
        return search(root, key);
    }
    
    private Value search(Node x, Key key)
    {
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if      (cmp < 0) return search(x.left, key);
        else if (cmp > 0) return search(x.right, key);
        else return x.val;
    }
    
    public void insert(Key key, Value val)
    {
        root = insert(root, key, val);
    }
    
    private Node insert(Node x, Key key, Value val)
    {
        if (x == null) return new Node(key, val, 1);
        int cmp = key.compareTo(x.key);
        if      (cmp < 0) x.left  = insert(x.left, key, val);
        else if (cmp > 0) x.right = insert(x.right, key, val);
        else x.val = val;
        x.N = size(x.left) + size(x.right) + 1;
        return x;
    }
    
    public Key min()
    {
        return min(root).key;
    }
    
    private Node min(Node x)
    {
        if (x.left == null) return x;
        return min(x.left);
    }
    
    public void deleteMin()
    {
        root = deleteMin(root);
    }
    
    private Node deleteMin(Node x)
    {
        if (x.left == null) return x.right;
        x.left = deleteMin(x.left);
        x.N = size(x.left) + size(x.right) + 1;
        return x;
    }
    
    // delete: replace x by its successor (smallest node in the right
    // subtree)
    public void delete(Key key)
    {
        root = delete(root, key);
    }
    
    private Node delete(Node x, Key key)
    {
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if      (cmp < 0) x.left  = delete(x.left, key);
        else if (cmp > 0) x.right = delete(x.right, key);
        else
        {
            if (x.right == null) return x.left;
            if (x.left == null) return x.right;
            Node t = x;
            x = min(t.right);
            x.right = deleteMin(t.right);
            x.left = t.left;
        }
        x.N = size(x.left) + size(x.right) + 1;
        return x;
    }
    
    public static void main(String[] args)
    {
        BST<String, String> bst = new BST<String, String>();
        
        bst.insert("S","S");
        bst.insert("E","E");
        bst.insert("X","X");
        bst.insert("A","A");
        bst.insert("C","C");
        bst.insert("R","R");
        bst.insert("H","H");
        
        System.out.println(bst.search("H"));
        System.out.println(bst.size());
        bst.delete("H");
        System.out.println(bst.size());
        System.out.println(bst.search("H"));
    }
}
~~~

#### <a name="bs"></a>Binary Search

The content comes from Page 9 (iterative) and Page 380 (recursive) in
the  book "Algorithms 4th" by Sedgewick, Wayne.  

Java version: 

~~~java
import java.util.Arrays;

public class BinarySearch
{
    public static int searchIterative(int key, int[] a)
    {
        int lo = 0;
        int hi = a.length - 1;
        while (lo <= hi)
        {
            int mid = lo + (hi - lo) / 2;
            if      (key < a[mid]) hi = mid - 1;
            else if (key > a[mid]) lo = mid + 1;
            else                   return mid;
        }
        return -1;
    }
    
    public static int searchRecursive(int key, int[] a)
    {
        return searchRecursiveHelper(key, 0, a.length-1, a);
    }
    
    private static int searchRecursiveHelper(int key, int lo, int hi, int[] a)
    {
        if (hi < lo) return -1;
        int mid = lo + (hi - lo) / 2;
        if      (key < a[mid]) return searchRecursiveHelper(key, lo, mid-1, a);
        else if (key > a[mid]) return searchRecursiveHelper(key, mid+1, hi, a);
        else                   return mid;
    }
    
    public static void main(String[] args)
    {
        int[] whitelist = new int[] {1,5,52,23,123,32,55,78,21,32};
        
        Arrays.sort(whitelist);
        
        System.out.println(searchIterative(1, whitelist));
        System.out.println(searchIterative(55, whitelist));
        System.out.println(searchIterative(32, whitelist));
        System.out.println(searchIterative(11, whitelist));
        
        System.out.println(searchRecursive(1, whitelist));
        System.out.println(searchRecursive(55, whitelist));
        System.out.println(searchRecursive(32, whitelist));        
        System.out.println(searchRecursive(11, whitelist));
    }
}
~~~