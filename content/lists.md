---
title: "Working with lists"
menu: main
weight: 4
---

At the end of the last lesson, we saw noticed that `sys.argv` gave us a new datastructure:
a list.
A list is a set of objects enclosed by a set of square brackets (`[]`).

```
example = [1, 2, 4, 5]
example
```
```
[1, 2, 4, 5]
```

Note that a list can hold any type of item, even other lists!

```
example = [1, True, None, ["word", 123], "test"]
example
```
```
[1, True, None, ['word', 123], 'test']
```

We can get different pieces of a list via indexing.
We add a set of square brackets after the list in question along with the index of the values
we want.
Note that in Python, all indices start from 0 - the first element is actually the 0th element 
(this is different from languages like R or Matlab).
The best way to think about array indices is that they are the number of offsets from the first position - 
the first element does not require an offset to get to.

![](https://imgs.xkcd.com/comics/donald_knuth.png)

And a few examples of this in action:

```
# first element
example[0]
# second element
example[1]
# fetch the list inside the list
example[3]
```
```
1
True
['word', 123]
```

Note that we can index a range using the colon (`:`) operator.
A colon by itself means fetch everything.

```
example[:]
```
```
[1, True, None, ['word', 123], 'test']
```

A colon on the right side of an index means everything after the specified index.

```
example[2:]
```
```
[None, ['word', 123], 'test']
```

A colon on the left side  of an index means everyting before, but not including, the index.

```
example[:2]
```
```
[1, True]
```

And if we use a negative index, it means get elements from the end,
going backwards.

```
# last element
example[-1]
# everything except the last two elements
example[:-2]
```
```
'test'
[1, True, None]
```

Note that we can use the index multiple times to retrieve information from nested objects.

```
example[3][0]
```
```
'word'
```

If we index out of range, it is an error:

```
example[5]
```
```
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-12-98429cb6526b> in <module>()
----> 1 example[5]

IndexError: list index out of range
```

We can also add two lists together to create a larger list.

```
[45, 2] + [3]
```
```
[45, 2, 3]
```

## Lists as objects

Like other objects in Python, lists have a unique behavior that can catch a lot of people off guard.
What happens when we run the following code?

```
list1 = [1, 2, 3, 4]
list2 = list1
list2 += [5, 6, 7]
print('List 2 is: ', list2)
print('List 1 is: ', list1)
```
```
print('List 2 is: ' + list2)                                                                  
print('List 1 is: ' + list1)  
```

Modifying `list2` actually modified `list1` as well.
In Python, lists are objects.
Objects are not copied when we assign them to a new value (like in R).
This is an important optimization, 
as we won't accidentally fill up all of our computer's memory by renaming a variable a couple of times.
When we ran `list2 == list1`, it just created a new name for `list2`.
`list2` still points at the same underlying object.

We can verify this with the `id()` function.
`id()` prints an objects unique identifier.
Two objects will not have the same ID unless they are the same object.

```
id(list1)
id(list2)
```
```
140319556870408
140319556870408
```

In order to create `list2` as a unique copy of `list1`. 
We have to use the `.copy()` method.

```
list1 = [1, 2, 3, 4]                                                                          
list2 = list1.copy()                                                                          list2 += [5, 6, 7]                                                                            
print('List 2 is: ', list2)                                                                   
print('List 1 is: ', list1)
id(list2)
id(list1)
```
```
List 2 is:  [1, 2, 3, 4, 5, 6, 7]
List 1 is:  [1, 2, 3, 4]
140319554648072
140319554461896
```

`.copy()` is a method. 
Methods are special functions associated with an object and define what it can do.
They always follow the syntax `object.method(arg1, arg2=some_value)`.

Other frequently used methods of lists include `.append()`:

```
list1.append(77)
[1, 2, 3, 4, 77]
# this adds a one-element list
list1.append([88])
[1, 2, 3, 4, 77, [88]]
```

And `.extend()` (combines two lists, instead of adding the second list as an element):

```
list1.extend([99, 88, 101])
```
```
[1, 2, 3, 4, 77, [88], 99, 88, 101]
```

And of course, `.remove()` and `.clear()` (both do exactly what you think they should do):

```
list1.remove([88])
print(list1)
list1.clear()
print(list1)
```
```
[1, 2, 3, 4, 77, 99, 88, 101]
[]
```

{{<admonition title="Dynamic resizing of lists">}}
Python's lists are an extremely optimized data structure.
Unlike R's vectors, there is no time penalty to continuously adding elements to list.
You never need to preallocate a list at a certain size for performance reasons.
{{</admonition>}}

## Iterating through lists

We'll very frequently want to iterate over lists and perform an operation with every element.
We do this using a for loop.

A for loop generally looks like the following:

```
for variable in things_to_iterate_over:
	do_stuff_with(variable)
```

An example of an actually functioning for loop is shown below:

```
for i in range(10):
	print(i)
```
```
0
1
2
3
4
5
6
7
8
9
```

In this case we are iterating over the values provided by `range()`.
`range()` is a special generator function we can use to provide.


