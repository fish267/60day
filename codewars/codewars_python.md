
## 1. [Not all but sometimes all](http://www.codewars.com/kata/564ab935de55a747d7000040/train/python)

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

## 2. [Group in 10s](http://www.codewars.com/kata/5694d22eb15d78fe8d00003a/train/python)

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

### Check

改题目考察了入参形式： f(arg1, arg2) --> f(x, y) 

f(*args)  传入的是一个元组 f(1, 2, 3, 5)

f(**args) 传入一个Map f(a = 1, b = 2)

### solution

```python

def group_in_10s(*nums):
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


## 3. [Complementary DNA](http://www.codewars.com/kata/554e4a2f232cdd87d9000038/train/python)

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


# 4. [Unary function chainer](http://www.codewars.com/kata/54ca3e777120b56cb6000710/train/python)

Your task is to write a higher order function for chaining together a list of unary functions. In other words, it should return a function that does a left fold on the given functions.

    chained([a,b,c,d])(input)

Should yield the same result as

    d(c(b(a(input))))

### 调用测试


    def f1(x): return x*2
    def f2(x): return x+2
    def f3(x): return x**2

    def f4(x): return x.split()
    def f5(xs): return [x[::-1].title() for x in xs]
    def f6(xs): return "_".join(xs)


    test.assert_equals( chained([f1,f2,f3])(0), 4 )
    test.assert_equals( chained([f1,f2,f3])(2), 36 )
    test.assert_equals( chained([f3,f2,f1])(2), 12 )

    test.assert_equals( chained([f4,f5,f6])("lorem ipsum dolor"), "Merol_Muspi_Rolod")


# check

函数式编程  递归


返回函数
```python
def outter():
    print('Call function ' + sys._getframe().f_code.co_name)

    def inner():
        print('Call function ' + sys._getframe().f_code.co_name)

    return inner

t = outter()

# t 加上括号，就使用了 inner 函数
t()

### 输出

Call function outter
Call function inner

```

如果里面内容不复杂，使用 lambda 直接写出来

```python
def test():
    return lambda x: x + 2
# 调用
test()(2)
```

本题目，也可以用 reduce 高阶函数，入参是一个list, 用一个少一个

 
使用文档说明

    reduce(function, sequence[, initial]) -> value

    Apply a function of two arguments cumulatively to the items of a sequence, from left to right, so as to reduce the sequence to a single value.
    For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates ((((1+2)+3)+4)+5). 
    If initial is present, it is placed before the items of the sequence in the calculation, and serves as a default when the sequence is empty.

```python

def f(x, y):
    return x + y

