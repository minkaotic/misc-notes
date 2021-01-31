# LINQ Fundamentals
Source: [*LINQ Fundamentals* on Pluralsight](https://app.pluralsight.com/library/courses/linq-fundamentals-csharp-6/table-of-contents)

### Contents
- [Intro](#intro)
- [Basic Queries](#basic-queries)
- [Filtering, Sorting, Projecting](#filtering-sorting-projecting)
- [Grouping, Joining, Aggregating](#grouping-joining-aggregating)

-------------
## Intro
LINQ (**l**anguage **in**tegrated **q**uery) can be run on anything that implements `IEnumerable<T>`. There are two types of LINQ syntax:
- *query syntax* - which is heavily inspired by SQL
  ```c#
  var numbers = new List<int> { 2, 4, 8, 16, 32, 64 };
  from n in numbers where n > 10 select n;  //returns { 16, 32, 64 }
  ```
- and *method syntax* - which allows some operations not possible in query syntax
  ```c#
  var numbers = new List<int> { 2, 4, 8, 16, 32, 64 };
  numbers.Where(n => n > 10);  //returns { 16, 32, 64 }
  ```

### C# Features underpinning LINQ
#### Extension Methods
...allow us to define a static method that appears to be a member of another type. They can be used to extend classes, interfaces, structs etc.

```c#
public static class StringExtensions {
    static public double ToDouble(this string input) {
        return double.Parse(input);
    }
}
```
- the first parameter of an extension method uses `this` modifier
- the type of the first parameter is the type you're targetting to extend
- this then allows us to invoke the extension method with instance syntax:

```c#
var myString = "2.345";
var myDouble = myString.ToDouble();
```

LINQ works by creating lots of useful extension methods on the `IEnumerable<T>` interface.

#### Lambda Expressions
A lambda expression is a short, concise syntax for defining a method that can be invoked. The following examples compare 3 different ways of passing a method as a parameter to another method:
- passing a named method:
  ```c#
  IEnumerable<string> filteredList = cities.Where(StartsWithL);
  
  public bool StartsWithL(string name)
  {
    return name.StartsWith("L");
  }
  ```
- passing an anonymous method (before lambda syntax - note how verbose this is):
  ```c#
  IEnumerable<string> filteredList = cities.Where(delegate(string s) { return name.StartsWith("L"); });
  ```
- passing an anonymous method using lambda syntax:
  ```c#
  IEnumerable<string> filteredList = cities.Where(s => s.StartsWith("L"));
  ```
The concise syntax of lambda expressions made using them extensively with LINQ's methods viable - in fact they were introduced in the same release of C# as LINQ.

#### 🎁 Bonus: Funky function syntax in C#
There are 2 delegate types in C#: **`Func` and `Action`** (they are syntactically very similar, with the key difference that `Action` has no return type / is void). LINQ mostly uses `Func`! The following 3 examples are all equivalent:
```c#
// 1. Func type declaration - NB: last generic type always refers to return type!
Func<int, int> square = x => x * x;
var result = square(3);

// 2. Local function definition
int square(int x) => x * x;
var result = square(3);

// 3. Inline function invokation
var result = ((Func<int, int>)(x => x * x))(3);
```
Similar to JS, if using none or more than one parameter in the lambda expression, you need parentheses; and you can add a function body with a `return` statement for more complex logic:
```c#
Func<int, int, int> add = (x, y) =>
{
    var sum = x + y;
    return sum;
};
var result = add(3, 6);
```
> :bulb: You *can* pass more complex lambda expressions such as the above to a LINQ query!
```c#
var scotts = developers.Where(x =>
{
    var isRelevant = x.Name == "Scott";
    return isRelevant;
});
```


## Basic Queries

## Filtering, Sorting, Projecting

## Grouping, Joining, Aggregating 


## Old content - work into rest of doc
### Selecting, Projection & Anonymous Types
**Projection** is the process of projecting the results of a LINQ [`Select`](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select) query to an [anonymous type](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types) (`new {}`), i.e. `.Select(x => new { x.foo, x.bar });`

Example - given a list of objects:

```c#
var birds = new List<Bird>
  {
    new Bird ( "Cardinal", "Red", 3 ),
    new Bird ( "Dove", "White", 2 ),
    new Bird ( "Robin", "Red", 5 ),
    new Bird ( "Canary", "Yellow", 0 ),
    new Bird ( "Blue Jay", "Blue", 1 )
  };
```
Projection using query syntax:
```c#
from b in birds where b.Color=="Red" select new { b.Name, b.Color };
//returns anonymous object: { { Name = "Cardinal", Color = "Red" }, { Name = "Robin", Color = "Red" } }
```
Projection using method syntax:
```c#
birds.Where(b => b.Color=="Red").Select(b => new { b.Name, b.Color });
//returns anonymous object: { { Name = "Cardinal", Color = "Red" }, { Name = "Robin", Color = "Red" } }
```

### Ordering
Using the same list of birds as above, to order the list alphabetically:

Query syntax with `orderby`:
```c#
from b in birds orderby b.Name select b.Name;
// returns: {"Blue Jay", "Canary", "Cardinal", "Dove", "Robin"}
```
Query syntax with `orderby descending`:
```c#
from b in birds orderby b.Name descending select b.Name;
// returns: {"Robin", "Dove", "Cardinal", "Canary", "Blue Jay"}
```

Method syntax:
```c#
birds.OrderBy(b => b.Name).Select(b => b.Name);
// returns: {"Blue Jay", "Canary", "Cardinal", "Dove", "Robin"}
```
Method syntax descending:
```c#
birds.OrderBy(b => b.Name).Select(b => b.Name); // confirm - how the hell do I use Descending in method syntax?
// returns: {"Robin", "Dove", "Cardinal", "Canary", "Blue Jay"}
```
