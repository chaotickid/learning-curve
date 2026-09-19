# Java Collections Framework — In-Depth Guide

## 1. Introduction to Java Collections

The **Java Collections Framework (JCF)** is a unified architecture provided by Java to store, manipulate, and process groups of objects.

Before collections, Java primarily used arrays to store multiple objects.

### Array Example

```java
String[] names = new String[5];

names[0] = "Aditya";
names[1] = "Rahul";
names[2] = "Amit";
```

Arrays have several limitations:

* Fixed size
* Limited built-in operations
* Difficult insertion/deletion
* No standard searching/sorting abstraction
* Cannot directly work with generic collection algorithms

Collections solve these problems by providing reusable data structures and algorithms.

---

# 2. Why Collections Are Required

Consider an application where the number of users is unknown.

With an array:

```java
User[] users = new User[1000];
```

You must decide the size beforehand.

With `ArrayList`:

```java
List<User> users = new ArrayList<>();

users.add(user1);
users.add(user2);
```

The collection can dynamically grow.

Collections provide:

* Dynamic sizing
* Searching
* Sorting
* Insertion
* Deletion
* Duplicate handling
* Key-value storage
* Thread-safe variants
* Stream processing
* Type safety through generics

---

# 3. Collection Framework Hierarchy

The major hierarchy can be represented as:

```text
                         Iterable
                            |
                       Collection
                            |
          +-----------------+------------------+
          |                 |                  |
         List               Set               Queue
          |                 |                  |
    +-----+-----+      +----+----+       +-----+------+
    |           |      |         |       |            |
ArrayList    LinkedList HashSet TreeSet PriorityQueue Deque
    |           |
Vector       ArrayDeque
    |
Stack
```

`Map` is part of the Collections Framework but **does not extend Collection**.

```text
                         Map
                          |
          +---------------+----------------+
          |               |                |
       HashMap        SortedMap       Hashtable
          |               |
     LinkedHashMap     TreeMap
                          |
                     NavigableMap
```

---

# 4. Iterable Interface

`Iterable` is the root interface for objects that can be iterated.

```java
public interface Iterable<T>
```

It provides:

```java
Iterator<T> iterator();
```

Therefore, objects implementing `Iterable` can be used with the enhanced `for` loop.

```java
List<String> names = List.of("A", "B", "C");

for (String name : names) {
    System.out.println(name);
}
```

Internally, enhanced `for` works approximately like:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();
    System.out.println(name);
}
```

---

# 5. Collection Interface

The `Collection` interface represents a group of objects.

Common methods:

```java
add()
addAll()
remove()
removeAll()
contains()
containsAll()
size()
isEmpty()
clear()
iterator()
toArray()
stream()
parallelStream()
removeIf()
```

Example:

```java
Collection<String> names = new ArrayList<>();

names.add("Aditya");
names.add("Rahul");
names.add("Amit");

System.out.println(names.size());
System.out.println(names.contains("Aditya"));
```

---

# 6. List Interface

A `List` represents an ordered collection.

Characteristics:

* Maintains insertion order
* Allows duplicates
* Allows positional access
* Usually allows multiple `null` values depending on implementation

Example:

```java
List<String> names = new ArrayList<>();

names.add("Aditya");
names.add("Rahul");
names.add("Aditya");

System.out.println(names);
```

Output:

```text
[Aditya, Rahul, Aditya]
```

Major implementations:

```text
ArrayList
LinkedList
Vector
Stack
CopyOnWriteArrayList
```

---

# 7. ArrayList

`ArrayList` is one of the most commonly used collection classes.

```java
List<String> names = new ArrayList<>();
```

Internally, `ArrayList` uses a **resizable array**.

Conceptually:

```text
ArrayList
   |
   +---- Object[]
           |
           +---- element 0
           +---- element 1
           +---- element 2
           +---- ...
```

---

## 7.1 ArrayList Characteristics

| Feature                   | ArrayList          |
| ------------------------- | ------------------ |
| Internal structure        | Dynamic array      |
| Maintains insertion order | Yes                |
| Allows duplicates         | Yes                |
| Allows null               | Yes                |
| Random access             | Fast               |
| Thread-safe               | No                 |
| Implements                | List, RandomAccess |

---

# 8. ArrayList Internal Working

When an `ArrayList` is created:

```java
ArrayList<String> list = new ArrayList<>();
```

The list manages an internal array.

When elements are added:

```java
list.add("A");
list.add("B");
list.add("C");
```

Conceptually:

```text
[A, B, C, _, _, _, ...]
```

When capacity becomes insufficient, the internal array is resized and elements are copied into the new array.

Therefore, insertion at the end is generally:

```text
O(1) amortized
```

But resizing can temporarily require:

```text
O(n)
```

---

# 9. ArrayList Random Access

Because elements are stored in an array:

```java
list.get(5);
```

can directly calculate the array location.

Therefore:

```text
get(index) = O(1)
```

Example:

```java
String value = list.get(3);
```

---

# 10. ArrayList Insertion

### Add at end

```java
list.add("Java");
```

Usually:

```text
O(1) amortized
```

### Add at beginning

```java
list.add(0, "Java");
```

Existing elements must be shifted.

```text
Before:

[A, B, C, D]

After adding X at index 0:

[X, A, B, C, D]
```

Complexity:

```text
O(n)
```

---

# 11. ArrayList Removal

Removing by index:

```java
list.remove(2);
```

Elements after the removed element must shift.

```text
[A, B, C, D, E]

remove(2)

[A, B, D, E]
```

Complexity:

```text
O(n)
```

Removing the last element is generally:

```text
O(1)
```

---

# 12. LinkedList

`LinkedList` implements both:

```java
List
Deque
```

It is internally based on a **doubly linked list**.

Conceptually:

```text
null
  |
  v
[A] <-> [B] <-> [C] <-> [D]
                         |
                        null
```

Each node contains:

```text
previous
element
next
```

Conceptually:

```java
class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

---

# 13. LinkedList Characteristics

| Feature                     | LinkedList         |
| --------------------------- | ------------------ |
| Internal structure          | Doubly linked list |
| Maintains insertion order   | Yes                |
| Allows duplicates           | Yes                |
| Allows null                 | Yes                |
| Random access               | Slow               |
| Insert/remove at known node | Fast               |
| Thread-safe                 | No                 |

---

# 14. LinkedList Random Access

Consider:

```java
list.get(50000);
```

A linked list cannot directly jump to index 50000.

It must traverse nodes.

Therefore:

```text
get(index) = O(n)
```

The implementation may traverse from the beginning or end depending on which is closer.

---

# 15. ArrayList vs LinkedList

