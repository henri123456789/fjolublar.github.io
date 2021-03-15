---
layout: post
title:  "Python Decorators"
date:   2021-01-21 01:27:15 +0200
categories: Python
---

In this page you'll find `Python Decorators` information.

<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>

### Decorators

Decorators can be thought as functions that accept a function and return the return of that function.

```python
@mydeco
def add(a, b):
	return a+b

#Above code can be seen as the same as below:

def add(a, b):
	return a+b
add = mydeco(add)
```

A shallow example of a decorator :

```python
def mydeco(func):
	def wrapper(*args, **kwargs):
		return f'{func(*args, **kwargs)} was called!'
	return wrapper
```
The outer function executes once when we decorate the function.
The inner function (wrapper) is executed every time the decorated function is called.

### Example 1. Timing the execution of a Function:

```python
def logtime(func):
	def wrapper(*args, **kwargs):
		start_time = time.time()
		result = func(*args, **kwargs)
		total_time = time.time() - start_time
		print ("Total Time took: ", total_time)
		return result
	return wrapper
```

The decorator accepts a function. It will time the execution of that function and it will print it.

### Example 2. Run the function once per minute:

```python
def logtime(func):
	last_invoked = 0
	def wrapper(*args, **kwargs):
		nonlocal last_invoked
		elapsed_time = time.time() - last_invoked
		if elapsed_time < 60:
			raise CalledTooOftenError(f'Only {elapsed_time} has passed!')
		last_invoked = time.time()
		return func(*args, **kwargs)
	return wrapper
```

The nonlocal, outer function variable `last_invoked` is created once when we decorate the function.
To change this variable inside the inner function we need to use the keyword `nonlocal`.

### Example 3. Specifying how many times per second a function needs to run.

To give a argument to the decorator function we need to use 3-func to func.

```python
def once_per_n (n):
	def middle(func):
		last_invoked = 0
		def wrapper(*args, **kwargs):
			nonlocal last_invoked
			if time.time() - last_invoked < n:
				raise CalledTooOftenError(f'Only {elapsed_time} has passed!')
			last_invoked = time.time()
			return func(*args, **kwargs)
		return wrapper
return middle	
```
The outer function executes once when we get an argument.
The middle function executes once when we decorate the function.
The inner function executes every time the function is called.

### Example 4. Memoization. In other words cache the results of function calls.

For the cache part we can use the Dictionaries that are build in python.

```python
def memoize(func):
	cache = {}
	def wrapper(*args, **kwargs):
		if args not in cache:
			print (f"Cashing NEW value for {func.__name__}{args}")
		cache[args] = func(*args, **kwargs)
		else:
			print(f"Using OLD value for {func.__name__}{args}")
		return cache[args]
	return wrapper
```

If the args are non-hashable values we can use pickle to serialize them before the caching.
This is because pickle will create strings/bytestrings that are hashable.

```python
#...
	def wrapper(*args, **kwargs):
		t = (pickle.dumps(args), pickle.dumps(kwargs))
		if t not in cache:
			#...
		cache[t] = func(*args, **kwargs)
			#...		
		return cache[t]
#...	
```

The keyword `nonlocal` is used in the inner function only if we are changing the value of that variable.
If we are accessing the variable, like we do with the dictionary, we don't need to write the keyword.

Ref: [Reuven M. Lerner - Practical decorators - PyCon 2019](https://www.youtube.com/watch?v=MjHpMCIvwsY)
