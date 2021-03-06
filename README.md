# Python Theory

- Code on https://replit.com/@canadaprogramme/Python-theory#main.py

## Data Types

- string: "string", 'string'

- number: 1

- float: 1.2

- boolean: True, False

- none: None

## Common Sequence Types Operations

- While the `in` and `not in` operations are used only for simple containment testing in the general case, some specialized sequences (such as str, `bytes` and `bytearray`) also use them for subsequence testing:

  - ```python
    >>> "gg" in "eggs"
    True
    >>> days = ['Mon', 'Tue', 'Wed]
    >> print('Mon' in days)
    True
    ```

- Values of n less than `0` are treated as `0` (which yields an empty sequence of the same type as s). Note that items in the sequence s are not copied; they are referenced multiple times. This often haunts new Python programmers; consider:

  - ```python
    >>> lists = [[]] * 3
    >>> lists
    [[], [], []]
    >>> lists[0].append(3)
    >>> lists
    [[3], [3], [3]]
    ```

  - What has happened is that `[[]]` is a one-element list containing an empty list, so all three elements of` [[]] \* 3` are references to this single empty list. Modifying any of the elements of `lists` modifies this single list. You can create a list of different lists this way:

    - ```python
      >>> lists = [[] for i in range(3)]
      >>> lists[0].append(3)
      >>> lists[1].append(5)
      >>> lists[2].append(7)
      >>> lists
      [[3], [5], [7]]
      ```

- `len(s)`: length of s

- If i or j is negative, the index is relative to the end of sequence s: `len(s) + i` or `len(s) + j` is substituted. But note that `-0` is still `0`.

  - `s[i]`: ith item of s, origin 0

  - `s[i:j]`: slice of s from i to j

- The slice of s from i to j is defined as the sequence of items with index k such that `i <= k < j`. If i or j is greater than `len(s)`, use `len(s)`. If i is omitted or `None`, use `0`. If j is omitted or `None`, use `len(s)`. If i is greater than or equal to j, the slice is empty.

- The slice of s from i to j with step k is defined as the sequence of items with index `x = i + n*k` such that `0 <= n < (j-i)/k`. In other words, the indices are `i`, `i+k`, `i+2*k`, `i+3*k` and so on, stopping when j is reached (but never including j). When k is positive, i and j are reduced to `len(s)` if they are greater. When k is negative, i and j are reduced to `len(s) - 1` if they are greater. If i or j are omitted or None, they become "end" values (which end depends on the sign of k). Note, k cannot be zero. If k is `None`, it is treated like 1.

  - `s[i:j:k]`: slice of s from i to j with step k

- Concatenating immutable sequences always results in a new object. This means that building up a sequence by repeated concatenation will have a quadratic runtime cost in the total sequence length. To get a linear runtime cost, you must switch to one of the alternatives below:

  - `s + t`: the concatenation of s and t

  - if concatenating `str` objects, you can build a list and use `str.join()` at the end or else write to an `io.StringIO` instance and retrieve its value when complete

  - if concatenating `bytes` objects, you can similarly use `bytes.join()` or `io.BytesIO`, or you can do in-place concatenation with a `bytearray` object. `bytearray` objects are mutable and have an efficient overallocation mechanism

  - if concatenating `tuple` objects, extend a `list` instead

  - for other types, investigate the relevant class documentation

- Some sequence types (such as `range`) only support item sequences that follow specific patterns, and hence don???t support sequence concatenation or repetition.

- `index` raises `ValueError` when x is not found in s. Not all implementations support passing the additional arguments i and j. These arguments allow efficient searching of subsections of the sequence. Passing the extra arguments is roughly equivalent to using` s[i:j].index(x)`, only without copying any data and with the returned index being relative to the start of the sequence rather than the start of the slice.

  - `s.index(x[, i[, j]])`: index of the first occurrence of x in s (at or after index i and before index j)

- `min(s)`: smallest item of s

- `max(s)`: largest item of s

- `s.count(x)`: total number of occurrences of x in s