| Operation         |      ArrayList | LinkedList |
| ----------------- | -------------: | ---------: |
| get(index)        |           O(1) |       O(n) |
| add(end)          | O(1) amortized |       O(1) |
| add(beginning)    |           O(n) |       O(1) |
| remove(end)       |           O(1) |       O(1) |
| remove(beginning) |           O(n) |       O(1) |
| Search            |           O(n) |       O(n) |
| Memory overhead   |          Lower |     Higher |

### Important Interview Point

Do not automatically say:

> "LinkedList is better for insertion and deletion."

The position must already be known.

For:

```java
list.add(50000, value);
```

finding the position in a `LinkedList` still takes traversal time.

---

# 16. Vector

`Vector` is a legacy collection.

```java
Vector<String> vector = new Vector<>();
```

It is synchronized.

Therefore, most individual operations have synchronization overhead.

Modern applications generally prefer:

```java
ArrayList
```

unless a specific legacy API requires `Vector`.

---

# 17. Stack

`Stack` extends `Vector`.

It represents a LIFO structure.

```text
LIFO = Last In First Out
```

Example:

```java
Stack<Integer> stack = new Stack<>();

stack.push(10);
stack.push(20);
stack.push(30);
```

Conceptually:

```text
30 <- top
20
10
```

Then:

```java
stack.pop();
```

returns:

```text
30
```

### Modern Alternative

Prefer:

```java
Deque<Integer> stack = new ArrayDeque<>();
```

with:

```java
stack.push(10);
stack.pop();
stack.peek();
```

---

# 18. Set Interface

A `Set` represents a collection that does not permit duplicate elements.

Example:

```java
Set<String> names = new HashSet<>();

names.add("Java");
names.add("Spring");
names.add("Java");

System.out.println(names);
```

Only one `"Java"` is stored.

Major implementations:

```text
HashSet
LinkedHashSet
TreeSet
EnumSet
CopyOnWriteArraySet
```

---

# 19. HashSet

`HashSet` is based on hashing.

```java
Set<String> set = new HashSet<>();
```

Characteristics:

* No duplicates
* No guaranteed iteration order
* Allows one `null`
* Fast average insertion/search/removal
* Not thread-safe

Typical complexity:

```text
add()      -> O(1) average
remove()   -> O(1) average
contains() -> O(1) average
```

Worst-case behavior depends on hash distribution and implementation details.

---

# 20. HashSet Internal Working

Consider:

```java
Set<String> set = new HashSet<>();

set.add("Java");
```

HashSet internally uses a `HashMap`.

Conceptually:

```java
HashSet<E>
       |
       v
HashMap<E, Object>
```

The set element becomes a key in the internal map.

Conceptually:

```text
Java -> PRESENT
Spring -> PRESENT
Kafka -> PRESENT
```

---

# 21. Hashing

When an object is inserted into a hash-based collection:

```java
set.add(object);
```

the collection obtains a hash value.

Conceptually:

```text
object
   |
hashCode()
   |
hash value
   |
bucket index
```

The bucket determines where the entry is stored.

---

# 22. hashCode() and equals()

Hash-based collections depend heavily on:

```java
hashCode()
equals()
```

The contract is:

> If two objects are equal according to `equals()`, they must return the same hash code.

Example:

```java
class Employee {

    private int id;
    private String name;

    @Override
    public boolean equals(Object obj) {
        // comparison logic
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

---

# 23. Why equals() and hashCode() Must Both Be Overridden

Consider:

```java
Set<Employee> employees = new HashSet<>();
```

If `equals()` and `hashCode()` are inconsistent, duplicate logical objects may be stored.

Incorrect:

```java
@Override
public boolean equals(Object obj) {
    return this.id == ((Employee) obj).id;
}
```

but no corresponding `hashCode()` implementation.

This violates the contract.

Correct:

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) {
        return true;
    }

    if (!(obj instanceof Employee other)) {
        return false;
    }

    return id == other.id;
}

@Override
public int hashCode() {
    return Integer.hashCode(id);
}
```

---

# 24. LinkedHashSet

`LinkedHashSet` combines:

```text
HashSet + Linked List ordering
```

It maintains insertion order.

Example:

```java
Set<String> set = new LinkedHashSet<>();

set.add("C");
set.add("A");
set.add("B");

System.out.println(set);
```

Output:

```text
[C, A, B]
```

---

# 25. TreeSet

`TreeSet` stores elements in sorted order.

```java
Set<Integer> numbers = new TreeSet<>();

numbers.add(50);
numbers.add(10);
numbers.add(30);
numbers.add(20);
```

Result:

```text
[10, 20, 30, 50]
```

Internally it is based on a tree structure.

Java's implementation uses a `TreeMap`-based balanced tree structure.

Typical complexity:

```text
add()      -> O(log n)
remove()   -> O(log n)
contains() -> O(log n)
```

---

# 26. TreeSet and Comparable

For custom objects, objects need a way to determine ordering.

Example:

```java
class Employee implements Comparable<Employee> {

    private int id;

    @Override
    public int compareTo(Employee other) {
        return Integer.compare(this.id, other.id);
    }
}
```

Then:

```java
TreeSet<Employee> employees = new TreeSet<>();
```

uses `compareTo()`.

---

# 27. TreeSet and Comparator

Instead of implementing `Comparable`, a comparator can be supplied.

```java
TreeSet<Employee> employees =
        new TreeSet<>(Comparator.comparing(Employee::getName));
```

This is useful when multiple sorting strategies are required.

---

# 28. List vs Set

| Feature         | List               | Set                       |
| --------------- | ------------------ | ------------------------- |
| Duplicates      | Allowed            | Not allowed               |
| Index access    | Yes                | No                        |
| Insertion order | Usually maintained | Depends on implementation |
| Main use        | Ordered data       | Unique data               |

Example:

```text
List:
[A, B, A, C]

Set:
[A, B, C]
```

---

# 29. Queue

A `Queue` is generally used for processing elements in a particular order.

Common implementations:

```text
LinkedList
PriorityQueue
ArrayDeque
```

Common methods:

```java
add()
offer()
remove()
poll()
element()
peek()
```

---

# 30. Queue Methods

There are paired methods.

### Insert

```java
add()
offer()
```

Difference:

* `add()` may throw an exception when insertion fails.
* `offer()` returns `false`.

### Remove

```java
remove()
poll()
```

Difference:

* `remove()` throws exception when empty.
* `poll()` returns `null`.

### Examine

```java
element()
peek()
```

Difference:

* `element()` throws exception when empty.
* `peek()` returns `null`.

---

# 31. PriorityQueue

`PriorityQueue` processes elements according to priority rather than insertion order.

```java
PriorityQueue<Integer> queue = new PriorityQueue<>();

queue.add(30);
queue.add(10);
queue.add(20);
```

