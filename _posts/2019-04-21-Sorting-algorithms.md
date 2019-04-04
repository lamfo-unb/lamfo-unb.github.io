---
layout: post
title: Sorting Algorithms
lang: en
header-img: img/Sorting-algorithms/header-balance-2686214_1920.jpg
date: 2019-04-21 23:59:07
tags: [sorting,algorithms,computer-science]
author: Leonardo Galler and Matteo Kimura
comments: true
---
# What are Sorting Algorithms?

Sorting algorithms are ways to organize an array of items from smallest to largest. These algorithms can be used to organize messy data and make it easier to use. Furthermore, having an understanding of these algorithms and how they work is fundamental for a strong understanding of Computer Science which is becoming more and more critical in a world of premade packages. This blog focuses on the speed, uses, advantages, and disadvantages of specific Sorting Algorithms.

Although there is a wide variety of sorting algorithms, this blog explains Straight Insertion, Shell Sort, Bubble Sort, Quick Sort, Selection Sort, and Heap Sort. The first two algorithms (Straight Insertion and Shell Sort) sort arrays with insertion, which is when elements get inserted into the right place. The next 2 (Bubble Sort and Quick Sort) sort arrays with exchanging which is when elements move around the array. The last two (Heap Sort and Selection Sort)  sort through selection where the right elements are selected as the algorithm moves down the array.

## Straight Insertion Sort
![image alt](/img/Sorting-algorithms/Insertion_sort_animation.gif "Straight Insertion")

Straight insertion sort is one of the most basic sorting algorithms that essentially inserts an element into the right position of an already sorted list. It is usually inserted at the end of a new array and moves down until it finds an element smaller thank itself (the desired position). The process repeats for all the elements in the unsorted array. Consider the array {3,1,2,5,4}, we begin at 3, and since there are no other elements in the sorted array, the sorted array becomes just {3}. Afterward, we insert 1 which is smaller than 3, so it would move in front of 3 making the array {1,3}. This same process is repeated down the line until we get the array {1,2,3,4,5}.

![image alt](/img/Sorting-algorithms/Insertion-sort-example-300px.gif "Straight Insertion")

The advantages of this process are that it is straightforward and easy to implement. Also, it is relatively quick when there are small amounts of elements to sort. It can also turn into binary insertion which is when you compare over longer distances and narrow it down to the right spot instead of comparing against every single element before the right spot. However, a straight insertion sort is usually slow whenever the list becomes large.

## Shell Sort
![image alt](/img/Sorting-algorithms/Sorting_shellsort_anim.gif "Shell Sort")

Shell sort is an insertion sort that first partially sorts its data and then finishes the sort by running an insertion sort algorithm on the entire array. It generally starts by choosing small subsets of the array and sorting those arrays. Afterward, it repeats the same process with larger subsets until it reaches a point where the subset is the array, and the entire thing becomes sorted. The advantage of doing this is that having the array almost entirely sorted helps the final insertion sort achieve or be close to its most efficient scenario.
Furthermore, increasing the size of the subsets is achieved through a decreasing increment term. The increment term essentially chooses every kth element to put into the subset. It starts large, leading to smaller (more spread out) groups, and it becomes smaller until it becomes 1 (all of the array).

![image alt](/img/Sorting-algorithms/Shell_Sort_Algorithm.gif "Shell Sort")

The main advantage of this sorting algorithm is that it is more efficient than a regular insertion sort. Also, there is a variety of different algorithms that seek to optimize shell sort by changing the way the increment decreases since the only restriction is that the last term in the sequence of increments is 1. The most popular is usually Knuth’s method which uses the formula h=((3^k)-1)/2 giving us a sequence of intervals of 1 (k=1),4 (k=2),13 (k=3), and so on. On the other hand, shell sort is not as efficient as other sorting algorithms such as quicksort and merge sort. 

## Bubble Sort
Bubble sort compares adjacent elements of an array and organizes those elements. Its name comes from the fact that large numbers tend to “float” (bubble) to the top. It loops through an array and sees if the number at one position is greater than the number in the following position which would result in the number moving up. This cycle repeats until the algorithm has gone through the array without having to change the order. This method is advantageous because it is simple and works very well for mostly sorted lists.  As a result, programmers can quickly and easily implement this sorting algorithm. However, the tradeoff is that this is one of the slower sorting algorithms.