## Mutable Sequence Types Operation

- `s[i] = x`: item i of s is replaced by x

- `s[i:j] = t`: slice of s from i to j is replaced by the contents of the iterable t

- `del s[i:j]`: same as `s[i:j] = []`

- `s[i:j:k] = t`: the elements of `s[i:j:k]` are replaced by those of t

  - t must have the same length as the slice it is replacing.

- `del s[i:j:k]`: removes the elements of `s[i:j:k]` from the list

- `s.append(x)`: appends x to the end of the sequence (same as `s[len(s):len(s)] = [x]`)

- `s.clear()`: removes all items from s (same as `del s[:]`)

- `s.copy()`: creates a shallow copy of s (same as `s[:]`)

  - `clear()` and `copy()` are included for consistency with the interfaces of mutable containers that don???t support slicing operations (such as dict and set). `copy()` is not part of the collections.abc.MutableSequence ABC, but most concrete mutable sequence classes provide it.

  - New in version 3.3: clear() and copy() methods.

- `s.extend(t)` or `s += t`: extends s with the contents of t (for the most part the same as
  `s- [len(s):len(s)] = t`)

- `s *= n`: updates s with its contents repeated n times

  - The value n is an integer, or an object implementing `__index__()`. Zero and negative values of n clear the sequence. Items in the sequence are not copied; they are referenced multiple times, as explained for `s * n` under Common Sequence Operations.

- `s.insert(i, x)`: inserts x into s at the index given by i (same as s[i:i] = [x])

- `s.pop()` or `s.pop(i)`: retrieves the item at i and also removes it from s

  - The optional argument i defaults to `-1`, so that by default the last item is removed and returned.

- `s.remove(x)`: remove the first item from s where s[i] is equal to x

  - `remove()` raises ValueError when x is not found in s.

- `s.reverse()`: reverses the items of s in place

  - The `reverse()` method modifies the sequence in place for economy of space when reversing a large sequence. To remind users that it operates by side effect, it does not return the reversed sequence.

### Lists

- Lists are mutable sequences, typically used to store collections of homogeneous items (where the precise degree of similarity will vary by application).

  - ```python
    >>>days = ['Mon', 'Tue', 'Wed']
    >>>print(type(days));
    <class 'list'>
    >>>print('Mon' in days)
    yes
    ```

## Immutable Sequence Types

### Tuples

- Tuples are immutable sequences, typically used to store collections of heterogeneous data (such as the 2-tuples produced by the enumerate() built-in). Tuples are also used for cases where an immutable sequence of homogeneous data is needed (such as allowing storage in a `set` or `dict` instance).

- Tuples implement all of the common sequence operations.

  - ```python
    >>>days = ('Mon', 'Tue', 'Wed')
    >>>print(type(days))
    <class 'tuple'>
    >>>print('Mon' in days)
    yes
    ```

### Dict

- Dictionaries can be created by placing a comma-separated list of key: value pairs within braces, for example: `{'jack': 4098, 'sjoerd': 4127}` or `{4098: 'jack', 4127: ['sjoerd', 'test']}`, or by the dict constructor.

- Dictionaries can be created by several means:

  - Use a comma-separated list of key: value pairs within braces: `{'jack': 4098, 'sjoerd': 4127}` or `{4098: 'jack', 4127: 'sjoerd'}`

  - Use a dict comprehension: `{}`, `{x: x ** 2 for x in range(10)}`

  - Use the type constructor: `dict()`,` dict([('foo', 100), ('bar', 200)])`, `dict(foo=100, bar=200)`

  - ```python
    >>>person1 = {
    >>>  'name': 'Jin',
    >>>  'age': 20,
    >>>  'korean': True
    >>>}
    >>>print(person1)
    {'name':'Jin','age':20,'korean':True}
    >>>person1['handsome'] = True
    >>>print(person1)
    {'name':'Jin','age':20,'korean':True,'handsome':True}
    ```

## Built-in Functions