The smallest element has highest priority under natural ordering.

```java
queue.poll();
```

returns:

```text
10
```

---

# 32. Important PriorityQueue Interview Point

Printing a `PriorityQueue` does **not** necessarily show all elements in sorted order.

For example:

```java
System.out.println(queue);
```

does not mean:

```text
[10, 20, 30]
```

The heap guarantees the priority element at the head, not complete iteration ordering.

---

# 33. Deque

`Deque` means:

```text
Double Ended Queue
```

It supports insertion and removal from both ends.

```java
Deque<Integer> deque = new ArrayDeque<>();

deque.addFirst(10);
deque.addLast(20);

deque.removeFirst();
deque.removeLast();
```

It can be used as both:

```text
Queue
Stack
```

---

# 34. ArrayDeque

`ArrayDeque` is a resizable-array implementation of `Deque`.

Example:

```java
Deque<String> deque = new ArrayDeque<>();

deque.addFirst("A");
deque.addLast("B");
```

Advantages:

* Fast operations at both ends
* Good replacement for Stack
* Good replacement for LinkedList in many queue/deque use cases
* Does not allow `null`

---

# 35. Map Interface

`Map` stores data as:

```text
key -> value
```

Example:

```java
Map<Integer, String> employees = new HashMap<>();

employees.put(101, "Aditya");
employees.put(102, "Rahul");
employees.put(103, "Amit");
```

Conceptually:

```text
101 -> Aditya
102 -> Rahul
103 -> Amit
```

Keys must be unique.

Values can be duplicated.

---

# 36. HashMap

`HashMap` is one of the most frequently used Java collections.

Characteristics:

* Key-value storage
* No guaranteed iteration order
* One `null` key
* Multiple `null` values
* Not thread-safe
* Average O(1) lookup/insertion/removal

Example:

```java
Map<String, Integer> scores = new HashMap<>();

scores.put("Java", 90);
scores.put("Spring", 85);

System.out.println(scores.get("Java"));
```

---

# 37. HashMap Internal Structure

Conceptually:

```text
HashMap
   |
   v
Bucket Array
   |
   +---- Bucket 0
   +---- Bucket 1
   +---- Bucket 2
   +---- Bucket 3
             |
             +---- Node
             +---- Node
```

A map entry conceptually contains:

```text
hash
key
value
next
```

---

# 38. HashMap put() Flow

Consider:

```java
map.put("Java", 100);
```

Conceptual process:

```text
1. Calculate hash of key
        |
        v
2. Calculate bucket/index
        |
        v
3. Check bucket
        |
        +---- Empty -> insert
        |
        +---- Occupied
                 |
                 v
          Compare keys
                 |
       +---------+---------+
       |                   |
     equal              different
       |                   |
    replace           collision handling
```

---

# 39. HashMap Collision

A collision occurs when multiple keys map to the same bucket.

Example:

```text
Key A -> bucket 5
Key B -> bucket 5
```

The collection needs to store both entries.

Modern Java HashMap implementations can use:

```text
Linked structure
```

and under certain conditions:

```text
Tree structure
```

for heavily populated buckets.

---

# 40. HashMap Treeification

Modern Java implementations can convert a heavily populated bucket into a tree structure when certain implementation thresholds are met.

This improves lookup behavior in pathological collision scenarios.

Conceptually:

```text
Before:

Bucket
 |
 A -> B -> C -> D -> E

After treeification:

        C
       / \
      A   D
           \
            E
```

The exact thresholds are implementation details and should not be hard-coded into application logic.

---

# 41. HashMap Capacity

HashMap has concepts such as:

```text
capacity
load factor
threshold
```

A common default load factor is:

```text
0.75
```

Conceptually:

```text
threshold = capacity × loadFactor
```

When the number of entries crosses the threshold, the table is resized.

---

# 42. HashMap Resizing

Suppose:

```text
capacity = 16
load factor = 0.75
```

Then the threshold is approximately:

```text
16 × 0.75 = 12
```

When resizing occurs, the internal table grows and entries are redistributed according to the new table size.

Resizing can be expensive, which is why specifying a suitable initial capacity can help when the approximate number of entries is known.

---

# 43. HashMap get() Flow

For:

```java
map.get("Java");
```

conceptually:

```text
Java
 |
hashCode()
 |
hash transformation
 |
bucket index
 |
compare candidate keys
 |
equals()
 |
value
```

Both `hashCode()` and `equals()` therefore matter.

---

# 44. Why HashMap Allows One Null Key

HashMap supports a `null` key:

```java
Map<String, Integer> map = new HashMap<>();

map.put(null, 100);
```

It can also store multiple null values:

```java
map.put("A", null);
map.put("B", null);
```

---

# 45. LinkedHashMap

`LinkedHashMap` maintains a predictable iteration order.

By default, it maintains insertion order.

```java
Map<String, Integer> map = new LinkedHashMap<>();

map.put("A", 1);
map.put("B", 2);
map.put("C", 3);
```

Iteration follows:

```text
A
B
C
```

It can also be configured for access-order behavior, which is useful in LRU-style cache implementations.

---

# 46. TreeMap

`TreeMap` stores keys in sorted order.

```java
Map<Integer, String> map = new TreeMap<>();

map.put(30, "C");
map.put(10, "A");
map.put(20, "B");
```

Iteration:

```text
10 -> A
20 -> B
30 -> C
```

Typical complexity:

```text
put()    -> O(log n)
get()    -> O(log n)
remove() -> O(log n)
```

---

# 47. TreeMap and Comparator

```java
TreeMap<String, Integer> map =
        new TreeMap<>(Comparator.reverseOrder());
```

Now keys are ordered in descending order.

---

# 48. Hashtable

`Hashtable` is a legacy synchronized map.

Characteristics:

* Thread-safe through synchronization
* Does not allow null keys
* Does not allow null values
* Legacy API

Modern applications commonly prefer:

```java
ConcurrentHashMap
```

for concurrent access.

---

# 49. Map Implementations Comparison

| Implementation    | Ordering               | Null Key                           | Typical Lookup |
| ----------------- | ---------------------- | ---------------------------------- | -------------: |
| HashMap           | None guaranteed        | Yes                                |   O(1) average |
| LinkedHashMap     | Insertion/access order | Yes                                |   O(1) average |
| TreeMap           | Sorted                 | No null key under natural ordering |       O(log n) |
| Hashtable         | None guaranteed        | No                                 |   O(1) average |
| ConcurrentHashMap | None guaranteed        | No                                 |   O(1) average |

---

# 50. Collection Complexity Cheat Sheet

