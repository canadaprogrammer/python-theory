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

- Some sequence types (such as `range`) only support item sequences that follow specific patterns, and hence don’t support sequence concatenation or repetition.

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

  - `clear()` and `copy()` are included for consistency with the interfaces of mutable containers that don’t support slicing operations (such as dict and set). `copy()` is not part of the collections.abc.MutableSequence ABC, but most concrete mutable sequence classes provide it.

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
