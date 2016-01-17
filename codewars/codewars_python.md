## 1. Not all but sometimes all

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

## 2. Group in 10s

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

[1]: [http://www.codewars.com/kata/5694d22eb15d78fe8d00003a/train/python]


## 3. Complementary DNA

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