| Collection    |      get |      add |   remove | contains |
| ------------- | -------: | -------: | -------: | -------: |
| ArrayList     |     O(1) |    O(1)* |     O(n) |     O(n) |
| LinkedList    |     O(n) |   O(1)** |   O(1)** |     O(n) |
| HashSet       |        - |    O(1)* |    O(1)* |    O(1)* |
| LinkedHashSet |        - |    O(1)* |    O(1)* |    O(1)* |
| TreeSet       |        - | O(log n) | O(log n) | O(log n) |
| HashMap       |    O(1)* |    O(1)* |    O(1)* |    O(1)* |
| TreeMap       | O(log n) | O(log n) | O(log n) | O(log n) |

`*` Average/amortized behavior.

`**` When the relevant linked-list position/node is already known; locating an arbitrary index can take O(n).

---

# 51. Iterator

`Iterator` provides a standard mechanism for traversing collections.

```java
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String value = iterator.next();
    System.out.println(value);
}
```

Methods:

```java
hasNext()
next()
remove()
```

---

# 52. Iterator and ConcurrentModificationException

Consider:

```java
List<String> list = new ArrayList<>();

list.add("A");
list.add("B");
list.add("C");

for (String value : list) {
    if (value.equals("B")) {
        list.remove(value);
    }
}
```

This can cause:

```text
ConcurrentModificationException
```

because the collection is structurally modified while being traversed using a fail-fast iterator.

---

# 53. Safe Removal Using Iterator

Use:

```java
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String value = iterator.next();

    if (value.equals("B")) {
        iterator.remove();
    }
}
```

The iterator's own removal operation is designed for this purpose.

---

# 54. ListIterator

`ListIterator` is specific to lists and provides bidirectional traversal.

```java
ListIterator<String> iterator = list.listIterator();
```

It supports:

```java
hasNext()
next()
hasPrevious()
previous()
add()
set()
remove()
```

Example:

```java
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}

while (iterator.hasPrevious()) {
    System.out.println(iterator.previous());
}
```

---

# 55. Fail-Fast vs Fail-Safe

### Fail-fast

Common examples include iterators from:

```text
ArrayList
HashMap
HashSet
```

Structural modification outside the iterator may result in:

```text
ConcurrentModificationException
```

### Concurrent collections

Collections such as:

```text
CopyOnWriteArrayList
ConcurrentHashMap
```

provide different concurrency semantics and do not behave like ordinary fail-fast collections.

The terms "fail-fast" and "fail-safe" are informal interview terminology; Java's API documentation generally describes the precise iterator behavior instead.

---

# 56. Sorting Collections

Collections can be sorted using:

```java
Collections.sort()
```

Example:

```java
List<Integer> numbers =
        new ArrayList<>(List.of(50, 10, 30, 20));

Collections.sort(numbers);

System.out.println(numbers);
```

Output:

```text
[10, 20, 30, 50]
```

Modern Java can also use:

```java
numbers.sort(Integer::compareTo);
```

---

# 57. Comparable

`Comparable` defines the object's natural ordering.

```java
class Employee implements Comparable<Employee> {

    private int id;

    @Override
    public int compareTo(Employee other) {
        return Integer.compare(this.id, other.id);
    }
}
```

Then:

```java
Collections.sort(employees);
```

uses `compareTo()`.

---

# 58. Comparator

`Comparator` defines external/custom ordering.

```java
Comparator<Employee> byName =
        Comparator.comparing(Employee::getName);
```

Then:

```java
employees.sort(byName);
```

---

# 59. Comparable vs Comparator

| Comparable                   | Comparator                    |
| ---------------------------- | ----------------------------- |
| `java.lang`                  | `java.util`                   |
| `compareTo()`                | `compare()`                   |
| Defines natural ordering     | Defines custom ordering       |
| Usually implemented by class | Usually separate object       |
| One primary natural ordering | Multiple comparators possible |

Example:

```java
employees.sort(Comparator.comparing(Employee::getName));

employees.sort(Comparator.comparing(Employee::getSalary));
```

---

# 60. Comparator Chaining

Java allows comparator chaining.

```java
Comparator<Employee> comparator =
        Comparator.comparing(Employee::getDepartment)
                  .thenComparing(Employee::getName)
                  .thenComparing(Employee::getSalary);
```

This means:

```text
1. Sort by department
2. If equal, sort by name
3. If equal, sort by salary
```

---

# 61. Reverse Sorting

```java
numbers.sort(Comparator.reverseOrder());
```

For objects:

```java
employees.sort(
    Comparator.comparing(Employee::getSalary).reversed()
);
```

---

# 62. Collections Utility Class

`Collections` is a utility class containing algorithms for collections.

Common methods:

```java
sort()
reverse()
shuffle()
binarySearch()
min()
max()
frequency()
copy()
fill()
swap()
rotate()
```

Example:

```java
Collections.reverse(list);
```

---

# 63. Arrays vs Collections

| Arrays               | Collections     |
| -------------------- | --------------- |
| Fixed size           | Usually dynamic |
| Can store primitives | Store objects   |
| Less abstraction     | Rich APIs       |
| Simple               | More flexible   |
| `.length`            | `.size()`       |

Important:

```java
int[] numbers = new int[5];
```

can store primitives directly.

But:

```java
List<Integer>
```

stores `Integer` objects.

Autoboxing converts:

```java
int -> Integer
```

when required.

---

# 64. Generics in Collections

Generics provide compile-time type safety.

Without generics:

```java
List list = new ArrayList();

list.add("Java");
list.add(100);
```

Potential runtime problems can occur.

With generics:

```java
List<String> list = new ArrayList<>();

list.add("Java");
```

The compiler prevents:

```java
list.add(100);
```

---

# 65. Collection of Custom Objects

Example:

```java
class Employee {

    private int id;
    private String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Usage:

```java
List<Employee> employees = new ArrayList<>();

employees.add(new Employee(101, "Aditya"));
employees.add(new Employee(102, "Rahul"));
```

---

# 66. Unmodifiable Collections

Java provides factory methods:

```java
List.of()
Set.of()
Map.of()
```

Example:

```java
List<String> names =
        List.of("Java", "Spring", "Kafka");
```

Attempting:

```java
names.add("Docker");
```

throws:

```text
UnsupportedOperationException
```

These collections are unmodifiable.

---

# 67. `List.copyOf()`

Example:

```java
List<String> copy = List.copyOf(names);
```

This creates an unmodifiable list containing the elements.

Similarly:

```java
Set.copyOf()
Map.copyOf()
```

are available.

---

# 68. `Collections.unmodifiableList()`

Example:

```java
List<String> original =
        new ArrayList<>();

List<String> readOnly =
        Collections.unmodifiableList(original);