- A

  - `abs()`:
  - `aiter()`:
  - `all()`:
  - `any()`:
  - `anext()`:
  - `ascii()`:

- B

  - `bin()`:
  - `bool(x)`: Return a Boolean value, i.e. one of True or False. x is converted using the standard truth testing procedure. If x is false or omitted, this returns False; otherwise, it returns True. The bool class is a subclass of int (see Numeric Types ??? int, float, complex). It cannot be subclassed further. Its only instances are False and True (see Boolean Values).

    - Changed in version 3.7: x is now a positional-only parameter.

  - `breakpoint()`:
  - `bytearray()`:
  - `bytes()`:

- C

  - `callable()`:
  - `chr()`:
  - `classmethod()`:
  - `compile()`:
  - `complex()`:

- D

  - `delattr()`:
  - `dict()`:
  - `dir()`:
  - `divmod()`:

- E

  - `enumerate()`:
  - `eval()`:
  - `exec()`:

- F

  - `filter()`:
  - `float(x)`: Return a floating point number constructed from a number or string x.

    - If the argument is a string, it should contain a decimal number, optionally preceded by a sign, and optionally embedded in whitespace. The optional sign may be `'+'` or `'-'`; a `'+'` sign has no effect on the value produced. The argument may also be a string representing a NaN (not-a-number), or positive or negative infinity. More precisely, the input must conform to the following grammar after leading and trailing whitespace characters are removed

    - Here `floatnumber` is the form of a Python floating-point literal, described in Floating point literals. Case is not significant, so, for example, ???inf???, ???Inf???, ???INFINITY???, and ???iNfINity??? are all acceptable spellings for positive infinity.

    - Otherwise, if the argument is an integer or a floating point number, a floating point number with the same value (within Python???s floating point precision) is returned. If the argument is outside the range of a Python float, an OverflowError will be raised.

    - For a general Python object `x`, `float(x)` delegates to `x.__float__()`. If `__float__()` is not defined then it falls back to `__index__()`.

    - If no argument is given, `0.0` is returned.

    - Examples:

      - ```python
        >>>
        >>> float('+1.23')
        1.23
        >>> float('   -12345\n')
        -12345.0
        >>> float('1e-003')
        0.001
        >>> float('+1E6')
        1000000.0
        >>> float('-Infinity')
        -inf
        ```

    - The float type is described in Numeric Types ??? `int`, `float`, `complex`.

    - Changed in version 3.6: Grouping digits with underscores as in code literals is allowed.

    - Changed in version 3.7: x is now a positional-only parameter.

    - Changed in version 3.8: Falls back to `__index__()` if `__float__()` is not defined.

  - `format()`:
  - `frozenset()`:

- G

  - `getattr()`:
  - `globals()`:

- H

  - `hasattr()`:
  - `hash()`:
  - `help()`:
  - `hex()`:

- I

  - `id()`:
  - `input()`:
  - `int(x, base=10)`:

    - Return an integer object constructed from a number or string `x`, or return `0` if no arguments are given. If x defines `__int__()`, `int(x)` returns `x.__int__()`. If `x` defines `__index__()`, it returns `x.__index__()`. If `x` defines `__trunc__()`, it returns `x.__trunc__()`. For floating point numbers, this truncates towards zero.

    - If x is not a number or if base is given, then x must be a string, bytes, or bytearray instance representing an integer literal in radix base. Optionally, the literal can be preceded by `+` or `-` (with no space in between) and surrounded by whitespace. A base-n literal consists of the digits 0 to n-1, with `a` to `z` (or `A` to `Z`) having values 10 to 35. The default base is 10. The allowed values are 0 and 2???36. Base-2, -8, and -16 literals can be optionally prefixed with `0b`/`0B`, `0o`/`0O`, or `0x`/`0X`, as with integer literals in code. Base 0 means to interpret exactly as a code literal, so that the actual base is 2, 8, 10, or 16, and so that `int('010', 0)` is not legal, while `int('010')` is, as well as i`nt('010', 8)`.

    - The integer type is described in Numeric Types ??? int, float, complex.

    - Changed in version 3.4: If base is not an instance of int and the base object has a `base.__index__` method, that method is called to obtain an integer for the base. Previous versions used `base.__int__` instead of `base.__index__`.

    - Changed in version 3.6: Grouping digits with underscores as in code literals is allowed.

    - Changed in version 3.7: x is now a positional-only parameter.

    - Changed in version 3.8: Falls back to `__index__()` if `__int__()` is not defined.

  - `isinstance()`:
  - `issubclass()`:
  - `iter()`:

- L

  - `len(s)`: Return the length (the number of items) of an object. The argument may be a sequence (such as a string, bytes, tuple, list, or range) or a collection (such as a dictionary, set, or frozen set).
  - `list()`:
  - `locals()`:

- M

  - `map()`:
  - `max()`:
  - `memoryview()`:
  - `min()`:

- N

  - `next()`:

- O

  - `object()`:
  - `oct()`:
  - `open()`:
  - `ord()`:

- P

  - `pow()`:
  - `print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`:

    - Print objects to the text stream file, separated by sep and followed by end. sep, end, file, and flush, if present, must be given as keyword arguments.

    - All non-keyword arguments are converted to strings like `str()` does and written to the stream, separated by sep and followed by end. Both sep and end must be strings; they can also be None, which means to use the default values. If no objects are given, `print()` will just write end.

    - The file argument must be an object with a `write(string)` method; if it is not present or `None`, `sys.stdout` will be used. Since printed arguments are converted to text strings, print() cannot be used with binary mode file objects. For these, use `file.write(...)` instead.

    - Whether the output is buffered is usually determined by file, but if the flush keyword argument is true, the stream is forcibly flushed.

    - Changed in version 3.3: Added the flush keyword argument.

  - `property()`:

- R

  - `range()`:
  - `repr()`:
  - `reversed()`:
  - `round()`:

- S

  - `set()`:
  - `setattr()`:
  - `slice()`:
  - `sorted()`:
  - `staticmethod()`:
  - `str(object=b'', encoding='utf-8', errors='strict')`:

    - Return a `string` version of object. If object is not provided, returns the empty string. Otherwise, the behavior of `str()` depends on whether encoding or errors is given, as follows.

    - If neither encoding nor errors is given, `str(object)` returns `object.__str__()`, which is the ???informal??? or nicely printable string representation of object. For string objects, this is the string itself. If object does not have a `__str__()` method, then `str()` falls back to returning `repr(object)`.

    - If at least one of encoding or errors is given, object should be a `bytes-like object` (e.g. `bytes` or `bytearray`). In this case, if object is a `bytes` (or `bytearray`) object, then `str(bytes, encoding, errors)` is equivalent to `bytes.decode(encoding, errors)`. Otherwise, the bytes object underlying the buffer object is obtained before calling `bytes.decode()`. See Binary Sequence Types ??? `bytes`, `bytearray`, `memoryview` and `Buffer Protocol` for information on buffer objects.

    - Passing a `bytes` object to `str()` without the encoding or errors arguments falls under the first case of returning the informal string representation (see also the -b command-line option to Python). For example:

    - ```python
      >>>
      >>> str(b'Zoot!')
      "b'Zoot!'"
      ```

  - `sum()`:
  - `super()`:

- T

  - `tuple()`:
  - `type(name, bases, dict, **kwds)`:

    - With one argument, return the type of an object. The return value is a type object and generally the same object as returned by `object.__class__`.

    - The `isinstance()` built-in function is recommended for testing the type of an object, because it takes subclasses into account.

    - With three arguments, return a new type object. This is essentially a dynamic form of the `class` statement. The name string is the class name and becomes the `__name__` attribute. The bases tuple contains the base classes and becomes the `__bases__` attribute; if empty, `object`, the ultimate base of all classes, is added. The dict dictionary contains attribute and method definitions for the class body; it may be copied or wrapped before becoming the `__dict__` attribute. The following two statements create identical `type` objects:

    - ```python
      >>> class X:
      ...  a = 1
      ...
      >>> X = type('X', (), dict(a=1))
      ```

    - Keyword arguments provided to the three argument form are passed to the appropriate metaclass machinery (usually `__init_subclass__()`) in the same way that keywords in a class definition (besides metaclass) would.

    - Changed in version 3.6: Subclasses of type which don???t override `type.__new__` may no longer use the one-argument form to get the type of an object.

