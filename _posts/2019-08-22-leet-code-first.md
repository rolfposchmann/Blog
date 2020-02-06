---
layout: post
title: Solving Problems with Python on LeetCode
description: Trying out a coding platform for Python 3
---

Solving problems in Python can be a good way to improve your skills in a programming language. There are a lot of websites where you can find such problems but my favourite is [LeetCode](https://leetcode.com/).

It is free, has many problems and an inbuilt compiler so you can directly test your code. It even gives you a ranking on "how good" your code was compared to other people's code which has been submitted.

I scrolled through some problems and found this:

> Given an array `A` of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of `A`.
> 
> You may return any answer array that satisfies this condition.

And here is some example given:

> **Input:** [3,1,2,4]
> 
> **Output:** [2,4,3,1]
> 
> The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

A good way to solve this problem is first to think about a solution in a logical way, not in a programming one.

The first solution which came to my mind was to look for the even numbers, then for the odd numbers, pack them together and return them as a list.

![2loops](/public/images/leet-code-first/2forloops.png)

While this solution works, it is not the best one. You can easily see that you would need to run two for loops. This is still a linear runtime of $n+n=2n$ where $n$ is the cardinality of set $A$: $n=|A|$. For asymptotic analysis where $n\rightarrow \infty$ we ignore constants coefficients in Big-O notation resulting in $\mathcal{O}(n)$.
The mathematical set $A$ is represented in Python with the list `A`.

Note that in Python sets and lists are different. Here is a quick example:

```python
# Sets
a1 = {1, 2, 3, 4}

# Lists
a2 = [1, 2, 3, 4]
```

Unlike lists, sets don't care about order and can change each time the code is executed. Sets also throw away duplicates which is suboptimal as we want to keep all elements. So the way to go here is with Python lists.


Let's imagine you are shopping, standing in front of a shelf with products.

![shelf](/public/images/leet-code-first/supermarket_shelf.jpg)

<sup>Photo by [Franki Chamaki](https://unsplash.com/@franki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)</sup>


What would be the fastest way to take specific products and put them in a sorted basket?
You have two baskets called even and odd.

1. **Go from left to right:** The be efficient, you would go **_one_** time only from left to right (or right to left).
2. **Take the product and put it in the right basket:** Equipped with two baskets you would watch at the product and put it directly in the right basket.
3. **Go home**: Before you can go home, you need to wrap it up into one package. Here you have two options:
	- Pack the items of the two baskets into one new sorted "result" basket
	- Take the even basket and put the odd basket into it.

Here is a schematic of the solution what it would look programming-wise:

![1forloop](/public/images/leet-code-first/1forloop.png)

To code this in Python we first define the function `sortArrayByParity` which takes one list `A` as an argument. Next, we create the two empty "baskets" aka lists for the even and odd numbers.

```python
def sortArrayByParity(A):
	even_list = []
	odd_list = []
```

Then we will iterate through the list `A` and check if it is even or odd. If it is even, we will put it in our even list, if not, then in our odd list. Note that the modulo operation `%` in Python will return `0` if the number is even which means `False`. Therefore I use the *is equal* operation to turn this check into `True`.

```python
# Iterate through list A
for number in A:
	# If the number is even
	if number % 2 == 0:
		# Add it in the "even" list
		even_list.append(number)
	else:
		# Add it in the "odd" list
		odd_list.append(number)
```

We could create a third list now to store our values, but this would be wasting memory. Instead, we add our odd list to our even list. And then we return our even list. It is important to **not** use the append command as it will create a list in a list which is what we don't want.

```python
# Append
print(even_list)
even_list.append(odd_list)
print(even_list)
```

> **Output**:
> 
> [2, 4]
> 
> [2, 4, [3, 1]]

Instead, we want to "extend" our even list with the elements of the odd list.

```python
# Extend
print(even_list)
even_list.extend(odd_list)
print(even_list)
```

> **Output**:
> 
> [2, 4]
> 
> [2, 4, 3, 1]

With this solution, we just needed one for-loop instead of two which results in a time complexity of $\mathcal{O}(n)$.
And as it turns out my solution was quite good:

![result_time](/public/images/leet-code-first/result_time.png)
