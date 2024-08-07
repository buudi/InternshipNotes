## [`List<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-8.0)

- [`manage data in arrays and collections`](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/tutorials/arrays-and-collections)

- T basically means type
- `List<T>` can be read as "a list of T" or "a list of type T"

```c#
var names = new List<string> {"Abdullah", "Yaser", "Ali"};
```
we use `var` (local type inference), since we are explicitly stating the type `List<string>` the compiler won't have trouble inferring its a List of strings, if you really want to be 100% explicit then can do the following:
```c#
List<string> names = new List<string> {"Abdullah", "Yaser", "Ali"};
```
using `var` we get rid of repetition.

using some syntactic sugar:
```c#
List<string> names = ["Abdullah", "Yaser", "Ali"];
```
#### printing the elements in the list

```c#
foreach (var name in names)
{
	Console.WriteLine($"Hello {name.ToUpper()}!");
}
```
this will give:
```bash
Hello ABDULLAH!
Hello YASER!
Hello ALI!
```

we used `foreach` cuz its simpler to use `name` in plural basically a copy of the item for each iteration (remember immutable right, so new copy)

Alternatively we can use a `for` loop:
```c#
for (int i=0; i < names.Count; i++)
{
	Console.WriteLine($"Hello {names[i].ToUpper()}!");
}
```
this will give the same output as `foreach`, notice we used the `Count` property, this will give us the number of items in the list.

#### adding to the list
```c#
names.Add("Ahmed");
```
##### what happens in memory?
when adding items the list would automatically occupy enough space in memory, `List` is flexible, we call this [capacity](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=net-8.0) resizing

#### accessing arrays from the end
when accessing arrays we can use `^` to access array from the end, this is called **index from end operator** introduced in C# 8.0.

we can use it like:
```c#
Console.WriteLine(names[^1]);
```
after Adding "Ahmed" the output will be "Ahmed"
we can read `names[^1]`  like "the last in names array"
or `names[^2}` as "the second last in the names array"

#### accessing arrays as a range 
we can use `..` to access arrays as a range:
`names[2..4]` we call this the **range operator** introduced in C# 8.0.

if we use `names [2..4]` on names:
```c#
var names = new List<string>
{
    "Abdullah",
    "Yaser",
    "Ali",
    "Ahmed",
    "Marwan"
};

foreach (var name in names[2..4])
{
	Console.WriteLine($"Hello {name.ToUpper()}!");
}
```
the output would be:
```bash
Hello ALI!
Hello AHMED!
```
so we started from index 2 until 3, which means `names[2..4]` gets the items in List names from index 2 until but not including index 4.

#### arrays
```c#
var names = new String[] { "Abdullah", "Yaser" };
```
arrays are fixed in length, cant add new items.
if we need to add, we can replace a new array with the another one which have the added values.

in C# 12.0, we can use the `..` **(spread element)** to add a new item:
```c#
names = [..names, "Ali"];
```
this will basically copy the names array, adds "Ali" then replaces names with `[..names, "Ali"]`

- ***honestly just default to `List<T>` for most of your work.***

#### Sorting and Searching Lists
```c#
names.Sort();
```
- this alphabetically sorts the names.
- Sort will actually sort the elements IN PLACE, it doesn't copy elements into a new list, it modifies the original list it self.
- if we use `IndexOf()` method on a item in the list before and after sorting, you will probably get a different index value for that item.
- `IndexOf()` is convenient for searching the index of an item.

 there are 3 more overloads for Sort:

| Method                             | Description                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------ |
| `Sort()`                           | Sorts the elements in the entire `List<T>` using the default comparer.               |
| `Sort(Comparison<T>)`              | Sorts the elements in the entire `List<T>` using the specified `Comparison<T>`.      |
| `Sort(IComparer<T>)`               | Sorts the elements in the entire `List<T>` using the specified comparer.             |
| `Sort(Int32, Int32, IComparer<T>)` | Sorts the elements in a range of elements in `List<T>` using the specified comparer. |
go to docs and read for more info


# Other commonly used methods:

### Adding Elements
- **Add**
  ```csharp
  list.Add(item);
  ```
  Adds an item to the end of the list.

- **AddRange**
  ```csharp
  list.AddRange(collection);
  ```
  Adds the elements of the specified collection to the end of the list.

### Removing Elements
- **Remove**
  ```csharp
  list.Remove(item);
  ```
  Removes the first occurrence of a specific object from the list.

- **RemoveAt**
  ```csharp
  list.RemoveAt(index);
  ```
  Removes the element at the specified index of the list.

- **RemoveAll**
  ```csharp
  list.RemoveAll(predicate);
  ```
  Removes all the elements that match the conditions defined by the specified predicate.

- **Clear**
  ```csharp
  list.Clear();
  ```
  Removes all elements from the list.

### Accessing Elements
- **Contains**
  ```csharp
  bool contains = list.Contains(item);
  ```
  Determines whether the list contains a specific value.

- **IndexOf**
  ```csharp
  int index = list.IndexOf(item);
  ```
  Searches for the specified object and returns the zero-based index of the first occurrence within the entire list.

- **LastIndexOf**
  ```csharp
  int index = list.LastIndexOf(item);
  ```
  Searches for the specified object and returns the zero-based index of the last occurrence within the entire list.

### Modifying Elements
- **Insert**
  ```csharp
  list.Insert(index, item);
  ```
  Inserts an element into the list at the specified index.

- **InsertRange**
  ```csharp
  list.InsertRange(index, collection);
  ```
  Inserts the elements of a collection into the list at the specified index.

### Sorting and Reversing
- **Sort**
  ```csharp
  list.Sort();
  list.Sort(comparer);
  list.Sort(comparison);
  list.Sort(index, count, comparer);
  ```
  Sorts the elements in the entire list or a portion of it using various sorting options.

- **Reverse**
  ```csharp
  list.Reverse();
  list.Reverse(index, count);
  ```
  Reverses the order of the elements in the entire list or a portion of it.

### Iterating
- **ForEach**
  ```csharp
  list.ForEach(action);
  ```
  Performs the specified action on each element of the list.

### Other Useful Methods
- **Count**
  ```csharp
  int count = list.Count;
  ```
  Gets the number of elements contained in the list.

- **ToArray**
  ```csharp
  T[] array = list.ToArray();
  ```
  Copies the elements of the list to a new array.

- **Find**
  ```csharp
  T item = list.Find(predicate);
  ```
  Searches for an element that matches the conditions defined by the specified predicate and returns the first occurrence within the entire list.

- **FindAll**
  ```csharp
  List<T> matches = list.FindAll(predicate);
  ```
  Retrieves all the elements that match the conditions defined by the specified predicate.

- **FindIndex**
  ```csharp
  int index = list.FindIndex(predicate);
  int index = list.FindIndex(startIndex, predicate);
  int index = list.FindIndex(startIndex, count, predicate);
  ```
  Searches for an element that matches the conditions defined by the specified predicate and returns the zero-based index of the first occurrence.

- **FindLastIndex**
  ```csharp
  int index = list.FindLastIndex(predicate);
  int index = list.FindLastIndex(startIndex, predicate);
  int index = list.FindLastIndex(startIndex, count, predicate);
  ```
  Searches for an element that matches the conditions defined by the specified predicate and returns the zero-based index of the last occurrence.