- V

  - `vars()`:

- Z

  - `zip()`:

- **import**()

## Define Function

- ```python
  def say():
      print('hi')

  say() # hi

  def say_hello(name):
      print('hello', name)

  say_hello() # TypeError: say_hello() missing 1 required positional argument: 'name'
  say_hello('Jin')  # hello Jin

  def plus(a, b):
    print(a + b)

  plus(3 + 2) # 5
  plus(2) # TypeError: plus() missing 1 required positional argument: 'b'

  # set default value
  def minus(a, b=0):
    print(a - b)

  minus(2) # 2

  def r_plus(a, b):
    return a + b

  result = r_plus(3, 5)
  print(result) # 8

  # keyworded arguments
  def divide(a, b):
    return a / b

  d_result = divide(b=10, a=40)
  print(d_result) # 4.0

  # string with variable = f"string {variable} "
  def say_hi(name, age):
    return f"Hi {name}, you are {age} years old"

  hi = say_hi(age=20, name='Jin')
  print(hi) # Hi Jin, you are 20 years old
  ```

## Calculator

- `x + y`, `x - y`, `x * y`, `x / y`, `x % y`, `-x`, `x ** y`

- ```python
  def calculator(a, op, b='No Input'):
    try:
      if b=='No Input' and op=='-1':
        return f'negated {a} = {-1 * float(a)}'
      elif type(op) is not str:
        return 'Operator needs to be string'
      else:
        switcher = {
          '+': float(a) + float(b),
          '-': float(a) - float(b),
          '*': float(a) * float(b),
          '/': float(a) / float(b),
          '%': float(a) % float(b),
          '**': float(a) ** float(b),
          '-1': -1 * float(a),
        }
        return f'{a} {op} {b} = ' + str(switcher.get(op, f'The "{op}" operation doesn\'t exsit.'));
    except:
      return f'{a} {op} {b}: Error'

  print(calculator(4.2, '+', 3))    # 4.2 + 3 = 7.2
  print(calculator(4, '-', '3'))    # 4 - 3 = 1.0
  print(calculator(4, '*', '3.5'))  # 4 * 3.5 = 14.0
  print(calculator('4', '/', 1))    # 4 / 1 = 4.0
  print(calculator('4', '%', '2'))  # 4 % 2 = 0.0
  print(calculator(4, '**'))        # 4 ** No Input: Error
  print(calculator(4, '-1'))        # negated 4 = -4.0
  print(calculator(4, 1))           # Operator needs to be string
  ```

## Improved Calculator

- ```python
  def check_num(x):
    if type(x) is int or type(x) is float:
      return True
    else:
      return False

  def calculator(a, op, b=None):
    try:
      if check_num(a) and op=='-1' and b==None:
        return f'negated {a} = {-1 * a}'
      elif type(op) is not str:
        return 'Operator needs to be string.'
      elif check_num(a) and check_num(b):
        switcher = {
          '+': a + b,
          '-': a - b,
          '*': a * b,
          '/': a / b,
          '%': a % b,
          '**': a ** b,
          '-1': -1 * a,
        }
        return f'{a} {op} {b} = ' + str(switcher.get(op, f'The "{op}" operation doesn\'t exist.'));
      else:
        result = f'{a} and {b} need to be integer or float.'
        if not check_num(a):
          result = result + f'\n\t type of {a}: {type(a)}'
        if not check_num(b):
          result = result + f'\n\t type of {b}: {type(b)}'
        return result
    except:
      return f'{a} {op} {b}: Error'

  print(calculator(4.2, '+', 3))    # 4.2 + 3 = 7.2
  print(calculator(4, '-', '3'))    # 4 and 3 need to be integer or float.
                                    #   type of 3: <class 'str'>
  print(calculator(4, '*', 3.5))    # 4 * 3.5 = 14.0
  print(calculator('4', '/', 1))    # 4 and 1 need to be integer or float.
                                    #   type of 4: <class 'str'>
  print(calculator(4, '%', 2))      # 4 % 2 = 0
  print(calculator(4, '**'))        # 4 ** None need to be integer or float.
                                    #   type of None: <class 'NoneType'>
  print(calculator(4, '-1'))        # negated 4 = -4
  print(calculator(4, 1))           # Operator needs to be string.
  ```