```

The wrapper prevents modification through `readOnly`, but the underlying `original` can still change.

This differs conceptually from creating an independent immutable snapshot.

---

# 69. Immutable vs Unmodifiable

### Unmodifiable

A reference cannot modify the collection through that view.

```java
Collections.unmodifiableList(list);
```

But the underlying list may change.

### Immutable

The collection's state cannot be changed through its API.

Examples:

```java
List.of()
Set.of()
Map.of()
```

---

# 70. `removeIf()`

Java 8 introduced convenient collection operations.

```java
List<Integer> numbers =
        new ArrayList<>(List.of(10, 15, 20, 25));

numbers.removeIf(n -> n % 2 == 0);
```

Result:

```text
[15, 25]
```

---

# 71. forEach()

Collections support `forEach()`.

```java
list.forEach(System.out::println);
```

Equivalent conceptually to iterating over elements.

---

# 72. Stream API and Collections

Collections integrate closely with streams.

Example:

```java
List<String> names =
        List.of("Aditya", "Rahul", "Amit");

List<String> result =
        names.stream()
             .filter(name -> name.startsWith("A"))
             .toList();
```

Result:

```text
[Aditya, Amit]
```

---

# 73. Converting Collection to Stream

```java
List<Integer> numbers =
        List.of(10, 20, 30);

numbers.stream()
       .filter(n -> n > 15)
       .forEach(System.out::println);
```

---

# 74. Converting Stream to Collection

Modern Java:

```java
List<Integer> result =
        numbers.stream()
               .filter(n -> n > 10)
               .toList();
```

Traditional collector:

```java
List<Integer> result =
        numbers.stream()
               .filter(n -> n > 10)
               .collect(Collectors.toList());
```

---

# 75. Map Operations

Important methods:

```java
put()
putIfAbsent()
get()
getOrDefault()
containsKey()
containsValue()
remove()
replace()
replaceAll()
compute()
computeIfAbsent()
computeIfPresent()
merge()
```

---

# 76. getOrDefault()

```java
Map<String, Integer> scores = new HashMap<>();

scores.put("Java", 90);

int value =
        scores.getOrDefault("Spring", 0);
```

Result:

```text
0
```

---

# 77. putIfAbsent()

```java
map.putIfAbsent("Java", 100);
```

If the key already exists, its value is not replaced.

---

# 78. computeIfAbsent()

Very useful for grouping.

```java
Map<String, List<String>> map =
        new HashMap<>();

map.computeIfAbsent("Java",
        key -> new ArrayList<>())
   .add("Spring");
```

This avoids manual null checks.

---

# 79. merge()

Example:

```java
Map<String, Integer> counts =
        new HashMap<>();

counts.merge("Java", 1, Integer::sum);
counts.merge("Java", 1, Integer::sum);
```

Result:

```text
Java -> 2
```

Useful for frequency counting.

---

# 80. Frequency Counting

Example:

```java
String text = "java spring java kafka java";

Map<String, Integer> frequency =
        new HashMap<>();

for (String word : text.split(" ")) {
    frequency.merge(word, 1, Integer::sum);
}
```

Result:

```text
java -> 3
spring -> 1
kafka -> 1
```

---

# 81. EntrySet

When iterating over a map, prefer `entrySet()` when both key and value are required.

```java
for (Map.Entry<String, Integer> entry :
        map.entrySet()) {

    System.out.println(
        entry.getKey() + " = " + entry.getValue()
    );
}
```

This avoids an additional lookup such as:

```java
map.get(key)
```

for every entry.

---

# 82. keySet()

Use when only keys are needed.

```java
for (String key : map.keySet()) {
    System.out.println(key);
}
```

---

# 83. values()

Use when only values are needed.

```java
for (Integer value : map.values()) {
    System.out.println(value);
}
```

---

# 84. Concurrent Collections

Normal collections such as:

```java
ArrayList
HashMap
HashSet
```

are not thread-safe.

For concurrent applications Java provides:

```text
ConcurrentHashMap
CopyOnWriteArrayList
BlockingQueue
ConcurrentLinkedQueue
ConcurrentSkipListMap
ConcurrentSkipListSet
```

---

# 85. ConcurrentHashMap

`ConcurrentHashMap` is designed for concurrent access.

```java
Map<String, Integer> map =
        new ConcurrentHashMap<>();
```

Important:

```java
ConcurrentHashMap
```

does not allow:

```text
null key
null value
```

It supports concurrent reads and updates without synchronizing the entire map for ordinary operations.

---

# 86. Synchronized Collections

Java provides wrappers:

```java
Collections.synchronizedList()
Collections.synchronizedSet()
Collections.synchronizedMap()
```

Example:

```java
List<String> list =
        Collections.synchronizedList(
            new ArrayList<>()
        );
```

However, iteration may still require external synchronization according to the API contract.

Example pattern:

```java
synchronized (list) {
    Iterator<String> iterator =
            list.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
}
```

---

# 87. CopyOnWriteArrayList

`CopyOnWriteArrayList` is useful when:

```text
Reads are frequent
Writes are rare
```

When the list is modified, a new underlying array is created.

Example:

```java
CopyOnWriteArrayList<String> list =
        new CopyOnWriteArrayList<>();

list.add("Java");
list.add("Spring");
```

It provides snapshot-style iteration behavior.

Common use cases:

* Listener lists
* Configuration snapshots
* Read-heavy collections

---

# 88. BlockingQueue

`BlockingQueue` is useful for producer-consumer systems.

Implementations include:

```text
ArrayBlockingQueue
LinkedBlockingQueue
PriorityBlockingQueue
DelayQueue
SynchronousQueue
```

Example:

```java
BlockingQueue<String> queue =
        new LinkedBlockingQueue<>();

queue.put("Task-1");

String task = queue.take();
```

`take()` waits if the queue is empty.

---

# 89. Producer-Consumer Model

```text
Producer
   |
   | put()
   v
+----------------+
| BlockingQueue  |
+----------------+
   |
   | take()
   v
Consumer
```

This is commonly used in multithreaded systems.

---

# 90. Queue vs Stack

### Queue

Usually:

```text
FIFO
First In First Out
```

Example:

```text
A -> B -> C

remove -> A
```

### Stack

Usually:

```text
LIFO
Last In First Out
```

Example:

```text
A
B
C <- top

remove -> C
```

---

# 91. EnumSet

`EnumSet` is specialized for enum values.

Example:

```java
enum Permission {
    READ,
    WRITE,
    DELETE
}
```

Usage:

```java
EnumSet<Permission> permissions =
        EnumSet.of(
            Permission.READ,
            Permission.WRITE
        );
```

It is highly efficient for enum sets.

---

# 92. EnumMap

`EnumMap` is specialized for enum keys.

```java
EnumMap<Permission, String> descriptions =
        new EnumMap<>(Permission.class);
