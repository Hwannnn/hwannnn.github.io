---
layout: post
published: True
title: Python coding
categories:
- Development
tags:
- Python
---

파이썬을 주언어로 즐겨 사용해왔지만 내장함수나 파이썬만의 표현 방식에 대해서는 잘 알지 못했었다.  
특히 종종 다른 사람의 코드를 리뷰하던 중에 이해가 안되는 문법이나 표현을 접하는 경우가 있어 이번 기회에 새로 학습하기로 했다.  
**"Think like python"**[^book] 에서는 파이썬만의 확장된 기능과 문법 등을 통해 파이썬을 좀더 파이썬답게 쓰는 방법에 대해서 설명하였다.



## THINK LIKE PYTHON

generally It is important to work with **python like python**.  
There are specific methods and statements in python. Python programmers emphasize simple and legibility using that. This pattern will be helpful for your work and projects.

<!--more-->


### Check your python version

1. Terminal

```bash
python --version
```
`python 2.7.8` or `python 3.4.3` 

2. Using sys module

```python
import sys

print(sys.version_info)
print(sys.version)
```

```sys.version_info(major=3, minor=5, micro=1, releaselevel=&#39;final&#39;, serial=0)
'3.5.1 |Anaconda 4.0.0 (x86_64)| (default, Dec  7 2015, 11:24:55) \n[GCC 4.2.1 (Apple Inc. build 5577)]'
```

### Follow PEP 8 guide

PEP[^PEP] is guide for the developer to keep in mind to improve productivity and cooperation with others.



1. Whitespace

   - Using space instead of tab.
   - Use 4 space for Indent having meaningful syntax.
   - The maximum length for one sentence is 79.
   - If the expression is too long, use the indent (4 space).
   - In file, separate function and class using two blank lines.
   - In class, method has one blank line for separation.
   - List index, call fund, allocating keyword don't have any space. 
   - Allocating variable have space before and after it. `a = 1`

2. naming

   - **Functions, variables, attributes** follow `lowercase_underscore`
   - **Protected instance attributes** follow `_leading_underscore`
   - **Private instance attributes** follow `__double_leading_underscore` 
   - **Classes and exceptions** follow `CapitalizedWord`
   - **Module level constants** follow `ALL_CAPS`
   - **In class and instance method,** first parameter(for instance itself) denote `self`
   - **In class method,** first parameter(for class itself) denote `cls`

3. Expression and sentences use 

   - **Use inline positive** `if a is not b` instead of negative of positive expression `if not a is b`

   - Consider blank value as a `False`

   - Consider non-blank value as a `True`

     ​

### Differences Byte, str, unicode

```python
#for python3
def to_str(bytes_or_str):
    if isinstance(bytes_or_str, bytes):
        value = bytes_or_str.decode('utf-8') #if instance is bytes,decode to str
    else:
		value = bytes_or_str
    return value # str instance

def to_bytes(bytes_or_str):
    if isinstance(bytes_or_str, str):
        value = bytes_or_str.encode('utf-8') #if instance is str, encode to bytes
    else:
        value = bytes_or_str
    return value
```



```python
#python3
with open('/tmp/random.bin','w') as f: #str instance
    f.write(os.urandom(10))
    
with open('/tmp/random.bin','wb') as f: #bytes instance
    f.write(os.urandom(10))
```





```python
#for python2
def to_unicode(unicode_or_str):
    if isinstance(unicode_or_str, str):
        value = unicode_or_str.decode('utf-8') #if instance is str, decode to str
    else:
		value = unicode_or_str
    return value # unicode instance

def to_str(unicode_or_str):
    if isinstance(unicode_or_str, unicode):
        value = unicode_or_str.encode('utf-8') #if instance is unicode, encode to str
    else:
		value = unicode_or_str
    return value # str instance
```



### Use helper method instead of complicated expression

```python
#Using bool expression
red = my_value.get('red',[''])[0] or 0 # empty str, empty list, 0 are False
green = my_value.get('green',[''])[0] or 0
opacity = my_value.get('opacity',[''])[0] or 0
print('Red:	%r' % red)
print('Green:	%r' % red)
print('Opacity:	%r' % red)
```



If you use this kind of expression many time, this expression should change to helper function.

```python
#Helper function
def get_first_int(values, key, default=0):
    found = values.get(key,[''])
    if found[0]:
        found = int(found[0])
    else:
        found = default
    return found

green = get_first_int(my_values,'green')
```



### How to slice sequence

