
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

# 12. [How many are smaller than me?](http://www.codewars.com/kata/56a1c074f87bc2201200002e/train/python)

Write
    maller(arr)

that given an array arr, you have to return the amount of numbers that are smaller than arr[i] to the right.

For example:

    smaller([5, 4, 3, 2, 1]) == [4, 3, 2, 1, 0]
    smaller([1, 2, 0]) == [1, 1, 0]

If you've completed this one and you feel like testing your performance tuning of this same kata, head over to the much tougher version How many are smaller than me II?

## Check

右边的数字都比这个数小, 查数

## Solution

```python
def smaller(arr):
    ret = []
    for i in range(len(arr)):
        count = 0
        for t in range(i + 1, len(arr)):
            if arr[i] > arr[t]:
                count += 1
        ret.append(count)
    return ret
```

# 13. [Wind component calculation](http://www.codewars.com/kata/542c1a6b25808b0e2600017c/train/python)

When landing an airplane manually, the pilot knows which runway he is using and usually has up to date wind information (speed and direction). This information alone does not help the pilot make a safe landing; what the pilot really needs to know is the speed of headwind, how much crosswind there is and from which side the crosswind is blowing relative to the plane.

Let's imagine there is a system in the ATC tower with speech recognition that works so that when a pilot says "wind info" over the comms, the system will respond with a helpful message about the wind.

Your task is to write a function that produces the response before it is fed into the text-to-speech engine.

Input:

- runway (string: "NN[L/C/R]"). NN is the runway's heading in tens of degrees. A suffix of L, C or R may be present and should be ignored. NN is between 01 and 36.
- wind_direction (int). Direction wind is blowing from in degrees. Between 0 and 359.
- wind_speed (int). Wind speed in knots

Output:

    a string in the following format: "(Head|Tail)wind N knots. Crosswind N knots from your (left|right)."

The wind speeds must be correctly rounded integers. If the rounded headwind component is 0, "Head" should be used. Similarly, "right" in case crosswind component is 0.

Calculating crosswind and headwind:

    A = Angle of the wind from the direction of travel (radians)
    WS = Wind speed
    CW = Crosswind
    HW = Headwind

    CW = sin(A) * WS
    HW = cos(A) * WS

More information about wind component calculation: http://en.wikipedia.org/wiki/Tailwind


## Check


# 14. [Counting Duplicates](http://www.codewars.com/kata/54bf1c2cd5b56cc47f0007a1/train/python)

Count the number of Duplicates

Write a function that will return the count of distinct case-insensitive alphabetic characters that occur more than once in the given string. The given string can be assumed to contain only uppercase and lowercase alphabets.

Example

    "abcde" -> 0 # no characters repeats more than once
    "aabbcde" -> 2 # 'a' and 'b'
    "aabbcdeB" -> 2 # 'a' and 'b'
    "indivisibility" -> 1 # 'i'
    "Indivisibilities" -> 2 # 'i' and 's'

## Check

不区分大小写, set 来去重

## Solution

```python
def duplicate_count(text):
    count = 0
    text = text.upper()
    for t in text:
        if text.count(t) > 1:
            count += 1
            text = text.replace(t, '')
    return count

def duplicate_count(text):
    return len([x for x in set(s.upper()) if s.upper().count(x) > 1])
```

# 15. [Tube strike options calculator](http://www.codewars.com/kata/568ade64cfd7a55d9300003e/train/python)

<b>Tube strike options calculator</b>

*The sweaty bus ride*

There is a tube strike today so instead of getting the London Underground home you have decided to take the bus. It's a hot day and you have been sitting on the bus for over an hour, but the bus is hardly moving. Your arm is sticking to the window and sweat drips off your nose as you try to read your neighbour's book when you say to yourself, "This is ridiculous. I could have walked faster than this!" Suddenly you have an idea for an app that helps people decide whether to walk or take the bus home when the London Underground is on strike.

You rush home (relatively speaking) and begin to define the function that will underpin your app.

<b>Function specification</b>

You must create a function which takes three parameters; walking distance home, distance the bus must travel, and the combined distance of walking from the office to the bus stop and from the bus stop to your house. All distances are in kilometres.

So for example, if your home is 5km away by foot, and the bus that takes you home travels 6km, but you have to walk 500 metres to the bus stop to catch it and 500 metres to your house once the bus arrives (i.e. 1km in total), which is faster, walking or taking the bus?

Example - Which of these is faster?:

    Start---Walk 5km--->End
    Start---Walk 500m---Bus 6km---Walk 500m--->End

Walking speed and bus driving speed have been given to you as two pre-loaded constants.

    walk = 5 (km/hr) bus = 8 (km/hr)