## for in

- ```python
  days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri']

  for day in days:
    if day == 'Wed':
      break
    else:
      print(day)

  # Mon
  # Tue

  for letter in 'red':
    print(letter)

  # r
  # e
  # d

  for x in range(1, 4):
    print(x)

  # 1
  # 2
  # 3
  ```

## Modules

- math

  - ```python
    import math

    print(math.ceil(2.2)) # 3
    ```

  - ```python
    from math import ceil

    print(ceil(2.2))  # 3
    ```

  - ```python
    from math import ceil as new_name

    print(new_name(2.2))  # 3
    ```

## Get Function from File

- Create `calculator.py`

  - ```python
    def plus(a, b):
      return a + b
    ```

- On `main.py`

  - ```python
    from calculator import plus

    print(plus(1, 3)) # 4
    ```

<!-- prettier-ignore-start -->

## *args and **kwargs

- `*args` allows to pass a varying number of **positional arguments**.

  - using list

    - ```python
      def my_sum(my_integers):
        result = 0
        for x in my_integers:
          result += x
        return result

      list_of_integers = [1, 2, 3]
      print(my_sum(list_of_integers))
      ```

  - using *args

    - ```python
      def my_sum(*args):
        result = 0
        # Iterating over the Python args tuple
        for x in args:
          result += x
        return result

      print(my_sum(1, 2, 3))
      ```

- `**kwargs` works just like *args, but instead of accepting positional arguments it accepts **keyword (or named) arguments**.

  - ```python
    def concatenate(**kwargs):
      result = ""
      key = ""
      # Iterating over the Python kwargs dictionary
      for arg in kwargs.values():
        result += arg
      return result
      # RealPythonIsGreat!

      # for arg in kwargs:
      #   key += arg
      # return key
      ## abcde
    print(concatenate(a="Real", b="Python", c="Is", d="Great", e="!"))
    ```

- If you want to create a function that takes a changeable number of both positional and named arguments, non-default arguments have to precede default arguments, so `*args` must come before `**kwargs`.

  - `def my_function(a, b, *args, **kwargs):`

<!-- prettier-ignore-end -->

## Object Oriented Programming (OOP)

- ```python
  class Person:
    def __init__(self, **kwarg):
      self.name = kwarg.get('name', 'Anonymous')
      self.age = kwarg.get('age', 0)
      self.language = kwarg.get('language', 'Korean')

    # method
    def myfunc(self):
      print(f"Hello my name is {self.name}.")

    def __str__(self):
      return f"{self.name} is {self.age}, and the native language is {self.language}"

  p1 = Person(name='John', age=20, language='English')
  p1.myfunc()
  # Hello my name is John.
  print(p1)
  # John is 20, and the native language is English
  ```

- `__init__()`

  - All classes have a function called `__init__()`, which is always executed when the class is being initiated.

  - Use the `__init__()` function to assign values to object properties, or other operations that are necessary to do when the object is being created.

- Object Methods

  - Methods in objects are functions that belong to the object.

  - `self` parameter is a reference to the current instance of the class, and is used to access variables that belong to the class.

    - It doesn't have to be named `self`, you can call it whatever you like, but **it has to be the first parameter of any function in the class**.