```python
a = ['a','b','c','d','e','f','g','h']
a[:]	#['a','b','c','d','e','f','g','h']
a[:5]	#['a','b','c','d','e']
a[:-1]	#['a','b','c','d','e','f','g']
a[4:]	#[e','f','g','h']
a[-3:]	#['f','g','h']
a[2:5]	#[e','f','g','h']

b = a[:] #copy
c = a #same instance like pointer
```





### Don't use start, end, stride in one slice

```python
a = ['a','b','c','d','e','f','g','h']
a[::2]		#['b', 'd', 'f', 'h']
a[1::2]		#['a', 'c', 'e', 'g']
a[::-2]		#['h', 'f', 'd', 'b']
a[2::2]		#['c', 'e', 'g']
a[-2::-2]	#['g', 'e', 'c', 'a']
a[-2:2:-2]	#['g', 'e']
a[2:2:-2] 	#[]

#Use stride sperately
b = a[::2]	#['a', 'c', 'e', 'g']
c = b[1:-1]	#['c', 'e']
```



### Use list comprehension instead of map and filter

**List comprehension** is more effective way comparing filter and map.

```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squares = [x**2 for x in a]	#[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

#map and filter
alt = map(lambda x: x**2, filter(lambda x: x%2 ==0,a))
assert even_squares == list(alt)

#list comprehension
even_squaers = [x**2 for x in a if x % 2 == 0]
print(even_squares)
```



### Don't use list comprehension more than two

avoid the case of using more than two comprehension in one list. Consider to make helper or loop.

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

#worst case, using two comprehension in onlist is hard to understand
flat = [x for row in matrix for x in row] 
print(flat)	#>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]

#using loop, easy to understand comparing above one.
flat = list()
for row in matrix:
    for x in row:
        flat.append(x)
        
print(flat)	#>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```



### Use generator with large comprehension

When you save the huge data into the list, you'd better use generator for keeping your memory.

```python
#list comprehension example
value = [len(x) for x in open('/tmp/my_file.txt')]
print(value) #[100, 57, 15, 1, 12, 75, 5, 86, 89, 11]

#generator use '()'
it = (len(x) for x in open('/tmp/my_file.txt'))
print(it) #<generator object <genexpr> at 0x10123213>

print(next(it)) #100
print(next(it)) #57
```



Keep your mind about generator can print out only **one time** using next()



### Use enumerate instead of range

```python
#basic way
flavor_list = ['apple', 'tomato', 'banana', 'kiwi']
i = 0
for flavor in flavor_list:
	print('%d: %s' % (i + 1, favlor))
    
#basic way2
for i in range(flavor_list):
    flavor = flavor_list[i]
	print('%d: %s' % (i + 1, favlor))

#Use enumerate!
for i, flavor in enumerate(flavor_list):
	print('%d: %s' % (i + 1, favlor))

'''Result
>>>
1: 'apple'
2: 'tomato'
3: 'banana'
4: 'kiwi'
'''
```



### Use zip to process iterator in parallel

```python
names = ['Cathy','Jack','Lisa']
letters = [len(n) for n in names]

longest_name = None
max_letters = 0

# basic loop
for i in range(len(names)):
    count = letters[i]
    if count > max_letters:
        longest_name = names[i]
        max_letters = count
        
# loop using enumerate
for i,name in enumerate(names):
    count = letters[i]
    if count > max_letters:
        longest_name = names
        max_letters = count

# Use zip!
for name, count in zip(names, letters):
    if count > max_letters:
        longest_name = name
        max_letters = count
```



There are **two problem** when you use `zip`

1. In python2, zip is not generator. So you should use `izip` in  `itertools` .
2. When the length in two list is different, zip doesn't work well. you can use `zip_longest` in `itertools`(`zip_longest` in python2)

Check the length of the lists before using `zip`



### Don't use else after repetitive statement like for and while

When will the `else`  work in the python loop?

```python
for i in range(3):
    print('Loop %d' % i)
else:
    print('Else block!')

'''
Loop 0
Loop 1
Loop 2
Else block!
'''
```

```python
for i in range(3):
    print('Loop %d' % i)
	if i == 1:
        break
else:
    print('Else block!')

'''
Loop 0
Loop 1
'''
```

```python
for x in []:
    print('No run')
else:
    print('Else block!')

'''
Else block!
'''
```

Using `else` in the loop make people confused. So, avoid to use `else` in the loop!