The function must return the fastest option, either "Walk" or "Bus". If the walk is going to be over 2 hours, the function should recommend that you take the bus. If the walk is going to be under 10 minutes, the function should recommend that you walk.

Update - If both options are going to take the same amount of time, the function should recommend that you walk.

## Check

阅读能力...

## Solution

```python
tor(distance, bus_drive, bus_walk):
    bus_time = bus_drive * 1.0 / 8 + bus_walk * 1.0 / 5
    walk_time = distance * 1.0 / 5
    min_time = min([bus_time, walk_time])
    if min_time > 2:
        return "Bus"
    if min_time < 1.0 / 6:
        return "Walk"
    return "Walk" if walk_time <= bus_time else "Bus"
```

# 16. [Decompose a number](http://www.codewars.com/kata/decompose-a-number)

Decompose a number <code>num</code> into an array (tuple in Haskell, array of arrays long[][] in C# or Java) of the form <code>[[k1,k2,k3...], r], ([k1,k2,k3...], r)</code> in Haskell, <code>[[k1,k2,k3...], [r]] in C# or Java)</code> such that:

![x](https://os.alipayobjects.com/rmsportal/nhkirTSSLwivcqY.png)

Examples:

    # when there are no `k` more than 1:

    3 

    [[], 3] = 

    3

    # when the remainder is zero:

    8330475

    [[22, 13, 10, 8, 7, 6, 6, 5, 5, 5, 4, 4, 4, 3, 3, 3, 3, 3, 3, 2, 2, 2, 2], 0] = 

    2 ^ 22 + 3 ^ 13 + 4 ^ 10 + 5 ^ 8 + 6 ^ 7 + 7 ^ 6 + 8 ^ 6 + 9 ^ 5 + 10 ^ 5 + 11 ^ 5 + 12 ^ 4 + 13 ^ 4 + 14 ^ 4 + 15 ^ 3 + 16 ^ 3 + 17 ^ 3 + 18 ^ 3 + 19 ^ 3 + 20 ^ 3 + 21 ^ 2 + 22 ^ 2 + 23 ^ 2 + 24 ^ 2 + 0 = 8330475

    # when there is both `k` and a remainder:

    26 

    [[4, 2], 1] = 

    2 ^ 4 + 3 ^ 2 + 1 = 26

    # when there is neither `k` nor a remainder:

    0

    [[], 0] = 

    0
## Check

理解数学公式， 该题目很恶心， 必须大于 2

我日， 还有 log 这个公式。。 

## Solution

```python
def fetch_index(i, num):
    if i ** 2 > num:
        return 0, num
    t = 2
    while i ** t <= num:
        t += 1
    return t - 1, num - i ** (t - 1)


def decompose(num):
    if num == 3:
        return [[], 3]
    index = []
    t = 2
    while True:
        i, num = fetch_index(t, num)
        if i > 1:
            index.append(i)
        if num <= 1 or i == 0:
            break
        t += 1
    return [index, num]

```

```python

from math import log

def decompose(n):
    i = 2
    result = []
    while n >= i*i:
        k = int(log(n, i))
        result.append(k)
        n -= i ** k
        i += 1
    
    return [result, n]
```

# 17. [Common Denominators](http://www.codewars.com/kata/54d7660d2daf68c619000d95/train/python)

Common denominators

You will have a list of rationals in the form

    [ [numer_1, denom_1] , ... [numer_n, denom_n] ]
where all numbers are positive ints.

You have to produce a result in the form

    [ [N_1, D] ... [N_n, D] ]
in which D is as small as possible and

    N_1/D == numer_1/denom_1 ... N_n/D == numer_n,/denom_n.
Example :

    [ [1, 2], [1, 3], [1, 4] ] produces the array [ [6,12], [4,12], [3,12] ]

## Check
    
类似求最小公倍数的, 先将每一个数约分， 然后分母的最小公倍数弄出来

最小公倍数 = m * n / (m, n)的最大公约数

最小公倍数， 用欧几里得辗转相除法， 否则会超时 
    

## Solution

```python
def gcd(a, b):
    if a < b:
        a, b = b, a

    while b != 0:
        temp = a % b
        a = b
        b = temp

    return a


def convertFracts(lst):
    fz, fm = 1, 1
    for l in lst:
        gys = gcd(l[0], l[1])
        l[0], l[1] = l[0] // gys, l[1] // gys

    multiple = 1

    for l in lst:
        multiple = multiple * l[1] // gcd(multiple, l[1])
    ret = []
    for l in lst:
        tmp = [multiple * l[0] // l[1], multiple // 1]
        ret.append(tmp)
    return ret
```