```

It is generally more efficient than a general-purpose map when keys are enums.

---

# 93. WeakHashMap

`WeakHashMap` uses weak references for keys.

This can be useful when entries should not keep keys alive solely because they are present in the map.

Example use cases can include:

* Certain caches
* Metadata associated with objects
* Memory-sensitive mappings

It should not be treated as a general-purpose cache without understanding garbage-collection behavior.

---

# 94. IdentityHashMap

`IdentityHashMap` compares keys using reference identity:

```java
==
```

instead of normal logical equality:

```java
equals()
```

This makes it suitable for specialized algorithms where object identity matters.

---

# 95. NavigableSet

`NavigableSet` extends `SortedSet`.

It provides navigation operations:

```java
lower()
floor()
ceiling()
higher()
pollFirst()
pollLast()
descendingSet()
```

Example:

```java
NavigableSet<Integer> set =
        new TreeSet<>();

set.add(10);
set.add(20);
set.add(30);
```

Then:

```java
set.floor(25);   // 20
set.ceiling(25); // 30
```

---

# 96. NavigableMap

`NavigableMap` extends `SortedMap`.

Methods include:

```java
lowerKey()
floorKey()
ceilingKey()
higherKey()
firstKey()
lastKey()
pollFirstEntry()
pollLastEntry()
```

Usually implemented by:

```java
TreeMap
```

---

# 97. Collection Views

Maps provide views:

```java
keySet()
values()
entrySet()
```

These are backed by the map.

Example:

```java
Map<String, Integer> map =
        new HashMap<>();

map.put("A", 1);

Set<String> keys = map.keySet();
```

Changes to the map can be reflected in the view.

---

# 98. Shallow Copy vs Deep Copy

Consider:

```java
List<Employee> list1 =
        new ArrayList<>();

List<Employee> list2 =
        new ArrayList<>(list1);
```

The list structure is copied, but the `Employee` objects themselves are not necessarily cloned.

Conceptually:

```text
list1 -----> Employee A
              ^
list2 --------|
```

Both lists can reference the same objects.

---

# 99. Collection vs Collections

This is a very common interview question.

### Collection

Interface:

```java
java.util.Collection
```

Represents a group of objects.

### Collections

Utility class:

```java
java.util.Collections
```

Provides algorithms and wrappers.

Example:

```java
Collections.sort(list);
Collections.reverse(list);
```

---

# 100. Collection vs Collections vs Collectors

### Collection

```java
Collection<T>
```

Interface.

### Collections

```java
Collections
```

Utility class.

### Collectors

```java
Collectors
```

Utility class used primarily with Stream API.

Example:

```java
stream.collect(Collectors.toList());
```

---

# 101. HashMap vs Hashtable

| HashMap             | Hashtable                   |
| ------------------- | --------------------------- |
| Not synchronized    | Synchronized                |
| Allows null key     | Does not allow null         |
| Allows null values  | Does not allow null         |
| Modern              | Legacy                      |
| Generally preferred | Usually avoided in new code |

---

# 102. HashMap vs ConcurrentHashMap

| HashMap                                  | ConcurrentHashMap                     |
| ---------------------------------------- | ------------------------------------- |
| Not thread-safe                          | Designed for concurrent use           |
| Allows null key/value                    | Does not allow null                   |
| Suitable for single-threaded/general use | Suitable for shared concurrent access |
| Can require external synchronization     | Provides concurrent operations        |

---

# 103. ArrayList vs Vector

| ArrayList                              | Vector                               |
| -------------------------------------- | ------------------------------------ |
| Not synchronized                       | Synchronized                         |
| Modern                                 | Legacy                               |
| Faster in ordinary single-threaded use | Synchronization overhead             |
| Preferred generally                    | Used mainly for legacy compatibility |

---

# 104. HashSet vs TreeSet

| HashSet                    | TreeSet                                          |
| -------------------------- | ------------------------------------------------ |
| Hash-based                 | Tree-based                                       |
| No guaranteed sorted order | Sorted                                           |
| O(1) average operations    | O(log n)                                         |
| Allows one null            | Natural ordering generally does not support null |
| Faster average lookup      | Supports navigation/sorting                      |

---

# 105. HashMap vs TreeMap

| HashMap                | TreeMap                                        |
| ---------------------- | ---------------------------------------------- |
| Hash-based             | Tree-based                                     |
| No guaranteed ordering | Sorted by key                                  |
| O(1) average           | O(log n)                                       |
| Supports null key      | Natural-order TreeMap does not permit null key |
| Faster average lookup  | Provides sorted-map operations                 |

---

# 106. ArrayDeque vs Stack

Prefer:

```java
Deque<Integer> stack =
        new ArrayDeque<>();
```

instead of:

```java
Stack<Integer> stack =
        new Stack<>();
```

Use:

```java
stack.push(10);
stack.pop();
stack.peek();
```

`ArrayDeque` is generally the modern choice for stack semantics.

---

# 107. Choosing the Correct Collection

### Need indexed access?

Use:

```text
ArrayList
```

### Need uniqueness?

Use:

```text
HashSet
```

### Need uniqueness + insertion order?

Use:

```text
LinkedHashSet
```

### Need sorted unique elements?

Use:

```text
TreeSet
```

### Need key-value storage?

Use:

```text
HashMap
```

### Need key-value storage + insertion order?

Use:

```text
LinkedHashMap
```

### Need sorted keys?

Use:

```text
TreeMap
```

### Need queue?

Use:

```text
ArrayDeque
```

or an appropriate queue implementation.

### Need priority-based processing?

Use:

```text
PriorityQueue
```

### Need concurrent map?

Use:

```text
ConcurrentHashMap
```

### Need producer-consumer?

Use:

```text
BlockingQueue
```

---

# 108. Common Collection Interview Questions

## Q1. What is Java Collections Framework?

It is a unified architecture of interfaces, implementations, and algorithms for storing and manipulating groups of objects.

---

## Q2. Difference between Collection and Collections?

`Collection` is an interface.

`Collections` is a utility class.

---

## Q3. Is Map a Collection?

No.

`Map` does not extend `Collection`.

It belongs to the Java Collections Framework but represents key-value mappings.

---

## Q4. Difference between List and Set?

List allows duplicates and supports positional access.

Set does not allow duplicate elements.

---

## Q5. Why is ArrayList fast for get()?

Because it internally uses an array and can directly access an element by index.

---

## Q6. Why is LinkedList get() slower?

Because it must traverse the linked nodes to locate an arbitrary index.

---

## Q7. How does HashMap work internally?

Conceptually:

```text
key
 ↓
hashCode()
 ↓
hash
 ↓
bucket
 ↓
key comparison using equals()
 ↓
