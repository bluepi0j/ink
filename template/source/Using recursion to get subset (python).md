title: Using recursion to get subset (python)
date: 2014-10-13 18:00:00 +0800
author: me
tags:
    - algorithms
    - recursion
preview: Recursion is more like a thinking method rather than an algorithm. We break up the whole thing into the first one and the rest, and the base case is the condition that we can solve directly.

I want to write a function using recursion to get the subset of a set and print like this when we input list [1, 2, 3]as set:

---

Recursion is more like a thinking method rather than an algorithm. We break up the whole thing into the first one and the rest, and the base case is the condition that we can solve directly.

I want to write a function using recursion to get the subset of a set and print like this when we input list [1, 2, 3]as set:

~~~python
[

[1],

[2],

[3],

[1, 2],

[1, 3],

[2, 3],

[1, 2, 3]

]
~~~

We can split the set into two part, the first one, and the rest. Then use the first one to add the every element in the rest list.

**Here is My Code:**

~~~python
def subset(set):
    #Base case: if set is empty set,return himself.
    if len(set) == 0:
        return[set]
    #if set is only has one element, return
    elif len(set) == 1:
        return [[]] + [set]
    #recursive thinking
    else:
        #split the set into the first and rest and get the rest element of set
        rest = subset(set[1:])
        #the list of all subset
        alist = []
        # for every element in the the rest part
        for item in rest:
            #first element in subset add every element in the rest
            blist = [set[0]]
            blist += item
            alist.append(blist)
        #rest is the subset of rest (last subset call)
        return rest + alist
~~~

**Output:**

``` python
>>>subset([1,2,3])
[[], [3], [2], [2, 3], [1], [1, 3], [1, 2], [1, 2, 3]]
```

If we want to change it into our order:

``` python
>>>print(sorted(subset([1, 2, 3]), key=lambda l : int('0' + ''.join(str(i) for i in l))))
[[], [1], [2], [3], [1, 2], [1, 3], [2, 3], [1, 2, 3]]
```
--
```python
def simplesubset(set):
    if not set:
        return [set]
    rest = simplesubset(set[1:])
    return rest + [[set[0]] + item for item in rest]
```

**Note:**

difference between list.append() and [ ]+[ ]

~~~python
>>>[[]]+[[3]+[]]
[[], [3]]
>>>[3]+[]
[3]
>>>list = []
>>>list.append(3)
>>>list
[3]
>>>list.append([3])
>>>list
[3, [3]]
~~~


## Get subset without recursion:

~~~python

def powerset(set):
    alist = [[]]
    for item in set:
        alist += [y + [item] for y in alist]
    return alist
~~~

## Other Resources:

[http://www.mhhe.com/engcs/compsci/sahni/c1/E5.HTM](http://www.mhhe.com/engcs/compsci/sahni/c1/E5.HTM)

[http://stackoverflow.com/questions/26311919/how-to-use-recursion-to-get-subset-by-splitting-the-set-to-first-one-and-rest-pa](http://stackoverflow.com/questions/26311919/how-to-use-recursion-to-get-subset-by-splitting-the-set-to-first-one-and-rest-pa)

[http://blog.csdn.net/qsyzb/article/details/23119529
](http://blog.csdn.net/qsyzb/article/details/23119529)

[http://narutolby.iteye.com/blog/1894901](http://narutolby.iteye.com/blog/1894901)