print(reduce(f, [1, 2, 3, 4])
print(reduce(lambda x, y: x + y, [1, 2, 3, 4]))

# 输出 10
```



# Solution

```python
def chained(functions):
    def inner(param):
        for function in functions:
            param = function(param)
        return param

    return inner
```

```python
def chained(functions):
    return (lambda x: reduce(lambda v, f: f(v), functions, x))
```


# 5. [Beginner Series #3 Sum of Numbers](http://www.codewars.com/kata/55f2b110f61eb01779000053/train/python/56962db8d0417b3baf000023)

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

# 6. [Duplicate Encoder](http://www.codewars.com/kata/54b42f9314d9229fd6000d9c/train/python)

The goal of this exercise is to convert a string to a new string where each character in the new string is '(' if that character appears only once in the original string, or ')' if that character appears more than once in the original string. Ignore capitalization when determining if a character is a duplicate.

Examples:

    "din" => "((("

    "recede" => "()()()"

    "Success" => ")())())"

    "(( @" => "))(("

## Check

考察的是英语水平...

    def count(self, sub, start=None, end=None) Inferred type: (self: str, sub: str, start: Optional[Integral], end: Integral | None) -> int   
    S.count(sub[, start[, end]]) -> int
    Return the number of non-overlapping occurrences of substring sub in string S[start:end]. Optional arguments start and end are interpreted as in slice notation

## Solution

```python
def duplicate_encode(word):
    ret = ''
    ret.count()
    for i in word.upper():
        ret += ')' if word.upper().count(i) > 1 else '('
    return ret
print(duplicate_encode('Success'))
```

# 7. [Looking for a benefactor](http://www.codewars.com/kata/569b5cec755dd3534d00000f/train/python)

The accounts of the "Fat to Fit Club (FFC)" association are supervised by John as a volunteered accountant. The association is funded through financial donations from generous benefactors. John has a list of the first n donations: <code>[14, 30, 5, 7, 9, 11, 15]</code> He wants to know how much the next benefactor should give to the association so that the average of the first n + 1 donations should reach an average of 30. After doing the math he found 149. He thinks that he made a mistake. Could you help him?

    if dons = [14, 30, 5, 7, 9, 11, 15] then new_avg(dons, 30) --> 149

The function new_avg(arr, newavg) should return the expected donation (rounded up to the next integer) that will permit to reach the average newavg. Should the last donation be a non positive number (<= 0) John wants us to throw an error (return Nothing in Haskell) so that he clearly sees that his expectations are not great enough.

Notes:

- all donations are numbers, arr can be empty.
- See examples below to see which error is to be thrown.

    new_avg([14, 30, 5, 7, 9, 11, 15], 92) should return 645
    new_avg([14, 30, 5, 7, 9, 11, 15], 2) should raise ValueError

## Check

考察阅读理解能力,  math 库的一些用法：

    math.ceil(3.4) --> 4    五入
    math.floor(3.1) --> 3   四舍

其他用法如下：

![pic](https://os.alipayobjects.com/rmsportal/QteCjQrfHgTRdIb.png)

## Solution

```python
import math

def new_avg(arr, newavg):
    ret = math.ceil(newavg * (len(arr) + 1) - sum(arr))
    if ret < 0:
        raise ValueError
    else:
        return ret
```

# 8. [Triple trouble](http://www.codewars.com/kata/55d5434f269c0c3f1b000058/train/python)

Write a function

    triple_double(num1, num2)

which takes in numbers num1 and num2 and returns 1 if there is a straight triple of a number at any place in num1 and also a straight double of the same number in num2.
For example:

    triple_double(451999277, 41177722899) == 1 // num1 has straight triple 999s and 
                                          // num2 has straight double 99s

    triple_double(1222345, 12345) == 0 // num1 has straight triple 2s but num2 has only a single 2

    triple_double(12345, 12345) == 0

    triple_double(666789, 12345667) == 1

## Check

  在第一个数字中连着出现3次，同时在第二个数字中连着出现2次
  可以使用 filter + lambda  实现
 
## Solution

```python
def triple_double(num1, num2):
    return 0 if len([y for y in set([x for x in str(num1) if x * 3 in str(num1)]) if y * 2 in str(num2)]) == 0 else 1
```

# 9. [Validate Credit Card Number](http://www.codewars.com/kata/5418a1dd6d8216e18a0012b2/train/python)

In this Kata, you will implement The [Luhn Algorithm](https://en.wikipedia.org/wiki/Luhn_algorithm), which is used to help validate credit card numbers.

Given a positive integer of up to 16 digits, return true if it is a valid credit card number, and false if it is not.

Here is the algorithm:

  If there are an even number of digits, double every other digit starting with the first, and if there are an odd number of digits, double every other digit starting with the second. Another way to think about it is, from the right to left, double every other digit starting with the second to last digit.

    1714 => [1*, 7, 1*, 4] => [2, 7, 2, 4]

    12345 => [1, 2*, 3, 4*, 5] => [1, 4, 3, 8, 5]

    891 => [8, 9*, 1] => [8, 18, 1]

  If a resulting doubled number is greater than 9, replace it with either the sum of it's own digits, or 9 subtracted from it.

     [8, 18*, 1] => [8, (1+8), 1] => [8, 9, 1]

   (or)

     [8, 18*, 1] => [8, (18-9), 1] => [8, 9, 1]

  Sum all of the final digits

     [8, 9, 1] => 8+9+1 => 18

  Finally, take that sum and divide it by 10. If the remainder equals zero, the original credit card number is valid.

     18 (modulus) 10 => 8.  

     8 does not equal 0, so 891 is not a valid credit card number.


## Check

  好像是练习 for 循环的... 


  关键的是忽略提示，去从wiki上提炼出这个算法~ English~

## Solution

```python

def validate(n):
    l = [int(x) for x in list(str(n))]
    length = len(l)
    if len(l) % 2 == 0:
        for i in range(length):
            if i % 2 == 0:
                l[i] += l[i]
    else:
        for i in range(length):
            if i % 2 == 1:
                l[i] += l[i]
    for i in range(length):
        if l[i] > 9:
            l[i] = l[i] - 9
    return sum(l) % 10 == 0

#print(validate(12345))

```

# 10. [Dbftbs Djqifs](http://www.codewars.com/kata/546937989c0b6ab3c5000183/train/python)

aesar Ciphers are one of the most basic forms of encryption. It consists of a message and a key, and it shifts the letters of the message for the value of the key.

Your task is to create a function encryptor that takes 2 arguments - key and message - and returns the encrypted message.

    A message 'Caesar Cipher' and a key of 1 returns 'Dbftbs Djqifs'.

    A message 'Caesar Cipher' and a key of -1 returns 'Bzdrzq Bhogdq'.

Make sure to only shift letters, and be sure to keep the cases of the letters the same. All punctuation, numbers, spaces, and so on should remain the same.

Also be aware of keys greater than 26 and less than -26. There's only 26 letters in the alphabet!

## Check

好基本的字母移动, ord(a)  chr(36)

## Solution
```python

def encryptor(key, message):
    ret = ''
    l = []
    low_letter = [chr(x) for x in range(ord('a'), ord('z') + 1)]
    up_letter = [x.upper() for x in low_letter]
    for t in message:
        index = -1
        for i in range(len(low_letter)):
            if t == low_letter[i]:
                index, l = i, low_letter
                break;
            if t == up_letter[i]:
                index, l = i, up_letter
                break;
        if index == -1:
            ret += t
        else:
            ret += l[(i + key) % 26]
    return ret
```

# 11.[Triangle number check](http://www.codewars.com/kata/557e8a141ca1f4caa70000a6/train/python)

Description:

A triangle number is a number where n objects form an equilateral triangle (it's a bit hard to explain). For example, 6 is a triangle number because you can arrange 6 objects into an equilateral triangle:
<pre>
      1
     2 3
    4 5 6

</pre>
8 is not a triangle number because 8 objects do not form an equilateral triangle:

<pre>
   1
  2 3
 4 5 6
7 8

</pre>
In other words, the nth triangle number is equal to the sum of the n natural numbers from 1 to n.
Your task:

Check if a given input is a valid triangle number. Return true if it is, false if it is not (note that any non-integers, including non-number types, are not triangle numbers).

You are encouraged to develop an effective algorithm: test cases include really big numbers.
Assumptions:

You may assume that the given input, if it is a number, is always positive.
Notes:

0 and 1 are triangle numbers.

## Check

是否是三角形的右下角， sum(range(n))

## Solution

```python

def is_triangle_number(number):
    try:
        for i in range(number + 1):
            if number == i * (i + 1) / 2:
                return True
                break
        return False
    except Exception:
        return  False
```

