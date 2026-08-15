# Python Collections for DSA (STL / Java Collections equivalent)

Python has no single unified library like C++ STL or Java Collections.
Instead it's: **built-in types + a few standard library modules.**

---

## 1. Built-in Types

### List — dynamic array (vector / ArrayList)
```python
lst = [1, 2, 3]
lst.append(4)          # add to end
lst.pop()               # remove from end
lst.insert(1, 10)       # insert at index
lst.remove(10)          # remove by value
lst.extend([5, 6])      # add multiple
lst[::-1]                # reverse (slicing)
lst.sort()               # in-place sort
sorted(lst, reverse=True)  # returns new sorted list
[x*x for x in lst]       # list comprehension
```

### Tuple — immutable fixed array
```python
t = (1, 2, 3)
# Hashable -> usable as dict key / set element (unlike list)
```

### Dict — hashmap (unordered_map / HashMap)
```python
d = {"a": 1}
d.get("b", 0)            # safe access with default
d["b"] = 2
for k, v in d.items():
    pass
"a" in d                  # O(1) key check
```
Note: since Python 3.7, dict preserves insertion order.

### Set / Frozenset — hashset (unordered_set / HashSet)
```python
s = {1, 2, 3}
s.add(4)
s.discard(2)              # remove without error if missing
a | b   # union
a & b   # intersection
a - b   # difference
a ^ b   # symmetric difference (in one but not both)
```

### String — immutable char array
```python
s = "hello"
s.split(",")
"-".join(["a", "b"])
s.strip()
s[::-1]                   # reverse
```

---

## 2. `collections` module

```python
from collections import deque, Counter, defaultdict, OrderedDict, namedtuple
```

### `deque` — double-ended queue (use as Stack AND Queue)
```python
dq = deque()
dq.append(x)        # push back
dq.appendleft(x)     # push front
dq.pop()              # pop back
dq.popleft()          # pop front  -> O(1), unlike list.pop(0) which is O(n)
dq.rotate(2)
```

### `Counter` — frequency map / multiset
```python
cnt = Counter("aabbbc")
cnt.most_common(2)      # top 2 frequent elements
cnt.elements()            # iterator with repeats
```

### `defaultdict` — dict with default factory (avoids key-check boilerplate)
```python
d = defaultdict(list)
d["x"].append(1)          # no KeyError even if "x" didn't exist
```

### `OrderedDict` — remembers insertion order, supports reordering
```python
od = OrderedDict()
od.move_to_end("key")     # useful for LRU Cache problems
```

### `namedtuple` — lightweight struct
```python
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
p.x, p.y
```

---

## 3. `heapq` module — Priority Queue (min-heap)

No separate class — operates directly on a plain list.

```python
import heapq

h = []
heapq.heappush(h, 3)
heapq.heappush(h, 1)
heapq.heappop(h)          # removes smallest

heapq.heapify(h)          # convert list to heap in O(n)

# Max-heap trick: negate values
heapq.heappush(h, -x)
-heapq.heappop(h)
```

**Common problems:** Kth Largest Element, Top K Frequent Elements, Merge K Sorted Lists.

---

## 4. `bisect` module — Binary Search on sorted list

```python
import bisect

a = [1, 2, 2, 3, 5]
bisect.bisect_left(a, 2)    # leftmost insert position
bisect.bisect_right(a, 2)   # rightmost insert position
bisect.insort(a, 4)          # insert while keeping list sorted
```

**Common problems:** Search Insert Position, Find First/Last Occurrence.

---

## 5. `itertools` module — combinatorics (useful for backtracking)

```python
from itertools import permutations, combinations, product

list(permutations([1, 2, 3]))
list(combinations([1, 2, 3], 2))
list(product([1, 2], repeat=2))
```

---

## 6. The Gap: No built-in balanced BST

C++ has `std::map` / `std::set` (ordered, O(log n) insert/search/range query).
Java has `TreeMap` / `TreeSet`.
**Python has no direct standard-library equivalent.**

Workarounds:
- Third-party: `sortedcontainers` → `SortedList`, `SortedDict`, `SortedSet`
  ```python
  from sortedcontainers import SortedList
  sl = SortedList([3, 1, 2])
  sl.add(4)
  sl.index(2)     # O(log n) lookup
  ```
- Or simulate using `heapq` + `bisect` depending on the problem.

---

## 7. Quick Cheat Sheet

| Need                        | Python                         |
|------------------------------|---------------------------------|
| Stack                        | `list`                          |
| Queue                        | `collections.deque`             |
| Priority Queue                | `heapq`                          |
| HashMap                       | `dict`                           |
| HashSet                       | `set`                            |
| Ordered/sorted set            | `sortedcontainers.SortedList` (3rd party) |
| Frequency count                | `collections.Counter`            |
| Sliding window / two-pointer   | `collections.deque`               |
| Binary search on sorted list    | `bisect`                           |
| Struct-like object              | `collections.namedtuple`           |
| Combinatorics (perm/comb)        | `itertools`                          |

---

## 8. Suggested Practice Order

1. List / Dict / Set basics
2. `Counter` / `defaultdict` (frequency problems)
3. `deque` (stack + queue problems)
4. `heapq` (top-K, kth element problems)
5. `bisect` (binary search variants)

---

## 9. Official Documentation Links

- Data structures overview: https://docs.python.org/3/tutorial/datastructures.html
- Built-in types (list, dict, set, tuple): https://docs.python.org/3/library/stdtypes.html
- `collections`: https://docs.python.org/3/library/collections.html
- `heapq`: https://docs.python.org/3/library/heapq.html
- `bisect`: https://docs.python.org/3/library/bisect.html
- `itertools`: https://docs.python.org/3/library/itertools.html
- `sortedcontainers` (3rd party): https://grantjenks.com/docs/sortedcontainers/