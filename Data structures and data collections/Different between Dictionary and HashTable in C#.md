# Different between Dictionary and HashTable
![](Pasted%20image%2020240703203025.png)
![](Pasted%20image%2020240703201546.png)
Above you look table which has different between these collections.
That is important that dictionary is generic collection, because it is type-safe collection.
Hashtable uses boxing/unboxing, so it is slower than dictionary.
A very important fact to know is that Dictionary implements a Hashtable internally.

`Dictionary<TKey,TValue>` represents a generic collection of keys and values. It resides in the `Systems.Collections.Generic` namespace.

`Hashtable` represents a collection of key/value pairs that are organized based on the hash code of the key. It resides in the `Systems.Collections` namespace.

**Key and Value both are of** `object` **types in** `Hashtable`.

## What are unboxing and boxing in C#:

**Boxing** is the process of converting a value type (e.g., `int`, `float`, `struct`) to a reference type (`object`).

- When a value type is boxed, a new object instance is allocated on the heap, and the value is copied into that instance.
- Boxing is implicit in C#.

```C#
int num = 123;        // Value type
object obj = num;     // Boxing: num is now an object
```

**Unboxing** is the reverse process of boxing, where a reference type is converted back to a value type.

- Unboxing requires an explicit cast in C#.
- The runtime checks the actual type of the object being unboxed, and if the types don't match, an `InvalidCastException` is thrown.

```C#
object obj = 123;     // Boxing
int num = (int)obj;   // Unboxing: obj is cast back to an int
```

A dictionary is not thread safe in during save process, because it causes lock collection. Reading is thread safe, but write not. However existing concurrent collections which are thread-safe. (ConcurrentDictionary, ImmutableDictionary).

## Similarities:

- Both are internally **hashtables** == fast access to many-item data according to key
- Both need **immutable and unique keys**
- Keys of both need own **`GetHashCode()`** method

## Alternative .NET collections:

_(candidates to use instead of Dictionary and Hashtable)_

- `ConcurrentDictionary` - **thread safe** (can be safely accessed from several threads concurrently)
- `HybridDictionary` - **optimized performance** (for few items and also for many items)
- `OrderedDictionary` - values can be **accessed via int index** (by order in which items were added)
- `SortedDictionary` - items **automatically sorted**
- `StringDictionary` - strongly typed and **optimized for strings** (now Deprecated in favor of Dictionary<string,string>)

[Why I should use TryGetValue](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.trygetvalue?view=net-8.0&redirectedfrom=MSDN#System_Collections_Generic_Dictionary_2_TryGetValue__0__1__)

> Use the [TryGetValue](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.trygetvalue?view=net-8.0) method if your code frequently attempts to access keys that are not in the dictionary. Using this method is more efficient than catching the [KeyNotFoundException](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.keynotfoundexception?view=net-8.0) thrown by the [Item[]](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.item?view=net-8.0) property.
> 
> This method approaches an O(1) operation.

![](Pasted%20image%2020240703205101.png)
[The best approach to compare two dictionaries](https://code-maze.com/csharp-compare-two-dictionaries/)
## Good practices in HashCode method in dictionary:
```C#
public class MyClass
{
    public int Property1 { get; set; }
    public string Property2 { get; set; }

    public override int GetHashCode()
    {
        unchecked
        {
            int hash = 17;
            hash = hash * 23 + Property1.GetHashCode();
            hash = hash * 23 + (Property2 != null ? Property2.GetHashCode() : 0);
            return hash;
        }
    }

    public override bool Equals(object obj)
    {
        if (obj == null || GetType() != obj.GetType())
            return false;

        MyClass other = (MyClass)obj;
        return Property1 == other.Property1 && Property2 == other.Property2;
    }
}

```

If you don't want to take any exception, you should use **unchecked**.

Tt is best practice to pick some prime numbers and multiply these with the single hash codes.

Properties of a Good HashCode:

- **Determinism**: The same hash value must be returned for the same object within the same application.
- **Distribution**: It should minimize collisions, meaning different objects should have different hash values.
- **Speed**: It should be quick to compute.

### // TODO
Immutable elements in dictionary (what is doing this?):


Source:
[Code maze](https://code-maze.com/csharp-dictionary-vs-hashtable/)
[dictionary](https://code-maze.com/dictionary-csharp/)
[dictionary thread safe](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-6.0#thread-safety)
[performance collection](https://theburningmonk.com/2011/03/hashset-vs-list-vs-dictionary/)
[Dictionary attack - brute force protector](https://medium.com/@alex.puiu/tips-for-defending-against-enumeration-attacks-in-c-fbfab54827b)
[Brute force hashcode](https://defendtheweb.net/discussion/4048-dictionary-attack)
[Dictionary Attack and Rainbow table attack](https://www.geeksforgeeks.org/rainbow-table-attack-vs-dictionary-attack/)
[HashCode](https://stackoverflow.com/questions/263400/what-is-the-best-algorithm-for-overriding-gethashcode)
