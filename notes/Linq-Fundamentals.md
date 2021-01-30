# LINQ Fundamentals

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
- Extension Methods
- Lambda Expressions

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