value
```

---

## Q8. Why override hashCode() when overriding equals()?

Because hash-based collections use the hash code to locate candidate entries.

Equal objects must have equal hash codes.

---

## Q9. Can HashMap have duplicate keys?

No.

Calling:

```java
map.put("Java", 10);
map.put("Java", 20);
```

replaces the previous value.

Result:

```text
Java -> 20
```

---

## Q10. Can HashMap have duplicate values?

Yes.

```java
map.put("A", 10);
map.put("B", 10);
```

is valid.

---

## Q11. Can HashSet contain duplicates?

No.

Duplicate elements are rejected according to equality semantics.

---

## Q12. Does HashSet maintain insertion order?

No.

If insertion order is required, use:

```java
LinkedHashSet
```

---

## Q13. Does HashMap maintain insertion order?

No guaranteed insertion order.

If predictable insertion order is required:

```java
LinkedHashMap
```

---

## Q14. Which collection is best for fast lookup?

For typical general-purpose use:

```text
HashMap
HashSet
```

provide average O(1) lookup.

---

## Q15. Which collection maintains sorted order?

Examples:

```text
TreeSet
TreeMap
```

---

# 109. Important Interview Coding Problems

## Problem 1: Remove Duplicates

```java
List<Integer> numbers =
        List.of(10, 20, 10, 30, 20);

Set<Integer> unique =
        new LinkedHashSet<>(numbers);

System.out.println(unique);
```

Output:

```text
[10, 20, 30]
```

Using `LinkedHashSet` preserves first-seen order.

---

# 110. Find Duplicate Elements

```java
List<Integer> numbers =
        List.of(10, 20, 10, 30, 20, 40);

Set<Integer> seen = new HashSet<>();
Set<Integer> duplicates =
        new HashSet<>();

for (Integer number : numbers) {

    if (!seen.add(number)) {
        duplicates.add(number);
    }
}
```

Result:

```text
[10, 20]
```

---

# 111. Frequency of Elements

```java
List<String> words =
        List.of(
            "Java",
            "Spring",
            "Java",
            "Kafka",
            "Java"
        );

Map<String, Integer> frequency =
        new HashMap<>();

for (String word : words) {
    frequency.merge(
        word,
        1,
        Integer::sum
    );
}
```

Result:

```text
Java   -> 3
Spring -> 1
Kafka  -> 1
```

---

# 112. Find First Non-Repeated Character

```java
String input = "swiss";

Map<Character, Integer> frequency =
        new LinkedHashMap<>();

for (char c : input.toCharArray()) {
    frequency.merge(c, 1, Integer::sum);
}

for (Map.Entry<Character, Integer> entry :
        frequency.entrySet()) {

    if (entry.getValue() == 1) {
        System.out.println(entry.getKey());
        break;
    }
}
```

Output:

```text
w
```

`LinkedHashMap` is important because it preserves insertion order.

---

# 113. Sort Map by Value

```java
Map<String, Integer> map =
        new HashMap<>();

map.put("Java", 90);
map.put("Spring", 70);
map.put("Kafka", 80);

List<Map.Entry<String, Integer>> entries =
        new ArrayList<>(map.entrySet());

entries.sort(
    Map.Entry.comparingByValue()
);
```

---

# 114. Sort Employees by Salary

```java
employees.sort(
    Comparator.comparing(Employee::getSalary)
);
```

Descending:

```java
employees.sort(
    Comparator.comparing(Employee::getSalary)
              .reversed()
);
```

---

# 115. Group Objects Using Collectors

Example:

```java
Map<String, List<Employee>> employeesByDepartment =
    employees.stream()
             .collect(
                 Collectors.groupingBy(
                     Employee::getDepartment
                 )
             );
```

Result:

```text
IT -> [Employee1, Employee2]
HR -> [Employee3]
```

---

# 116. Partitioning

```java
Map<Boolean, List<Employee>> result =
    employees.stream()
             .collect(
                 Collectors.partitioningBy(
                     e -> e.getSalary() > 100000
                 )
             );
```

This creates two groups:

```text
true  -> salary > 100000
false -> salary <= 100000
```

---

# 117. Collection Pipeline

A common Java backend pattern is:

```text
Database
   |
Repository
   |
List<Entity>
   |
Stream
   |
filter
   |
map
   |
sorted
   |
collect
   |
List<DTO>
```

Example:

```java
List<EmployeeDto> result =
    employees.stream()
             .filter(e -> e.getSalary() > 50000)
             .sorted(
                 Comparator.comparing(
                     Employee::getName
                 )
             )
             .map(this::toDto)
             .toList();
```

---

# 118. Collections in Spring Boot Applications

Collections are heavily used in backend development.

Examples:

```java
List<UserDto>
Set<String>
Map<String, Object>
Map<Long, User>
List<Order>
Set<Role>
```

Spring REST APIs frequently return:

```java
List<UserDto>
```

Example:

```java
@GetMapping("/users")
public List<UserDto> getUsers() {
    return userService.getUsers();
}
```

---

# 119. Collections in JPA/Hibernate

Typical entity relationships:

```java
@OneToMany
private List<Order> orders;
```

or:

```java
@ManyToMany
private Set<Role> roles;
```

Choice depends on the semantics of the relationship.

For example:

```text
User -> Roles
```

often uses a `Set` because duplicate roles generally have no meaning.

---

# 120. List vs Set in Entity Relationships

If ordering and duplicates are meaningful:

```java
List<Order>
```

If uniqueness is important:

```java
Set<Role>
```

However, persistence behavior, ordering, fetching, and equality/hashCode implementation must also be considered in JPA entities.

---

# 121. Common Mistakes

### Mistake 1

Using:

```java
LinkedList
```

because "insertions are fast."

The location still needs to be found.

---

### Mistake 2

Using `HashSet` and expecting insertion order.

Use:

```java
LinkedHashSet
```

if insertion order matters.

---

### Mistake 3

Using `TreeSet` without understanding ordering.

For custom objects, provide:

```java
Comparable
```

or:

```java
Comparator
```

---

### Mistake 4

Overriding `equals()` without `hashCode()`.

This can break hash-based collections.

---

### Mistake 5

Using mutable fields involved in `hashCode()` as keys.

If a key changes after insertion, lookup behavior can become incorrect.

---

# 122. Mutable HashMap Key Problem

Bad design:

```java
class Employee {

    private String id;

    // setters/getters
}
```

Suppose `id` participates in:

```java
hashCode()
```

Then:

```java
map.put(employee, "Data");
```

If later:

```java
employee.setId("NEW-ID");
```

the object's hash code can change.

The map may no longer find the entry using the mutated key.

Therefore, keys should ideally have stable equality/hash-code state while stored in hash-based collections.

---

# 123. Collection Thread Safety Summary

```text
ArrayList              -> Not thread-safe
LinkedList             -> Not thread-safe
HashSet                -> Not thread-safe
LinkedHashSet          -> Not thread-safe
TreeSet                -> Not thread-safe