```python
# Make helper function instead of else
def coprime(a, b):
    for i in range(2, min(a,b) + 1):
        if a% i == 0 and b % i == 0:
            return False
    return True

#helper function using break
def coprime2(a, b):
    is_coprime = True
    for i in range(2, min(a,b) + 1):
        if a% i == 0 and b % i == 0:
            is_coprime = False
            break
    return is_coprime
```

This is easy to understand. Never use `else` in the loop.



### Use benefits of each block in try/except/else/finally

```python
def load_json_key(data, key):
    try:
		result_dict = json.loads(data) # Might have ValueError
    except ValueError as e:
        raise KeyError from e
    else:
        return result_dict[key]	#Might have KeyError
```



```python
UNDEFINED = object()

def divide_json(path):
    handle = open(path, 'r+') # Might have IOError
    try:
        data = handle.read()	# Might have UnicodeDecodeError
        op = json.loads(data)	# Might have ValueError
        value = (
        op['numerator']/
        op['denominator']) # Might have ZeroDivisonError
    except ZeroDivisionError as e:
        return UNDEFINED
    else:
        op['result'] = value
        result = json.dumps(op)
        handle.seek(0)
        handle.write(result)	#Might have IOError
        return value
    finally:
        handle.close()			#Always working
```



[^PEP]: Python Enhancement Proposal. It is style guide about how to construct python code.
[^book]: "Effective python 59: specific ways to write better python" in English, 파이썬 코딩의 기술



## FUNCTION

Function is first basic tool for python developers. To improve readability and make more easily understandable code, it is useful for reuse and refactoring.



### Raise exception instead of returning None

```python
#worst case
def divide(a, b):
    try:
        return a /b
    except ZeroDivisonError:
        return None

result = divide(x, y) #resut will be 0 when y is 0. It might make a error.
if result is None:
    print('Invalid inputs')
```

```python
#Best case, use exception
def divide(a, b):
    try:
        return a /b
    except ZeroDivisonError as e:
        raise ValueEror('Invalid inputs') from e

x,y = 5, 2
try:
	result = divide(x, y)
except ValueError:
    print('Invalid inputs')
else:
    print('Result is %.1f' % result)
```



### How to work close in the variable scope

In the python, function is first class.

```python
def sort_priority(values, group):
	def helper(x): #closer means the function to handle the variable in other function
        if x in group:
            return (0, x)
        return (1, x)
    values.sort(key=helper) #Tuple check the first element in the tulple for sorting
    
    
numbers = [8, 3, 1, 2, 5, 4, 7, 6]
group = {2, 3, 5, 7}
sort_priority(numbers, group)
print(numbers)


'''
>>[2, 3, 5, 7, 1, 4, 6, 8]
'''
```

```python
#1. Use nonlocal to avoid scoping bug
def sort_priority(numbers, group):
    found = False
	def helper(x): #closer means the function to handle the variable in other function
        nonlocal found #To escape from scoping bug
        if x in group:
            found = True
            return (0, x)
        return (1, x)
    values.sort(key=helper) #Tuple check the first element in the tulple for sorting
	return found

#2. make class
class Sorter(object):
    def __init__(self, group):
		self.group = group
        self.found = False
    
    def __call__(self, x):
		if x in self.group:
            self.found = True
            return (0, x)
        return(1, x)

sorter = Sorter(group)
numbers.sort(key=sorter)
assert sorter.found is True
```



### Use generator instead of returning list

A generator can save your memory comparing a list when the size is too big.

```python
#There are two problem.
#1. Code is not simple and clear
#2. By using the list, the code lines are increased
def index_word(text):
    result = []
    if text:
		result.append(0)
	for index, letter in enumerate(text):
        if letter == ' ':
            result.append(index + 1)
	return result

address = 'Four score and seven years ago...'
result = index_words(address)
print(result[:3])

'''
>>>[0, 5, 11]
'''
```

```python
#Use the generator. Keep your mind about the generator only can use one time(no reusable)
def index_words_iter(text):
    if text:
		yield 0
    for index, letter in enumerate(text):
        if letter == ' ':
            yield index + 1


def index_file(handle):
    offest = 0
    for line in handle:
        if line:
            yield offset
		for letter in line:
            offset += 1
            if letter == ' ':
                yield offset
```



### Defensive iteration

When you use the iterative loop for a iterator, It might be a problem.

```python

```



### To make clear, use variable position argument



### Offer optional action by using keyword argument



### Use None and docstring for dynamic basic argument



### Emphasize the clearness of argument for keyword.