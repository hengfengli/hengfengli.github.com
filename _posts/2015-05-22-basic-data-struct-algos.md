---
layout: post
title: "Basic Data Structures and Algorithms"
categories: [programming]
tags: [algorithm]
published: true
---

This table comes from page 47 in the book "Cracking the coding interview 
(5th ed.)". 

| Data Structures               | Algorithms             | Concepts                 |
|:-----------------------------:|:----------------------:|:------------------------:|
| [Linked Lists](#linkedlists)  | Breadth First Search   | Bit Manipulation         |
| Binary Trees                  | Depth First Search     | Singleton Design Pattern |
| Tries                         | Binary Search          | Factory Design Pattern   |
| Stacks                        | Merge Sort             | Memory (Stack vs. Heap)  |
| Queues                        | Quick Sort             | Recursion                |
| Vectors/ArrayLists            | Tree Insert/Find/etc.  | Big-O Time               |
| Hash Tables                   |                        |                          |


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

{% highlight java %}
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
{% endhighlight %}