HashMap                -> Not thread-safe
LinkedHashMap          -> Not thread-safe
TreeMap                -> Not thread-safe

Vector                 -> Synchronized legacy collection
Hashtable              -> Synchronized legacy map

ConcurrentHashMap      -> Concurrent
CopyOnWriteArrayList    -> Concurrent
BlockingQueue          -> Concurrent
```

---

# 124. Big-O Quick Reference

```text
ArrayList

get(index)       O(1)
search           O(n)
add(end)         O(1) amortized
insert(begin)    O(n)
remove(index)    O(n)


LinkedList

get(index)       O(n)
search           O(n)
add/remove ends  O(1)
arbitrary index  O(n)


HashMap

put              O(1) average
get              O(1) average
remove           O(1) average
containsKey      O(1) average


TreeMap

put              O(log n)
get              O(log n)
remove           O(log n)


HashSet

add              O(1) average
contains         O(1) average
remove           O(1) average


TreeSet

add              O(log n)
contains         O(log n)
remove           O(log n)
```

---

# 125. Decision Tree

```text
Do you need key-value pairs?
        |
       YES
        |
        +---- Need sorted keys?
        |          |
        |         YES -> TreeMap
        |
        +---- Need insertion order?
        |          |
        |         YES -> LinkedHashMap
        |
        +---- Otherwise -> HashMap


Need unique elements?
        |
       YES
        |
        +---- Sorted?
        |       |
        |      YES -> TreeSet
        |
        +---- Insertion order?
        |       |
        |      YES -> LinkedHashSet
        |
        +---- Otherwise -> HashSet


Need ordered/indexed elements?
        |
       YES
        |
        +---- Frequent random access?
        |       |
        |      YES -> ArrayList
        |
        +---- Need deque operations?
                |
               YES -> ArrayDeque


Need concurrent access?
        |
       YES
        |
        +---- Map -> ConcurrentHashMap
        |
        +---- Producer/Consumer -> BlockingQueue
        |
        +---- Read-heavy List -> CopyOnWriteArrayList
```

---

# 126. Most Important Interview Topics

For Java interviews, master these topics deeply:

1. Collection vs Collections
2. Collection hierarchy
3. List vs Set vs Queue vs Map
4. ArrayList internal working
5. LinkedList internal working
6. ArrayList vs LinkedList
7. HashSet internal working
8. HashMap internal working
9. HashMap collision
10. `hashCode()` and `equals()`
11. HashMap resizing
12. HashMap load factor
13. TreeSet
14. TreeMap
15. Comparable vs Comparator
16. Iterator
17. ListIterator
18. ConcurrentModificationException
19. Fail-fast behavior
20. ConcurrentHashMap
21. CopyOnWriteArrayList
22. BlockingQueue
23. PriorityQueue
24. ArrayDeque
25. Immutable/unmodifiable collections
26. Collections utility methods
27. Stream + Collection integration
28. `computeIfAbsent()`
29. `merge()`
30. `removeIf()`
31. Collection time complexity
32. Choosing the correct collection for a use case

---

# 127. Advanced Interview Questions

### Q1. Why is HashMap average O(1)?

Because hashing generally maps keys to buckets, allowing direct access to the relevant bucket.

---

### Q2. What happens when two keys have the same hash code?

They collide and are stored in the same bucket. The implementation then distinguishes keys using equality comparison.

---

### Q3. Can two unequal objects have the same hash code?

Yes.

This is called a hash collision.

```text
A.hashCode() == B.hashCode()
```

does not imply:

```text
A.equals(B)
```

---

### Q4. If two objects are equal, can their hash codes differ?

No.

The contract requires:

```text
A.equals(B) == true
```

to imply:

```text
A.hashCode() == B.hashCode()
```

---

### Q5. Why doesn't Map extend Collection?

Because the abstraction is fundamentally different.

`Collection` represents individual elements.

`Map` represents mappings:

```text
key -> value
```

---

### Q6. Why does TreeSet sometimes appear to remove "duplicates" even when equals() says objects are different?

Because TreeSet determines uniqueness according to its ordering.

If:

```java
compareTo() == 0
```

the tree considers the objects equivalent for set ordering purposes, even if `equals()` would return false.

Therefore, natural ordering should ideally be consistent with equals when using sorted sets/maps unless there is a deliberate reason otherwise.

---

### Q7. Why should we prefer interface references?

Instead of:

```java
ArrayList<String> names =
        new ArrayList<>();
```

prefer:

```java
List<String> names =
        new ArrayList<>();
```

This reduces coupling to a specific implementation.

Later:

```java
names = new LinkedList<>();
```

can be possible without changing the variable's declared type.

---

# 128. Recommended Practical Learning Order

Learn Java Collections in this sequence:

```text
1. Iterable
      |
2. Collection
      |
3. List
      |
4. ArrayList
      |
5. LinkedList
      |
6. Set
      |
7. HashSet
      |
8. LinkedHashSet
      |
9. TreeSet
      |
10. Queue
      |
11. PriorityQueue
      |
12. Deque
      |
13. ArrayDeque
      |
14. Map
      |
15. HashMap
      |
16. LinkedHashMap
      |
17. TreeMap
      |
18. Iterator
      |
19. Comparable
      |
20. Comparator
      |
21. Collections utility
      |
22. Streams + Collections
      |
23. Concurrent Collections
      |
24. Advanced Collections
```

---

# 129. Final Mental Model

The easiest way to remember Java Collections is:

```text
                         JAVA COLLECTIONS
                                |
              +-----------------+----------------+
              |                                  |
          Collection                            Map
              |                                  |
       +------+------+                    +------+------+
       |      |      |                    |      |      |
      List   Set   Queue                HashMap TreeMap
       |      |      |                    |
   ArrayList HashSet PriorityQueue    LinkedHashMap
   LinkedList   |       |
   Vector    TreeSet   ArrayDeque
   Stack
```

Think about collections according to the requirement:

```text
Index access       -> ArrayList

Unique values      -> HashSet

Unique + ordered   -> LinkedHashSet

Unique + sorted    -> TreeSet

Key-value          -> HashMap

Key-value + order  -> LinkedHashMap

Key-value + sorted -> TreeMap

Priority processing -> PriorityQueue

Stack              -> ArrayDeque

Queue/Deque        -> ArrayDeque

Concurrent Map     -> ConcurrentHashMap

Producer/Consumer  -> BlockingQueue

Read-heavy List    -> CopyOnWriteArrayList
```

The most important principle is:

> **Choose a collection based on the operations and semantics your application needs, not simply based on familiarity.**
