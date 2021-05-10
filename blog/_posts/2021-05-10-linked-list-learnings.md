---
layout: post
title: The many learnings of a seemingly straightforward LinkedList problem
category: blog
---

Here's a seemingly simple LinkedList problem that utilizes the fast and slow pointer technique and some basic Linked List manipulation . But going down the rabbit hole of understanding every component of this problem proved to be more than I expected.

Here's the problem: Given a linked list, take the second half of the list (if it has an odd number of nodes, then take the second of the middle nodes), reverse it, and then interweave those nodes with the first half of the list.

Example 1:
Input: a > b > c > 1 > 2 > 3
Output: a > 3 > b > 2 > c > 1

Example 2:
Input: 1 > 3 > 5
Output: 1 > 5 > 3

After some initial analysis, it's clear that the problem can be solved by running three subroutines:
1. Split the list into two lists
2. Reverse the second list
3. Interweave the first with the second list

List reversal and interweaving the two lists are less tricky, so let's get those out of the way.

```javascript
const reverse = function(head) {
  let prev = null;
  
  while (head) {
    next = head.next;
    head.next = prev;
    prev = head;
    head = next;
  }

  return prev;
}
```

One interesting observation I made along the way is the value of the head node once the `reverse` function has ran. Suppose we have a print function in the class `Node` to observe the chnages in nodes.

```javascript
class Node {
  constructor(val, next = null) {
    this.val = val;
    this.next = next;
  }

  print() {
    let temp = this;
    while (temp) {
      console.log(temp.val);
      temp = temp.next;
    }
  }
}
```

If we run the input from Example 1 (a > b > c > 1 > 2 > 3), we observe a change in the `head` node.

```javascript
head.print(); // a > b > c > 1 > 2 > 3
let reversed = reverse(head); // 3 > 2 > 1 > c > b > a
head.print(); // a > null
```

I then realized that when we copy a Linked List, we are not deep cloning the data structure but merely creating a new head with shared nodes, kind of like this.

![alt text](https://i.stack.imgur.com/e1jOQ.png "Linked List")

So when we have initialized fast and slow pointers and assigned them to point to the same nodes as `head`, we are still very much working with the same nodes. So during the reversal process, we move the pointer with the newly initialized nodes, backwards, yet the initial head remains at the head of the original node `a`, except that at end of the reversal process, it's now pointing at `null`.

Prior to reversing the list
```
a > b > c > 1 > 2 > 3 > null
^
head
```

After reversing the list
```
3 > 2 > 1 > c > b > a > null
^                   ^
temp                head
```

Next, we interweave two lists, one representing the first half and the other the second half of the list.

```javascript
const merge = function(l1, l2) {
  while (l2) {
   nextNode = l1.next; // save the next node before redirecting the pointer
   l1.next = l2; // redirect the pointer to the other list
   l1 = nextNode; // move up the current pointer to the saved one above

   nextNode = l2.next; // same as above, save the next pointer before repointing the pointer
   l2.next = l1; // repoint the next pointer to the other list
   l2 = nextNode; // current list needs to jump position to saved pointer above
  }
}
```

Then we come to the first item on the list, which is the method responsible for splitting the original list in two.

1. In order to provide the second half of the list for both odd and even numbered nodes of LL, we set up the `while` loop to stop one node before the middle of the list is reached. The assigned variable to `slow.next` will always return the correct node at the middle of the list regardless of whether the LL is even or odd numbered. 
2. Instead of reassigning `slow = slow.next` and retunring `slow` to the main method as the split second part of the array, here we assign the node to the start of the second half of the list to a new variable. This allows us to move the pointer to the head `slow` to null, which means that the current head now will stop once it reaches the middle of the list. This way, we end up with two "clean" lists representing the first and second halfs of the list for processing later on.

```javascript
const split = function(head) {
  let fast = head, slow = head;
  
  while (fast.next && fast.next.next ) {
    fast = fast.next.next;
    slow = slow.next;
  }

  let secondHalf = slow.next;
  slow.next = null;
  return secondHalf;
}
```

So there they are, the three pieces: 1) splitting the head node, 2) reversing the second half of the list, and 3) interweaving two lists together. The trickiest thing is remembering what assigning new variables to the head node really means. Instead of having a hard-coded bopy of the nodes, we are creating a new head node that's connecting to the existing nodes. This means that while operations to split or reverse a list is taking place, the original head node is stationary, but the pointers can be manipulated and can be placed elsewhere.