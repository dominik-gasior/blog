# Why using interfaces are worth?

Everyone knows that using interfaces help us ex. mocking, DI (Dependency injection), hiding implementation or multiple implementation. I want to focus on multiple implementations in collections.

Why using interfaces instead of classes are worth in project? Because we can use everyone collection which implementing that interface.

```C#
public class Interfaces  
{  
    public static void Run()  
    {        var obj1 = new Object1();  
        var obj2 = new Object1();  
  
        obj1.Collection1 = new List<string>();  
        obj2.Collection1 = new HashSet<string>();  
        // Now we don't get any IDE errors  
    }  
}  
  
file class Object1  
{  
    // We used ICollection interface which List and HashSet have implemented  
    public ICollection<string> Collection1;  
}
```

However sometimes use classes instead interfaces, they have important thing. When we want to focus on immutable implementation and want to specific implementation.

```C#
public class Classes  
{  
    public static void Run()  
    {        var obj1 = new Object1();  
  
        obj1.Collection1 = new List<string>();  
        obj1.Collection1 = new HashSet<string>();  
        // ERROR -> Type 'HashSet' doesn't match expected type 'List'.}  
}  
  
file class Object1  
{  
    // We used with List<T>, not interface, so we can only initialize List<T>  
    public List<string> Collection1;  
}
```

So, it depends on situation, but worth is knowing why using interfaces are worth too.