- `dir(object)` function

  - It returns all properties and methods of the specified object, without the values, even built-in properties which are default for all object such as `__str__`, `

### Child Class

- To create a class that inherits the functionality from another class, send the parent class as a parameter when creating the child class.

  - ```python
    class Student(Person):
      def __init__(self, **kwarg):
        # inherit the parent parameters
        super().__init__(**kwarg)
        self.student_id = kwarg.get('student_id', 'No ID')

      def __str__(self):
        return f"My student ID is {self.student_id}"

    class Junior(Student):
      pass

    s1 = Student(student_id='1234')
    print(s1)
    # My student ID is 1234
    print(s1.name, s1.student_id)
    # Anonymous 1234
    ```

    - `pass` keyword when you do not want to add any other properties or methods tot he class.

## Decorators

- A decorator is a design pattern in Python that allows a user to add new functionality to an existing object without modifying its structure. Decorators are usually called before the definition of a function you want to decorate.

  - ```python
    def uppercase_decorator(function):
      def wrapper():
        func = function()
        make_uppercase = func.upper()
        return make_uppercase
      return wrapper

    def say_hi():
      return 'hello there'

    decorate = uppercase_decorator(say_hi)
    decorate()
    # 'HELLO THERE'
    ```

- Use the @ symbol before the function we'd like to decorate.

  - ```python
    def uppercase_decorator(function):
      def wrapper():
        func = function()
        make_uppercase = func.upper()
        return make_uppercase
      return wrapper

    @uppercase_decorator
    def say_hi():
      return 'hello there'

    say_hi()
    # 'HELLO THERE'
    ```

- Multiple Decorators

  - ```python
    def uppercase_decorator(function):
      def wrapper():
        func = function()
        make_uppercase = func.upper()
        return make_uppercase

      print('1', wrapper())
      return wrapper

    def split_string(function):
      def wrapper():
        func = function()
        splitted_string = func.split()
        return splitted_string

      print('2', wrapper())
      return wrapper

    @split_string
    @uppercase_decorator
    def say_hi():
      print('3, say')
      return 'hello there'

    print(say_hi())
    # 3, say
    # 1 HELLO THERE
    # 3, say
    # 2 ['HELLO', 'THERE']
    # 3, say
    # ['HELLO', 'THERE']
    ```

## Introduction to Flask

### Route

- ```python
  from flask import Flask

  app = Flask("SuperScrapper")

  @app.route("/")
  def home():
    return "Hello! Welcome to Flask!"

  @app.route("/contact")
  def contact_us():
    return "Contact Us"

  # 0.0.0.0 is the host of replit.com (https://Python-theory.canadaprogramme.repl.co)
  app.run(host="0.0.0.0")
  ```

### Dynamic URLs

- ```python
  @app.route("/<username>")
  def contact_us(username):
    return f"Hello, {username}. How are you doing?"
  ```

### Templates

- Create `/templates/home.html`

  - ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>Job Search</title>
      </head>
      <body>
        <h1>Job Search</h1>
        <form>
          <input placeholder="Which job do you want?" required />
          <button>Search</button>
        </form>
      </body>
    </html>
    ```

- On `main.py`

  - ```python
    from flask import Flask, render_template

    app = Flask("SuperScrapper")

    @app.route("/")
    def home():
      return render_template('home.html')
    ```

## Forms and Query Arguments

- On `main.py`

  - ```python
    from flask import ..., request

    ...

    @app.route("/report")
    def report():
      word = request.args.get('word')
      return render_template("report.html",searchBy=word)
    ```

- Create `/templates/report.html`

  - ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>Job Search</title>
      </head>
      <body>
        <h1>Search Results:</h1>
        <h3>You are looking for "{{searchBy}}."</h3>
      </body>
    </html>
    ```

## Redirect

- On `main.py`

  - ```python
    from flask import ..., redirect

    ...

    def report():
      word = request.args.get('word')
      if word:  # if word exists
        word = word.lower()
      else:     # if word doesn't exists, go to home
        return redirect('/')

      return render_template("report.html", searchBy=word)
    ```
