## 1. [Not all but sometimes all]

[1]: [http://www.codewars.com/kata/564ab935de55a747d7000040/train/python]

Write

    remove(text, what)

that takes in a string str(text in Python) and an object/hash/dict what and returns a string with the chars removed in what. For example:

    remove('this is a string',{'t':1, 'i':2}) == 'hs s a string'
    # remove from 'this is a string' the first 1 't' and the first 2 i's.
    remove('hello world',{'x':5, 'i':2}) == 'hello world'
    # there are no x's or i's, so nothing gets removed
    remove('apples and bananas',{'a':50, 'n':1}) == 'pples d bnns'
    # we don't have 50 a's, so just remove it till we hit end of string.

### check 

def replace(self, old, new, count=-1) 
Inferred type: (self: str, old: str, new: str, count: Integral) -> str   

### solution

```python
def remove(text, what):
    for k, v in what.items():
        text = text.replace(k, '', v)
    return text
```

## 2. [Group in 10s]

[2]: [http://www.codewars.com/kata/5694d22eb15d78fe8d00003a/train/python]

Write a function groupIn10s which takes any number of arguments, and groups them into sets of 10s and sorts each group in ascending order.

The return value should be an array of arrays, so that numbers between 0-9 inclusive are in position 0 and numbers 10-19 are in position 1, etc.

Here's an example of the required output:

    grouped = group_in_10s(8, 12, 38, 3, 17, 19, 25, 35, 50) 

    grouped[0]     # [3, 8]
    grouped[1]     # [12, 17, 19]
    grouped[2]     # [25]
    grouped[3]     # [35, 38]
    grouped[4]     # None
    grouped[5]     # [50]

### [link url][1]

### Check

改题目考察了入参形式： f(arg1, arg2) --> f(x, y) 

f(*args)  传入的是一个元组 f(1, 2, 3, 5)

f(**args) 传入一个Map f(a = 1, b = 2)

### solution

```python

n_10s(*nums):
    if not nums:
        return []
    nums = list(nums)
    nums.sort()
    l = int(nums[-1] // 10 + 1)

    ret = []
    for i in range(l):
        ret.append([])
    for num in nums:
        i = num // 10
        ret[i].append(num)
    for i in range(l):
        if len(ret[i]) == 0:
            ret[i] = None
    return ret
print(group_in_10s(1, 22, 33, 4, 56))
print(group_in_10s())
```


## 3. [Complementary DNA]

[3]: [http://www.codewars.com/kata/554e4a2f232cdd87d9000038/train/python]

Deoxyribonucleic acid (DNA) is a chemical found in the nucleus of cells and carries the "instructions" for the development and functioning of living organisms.

If you want to know more http://en.wikipedia.org/wiki/DNA

In DNA strings, symbols "A" and "T" are complements of each other, as "C" and "G". You have function with one side of the DNA (string, except for Haskell); you need to get the other complementary side. DNA strand is never empty or there is no DNA at all (again, except for Haskell).

    DNA_strand ("ATTGC") # return "TAACG"

    DNA_strand ("GTAT") # return "CATA"

    
### Solution

```python

map = {'T': 'A', 'A': 'T', 'C': 'G', 'G': 'C'}
def DNA_strand(dna):
    return ''.join([map[x] for x in dna])

```


# 4. Unary function chainer

Your task is to write a higher order function for chaining together a list of unary functions. In other words, it should return a function that does a left fold on the given functions.

    chained([a,b,c,d])(input)

Should yield the same result as

    d(c(b(a(input))))

函数式编程  递归

```python
def test():
    return lambda x: x + 2
# 调用
test()(2)
```
# Solution


# 5. [Beginner Series #3 Sum of Numbers][5]

[5]: [http://www.codewars.com/kata/55f2b110f61eb01779000053/train/python/56962db8d0417b3baf000023]

Given two integers, which can be positive and negative, find the sum of all the numbers between including them too and return it. If both numbers are equal return a or b.

Note! a and b are not ordered!

    Example: 
    get_sum(1, 0) == 1   // 1 + 0 = 1
    get_sum(1, 2) == 3   // 1 + 2 = 3
    get_sum(0, 1) == 1   // 0 + 1 = 1
    get_sum(1, 1) == 1   // 1 Since both are same
    get_sum(-1, 0) == -1 // -1 + 0 = -1
    get_sum(-1, 2) == 2  // -1 + 0 + 1 + 2 = 2

### Check 
考察 sum()函数 以及 max() min() 函数

### Solution

```python

def get_sum(a, b):
    sum = 0
    if a > b:
        a, b = b, a
    for i in range(a, b + 1):
        sum += i
    return sum

def get_sum(a, b):
    return sum(xrange(min(a, b), max(a, b) + 1))

```
