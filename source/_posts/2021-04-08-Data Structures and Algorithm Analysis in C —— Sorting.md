---
layout: post
title: Data Structures and Algorithm Analysis in C —— Sorting
date: 2021-04-08 09:11:05
categories: [Study Notes]
tag: [Data Structures, Algorithm, C/C++]

comments: true
---

**Start trying to write a blog in English, so start now!**

# Insertion sort

One of the simplest sorting algorithms is insertion sort. Insertion sorting consists of N-1 pass sorting.  For P=1 to P=N-1, insertion sort ensures that the elements from position 0 to position P are sorted. Insertion sort takes advantage of the fact that the elements from position 0 to position P-1 are already sorted.

**Insertion sort routine:**

```C
void InsertionSort(ElementType A[], int N){
    int j,P;
    
    ElementType Tmp;
    for(P=1; P < N; P++){ //1
        Tmp = A[P];		//2	
        for(j=P; j>0 && A[j-1]>Tmp; j--) //3
            A[j] = A[j-1]; /*4 Move the pervious element to position of the next element, and exit the loop when the pervious element is not greater than Tmp*/
        A[j] = Tmp; //5 Insert Tmp here.
    }
}
```

Lines 2 to 5 implement data movement without obvious use of exchange. Store the element at position P in **Tmp**, and (before position P) all lager elements move one position to the right. Place the **Tmp** in the correct position after you let it go. This method is the same as the technique used when implementing a binary head.