---
layout: default
title: "W01 Prepare: Reading"
---

# W01 Prepare: Reading
## Table of Contents
* [Dynamic Arrays](#dynamic-arrays)
    * [Fixed Arrays](#fixed-arrays)
    * [Dynamic Arrays](#dynamic-arrays-1)
    * [Inserting in the Dynamic Array](#inserting-in-the-dynamic-array)
    * [Deleting from the Dynamic Array](#deleting-from-the-dynamic-array)
    * [Lists in C#](#lists-in-c)
    * [Looping Through Lists or Arrays](#looping-through-lists-or-arrays)
* [Lists vs Arrays](#lists-vs-arrays)
* [Problem Solving Strategies](#problem-solving-strategies)
    * [Scientific Method](#scientific-method)
    * [Four Step Process](#four-step-process)
* [Key Terms](#key-terms)

## Dynamic Arrays
### Fixed Arrays

A **fixed array** is a collection of data put in memory with the following properties:

* The fixed array cannot grow or shrink after it is created.
* Each item in the fixed array is the same size
* Each item in the fixed array is contiguous (i.e. right next to each other) in memory

It is very easy for the programming language to access any position in the array. We use the term **index** when referring to a position in the array. The index starts at 0. In the example array below, we will assume that index 0 begins at memory location 100. We will also assume that the size of each element in the array is 4 bytes. Using this information, we can determine the memory location for each index. Generically, we can say that `memory(index) = startingAddress + (index * itemSize)`

{% include image.html url="fixed_array.jpg" description="Shows a fixed array with 8 items (index going from 0 to 7) where each item is 4 bytes long.  Since the starting memory address is 100, the addresses of all items (in order) are 100, 104, 108, 112, 116, 120, 124, and 128." caption="Fixed Array" %}

To create a fixed array in C#, you allocate the array with a size and type, and then add items by index:

```csharp
    var numbers = new int[3];
    numbers[0] = 1;
    numbers[1] = 2;
    numbers[2] = 3;
```

Another syntax that uses a few less characters is a collection initializer:

```csharp
    var numbers = new[] { 1, 2, 3 };
    // or
    int[] numbers = [1, 2, 3];
```

### Dynamic Arrays
The difference between a **dynamic array** and a fixed array is that the dynamic array can grow (and also shrink). This means that we can always add another item to a dynamic array. One of the common operations performed on a dynamic array is to append a new item to the end of the list. Consider the dynamic array below.

{% include image.html url="dynamic_array_not_full.jpg" description="Shows a dynamic array with capacity 8 and size 7.  Indices 0 through 6 are populated with 8, 12, 31, 15, 4, 42, and 27." caption="Dynamic Array" %}

In the dynamic array, the **size** of the array is 7 and the **capacity** is 8. If we append another number to the array, we can use the formula described earlier to get the memory location of the next available index. We will then store the new number into the memory location just calculated.

{% include image.html url="dynamic_array_full.jpg" description="Shows the same array with 19 added into index 7.  The array is now full with capacity and size both equal to 8." caption="Dynamic Array that is Full" %}

In this updated array, both the size and the capacity are now 8. If we want to append another number, we cannot just add it to the end. A dynamic array is really just a fixed size array (in this case a fixed array of size of 8) that we will discard in favor of building a bigger fixed array. If we tried to add another one, then we would be overwriting memory that likely belongs to another variable. This dangerous condition is called buffer **overflow**.

{% include image.html url="dynamic_array_overflow.jpg" description="Shows the same array with 99 attempted to be added after index 7 in memory.  Anything added after index 7 is considered overflow and is memory that belongs to another variable." caption="Overflowing a Dynamic Array" %}

To properly grow the array, we have to complete the following steps:

* Create a new array twice as big as the original array. Usually a dynamic array starts with capacity 0 and then increases in the following pattern: 0, 1, 2, 4, 8, 16, 32, 64, 128, etc.
* Copy all the values from the original smaller array into the new larger array.
* Delete the original smaller array.
* Add the new item to the larger array.

{% include image.html url="dynamic_array_grow.jpg" description="Shows the previous full array.  A new array with double the capacity (16) is created.  ALl the values from Indices 0 to 7 from the original array are copied to the new array.  The new value 99 is then added to index 8 of the new array." caption="Growing a Dynamic Array" %}

### Inserting in the Dynamic Array
If we wanted to insert an item somewhere besides the end of the dynamic array, we would need to be careful to maintain the order of the items in the array. In the array below, if we insert a value at the beginning of the array, all other items will need to move to the next index (i.e. move to the right).

{% include image.html url="dynamic_array_insert.jpg" description="Shows the previous array of capacity 16 and size 9.  The number 20 is inserted into index 0 which causes the previous values in Indices 0 to 8 to move over by one to the right (towards the end)." caption="Inserting into a Dynamic Array" %}

### Deleting from the Dynamic Array
If we wanted to delete an item from the array, we would need to move all items after the item removed to the previous index (i.e. move to the left). As a special case, if we wanted to remove the last item in the array, we would not need to move any other items. The typical effect of removing the last item is to just decrease the size variable by 1 while leaving the capacity the same. This means that the data is not really "deleted." If we append a new item, then the previously "deleted" item will be overwritten.

{% include image.html url="dynamic_array_delete.jpg" description="Shows the previous array of capacity 16 and size 10.  The number 20 is removed from index 0 which causes the previous values in Indices 1 to 9 to move over by one to the left (towards the beginning)." caption="Deleting from a Dynamic Array" %}

### Lists in C#
In C#, a dynamic array is created by using a **List** object. There are several ways to create lists in C#. Here's the most basic way to create a list of integers and add 1, 2, & 3:

```csharp
    var numbers = new List<int>();
    numbers.Add(1);
    numbers.Add(2);
    numbers.Add(3);
```

Another syntax that is a little less characters is to use a collection initializer:

```csharp
    var numbers = new List<int> { 1, 2, 3 };
```

The following table shows the most common operations in a dynamic array and the equivalent C# List code:

| Common Dynamic Array Operation |	Description	 | C# Code                    |
|:-------------------------------|:--------------|:-------------------------------|
| lookup(index) | Gets the value at the specific index.	| `value = myList[index]` |
| append(value)	| Adds "value" to the next available index.	| `myList.Add(value)` |
| insert(index, value)	| Adds "value" to the specified index and moves subsequent items to the next index.	| `myList.Insert(index, value)` |
| remove(index)	| Removes the item at the specified index and moves subsequent items to the previous index.	| `myList.Remove(index)` |
| size() | Return the size of the dynamic array.	| `myList.Count` |
| capacity() | Return the capacity of the dynamic array.	| `myList.Capacity` |
| empty()	| Returns true if the length of the dynamic array is zero.	| `myList.Count == 0` |

### Looping Through Lists or Arrays
To access, display, or do something with every item in a list, the `foreach` loop is used. The following code will display all values in a list one at a time:

```csharp
foreach(var item in myList)
{
    Console.WriteLine(item);
}
```

This type of for loop is called an iterator because it will visit all items in the collection without the programmer needing to think about the index. We can change the for loop so that it loops through all the index values for the list instead.

```csharp
// This is for a list
for (var index = 0; index < myList.Count; ++index)
{
	Console.WriteLine(myList[index]);
}
// This is for an array
for (var index = 0; index < myArray.Length; ++index)
{
	Console.WriteLine(myArray[index]);
}
```

## Lists vs Arrays
Dynamic arrays seem like they are so much more useful, so is there ever a reason to use a fixed array? Of course!

#### Pros for Dynamic Arrays
* Expandable

#### Pros for Fixed Arrays
* Faster memory allocation if you know how much to store
    * Dynamic arrays need to expand and copy the contents once they experience overflow
* Consumes slightly less memory as there is less metadata in an array

## Problem Solving Strategies
### Scientific Method
It can be daunting to try to solve a problem (especially if you are constrained by time in an interview). While it can be tempting to either dive into code or dive into the internet to find help, we want to develop engineering discipline in our work. When we learned about science, we were introduced to the scientific method.

{% include image.html url="scientific_method.jpg" description="Shows a flow chart representing the scientific method.  The process goes: Problem Statement, Research, Hypothesis, Experiment, and then Conclusions.  If the conclusions suggest that the problem is solved, then we can finish knowing we have the answer.  If the conclusions suggest that the problem is not solved, then we return back up to the Research step." caption="Scientific Method" %}


### Four Step Process
When we go directly to writing code we are short-circuiting the process. When we start out the process by researching online but use the answers without experimenting with those ideas, we are also short-circuiting the process. For the purposes of solving problems with software, consider the following four steps that align well with the scientific method:

* Understand the Problem - Restate the problem in your own words. Ask questions to clarify the problem.
* Plan and Design the Solution - Brainstorm ideas and write them down. Try to solve the problem manually on paper. Consider if a data structure (e.g. the dynamic array) could be used to solve the problem. Write down a design (e.g. flow charts, block diagrams, pseudo-code, step-by-step process, etc).
* Implement the Solution - Using your design, implement the solution in a programming language.
* Evaluate the Solution - Test your solution on several example inputs. Identify which scenarios are working and which ones are not. If it's working, ask yourself if it could be done better (e.g. faster, with less code, with fewer variables, etc). If the solution still needs work, return back to the first step again.

{% include image.html url="problem_solving.jpg" description="Shows a flow chart representing how to solve code problems.  The process goes: Understand the Problem, Plan & Design the Solution, Implement the Solution, and Evaluate the Solution.  If the solution is not complete, then the process goes back to Understand the Problem." caption="Four Step Process to Problem Solving" %}

## Key Terms
<dl>
<dt>capacity</dt>
<dd>The number of items that can fit in the fixed array or the number of items that can currently fit in the dynamic array. The capacity of a dynamic array can be increased. This should not be confused with the size of the array.</dd>
<dt>collection</dt>
<dd>Data that can be put in a data structure including a dynamic array.</dd>
<dt>condition</dt>
<dd>In programming, this is a boolean expression that results in True or False. A condition is usually used with an if, while, or list comprehension in Python.</dd>
<dt>dynamic array</dt>
<dd>An array that can grow. A dynamic array is a fixed array that is replaced with a new fixed array if more space is needed.</dd>
<dt>expression</dt>
<dd>In programming, this is a statement that evaluates to a result. Common expressions include mathematical operators (e.g. +, -, *, /) and variables.</dd>
<dt>index</dt>
<dd>In an array, the index represents the location of data. The first index in the array is 0. The index also refers to offset in memory from the starting address.</dd>
<dt>list</dt>
<dd>In Python, a dynamic array is called a list.</dd>
<dt>overflow</dt>
<dd>Occurs when something goes outside of the specified bounds. This occurs with arrays when access occurs outside of the size of the array.</dd>
<dt>size</dt>
<dd>The number of items that are currently in a fixed or dynamic array. If the array is empty, then the size is 0. The size cannot exceed the capacity.</dd>
</dl>
