- LINQ: Language Integrated Query Language
- docs: [LINQ](https://learn.microsoft.com/en-us/dotnet/csharp/linq/get-started/introduction-to-linq-queries) 

## `from`, `where`, `orderby` and `select`
Consider this list:
```c#
List<int> scores = [40, 60, 80, 81, 82, 90, 97];
```

we can perform the following query:
```c#
IEnumerable<int> scoreQuery = 
    from score in scores
    where score > 80
    select score;

foreach (int i in scoreQuery)
{
    Console.Write(i + " ");
}
```
this would give the following output:
```bash
81 82 90 97 
```

now  this:
```c#
IEnumerable<string> scoreQuery = 
    from score in scores
    where score > 80
    orderby score descending
    select $"The score is {score}";

foreach (var query in scoreQuery)
{
    Console.WriteLine(query);
}
```
gives the following output:
```bash
The score is 97 
The score is 90
The score is 82
The score is 8
```

to convert into a list:
```c#
List<int> myScores = scoreQuery.ToList();
```

## query syntax vs method syntax
this syntax
```c#
IEnumerable<string> scoreQuery = 
    from score in scores
    where score > 80
    orderby score descending
	select score;
```
is called **query syntax**.

an alternative in **method syntax** would be
```c#
var scoreQuery2 = scores.Where(s => s > 80).OrderByDescending(s => s);
```
in method syntax we chain methods, its honestly all a matter of style, sometimes its more succinct to use method syntax, query syntax looks nice for writing complicated queries (as it is more readable).

## Common queries and methods you will use the most
### Basic Query Methods

#### 1. **Where**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  where item > 10
                  select item;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.Where(item => item > 10);
     ```

#### 2. **Select**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  select item * 2;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.Select(item => item * 2);
     ```

#### 3. **OrderBy**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  orderby item
                  select item;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.OrderBy(item => item);
     ```

#### 4. **OrderByDescending**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  orderby item descending
                  select item;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.OrderByDescending(item => item);
     ```

#### 5. **ThenBy**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  orderby item.Property, item.AnotherProperty
                  select item;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.OrderBy(item => item.Property)
                      .ThenBy(item => item.AnotherProperty);
     ```

#### 6. **ThenByDescending**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  orderby item.Property, item.AnotherProperty descending
                  select item;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.OrderBy(item => item.Property)
                      .ThenByDescending(item => item.AnotherProperty);
     ```

#### 7. **GroupBy**

   - **Query Syntax**:
     ```csharp
     var result = from item in list
                  group item by item.Category into grouped
                  select grouped;
     ```

   - **Method Syntax**:
     ```csharp
     var result = list.GroupBy(item => item.Category);
     ```

#### 8. **Join**

   - **Query Syntax**:
     ```csharp
     var result = from item1 in list1
                  join item2 in list2 on item1.Id equals item2.Id
                  select new { item1.Name, item2.Description };
     ```

   - **Method Syntax**:
     ```csharp
     var result = list1.Join(
         list2,
         item1 => item1.Id,
         item2 => item2.Id,
         (item1, item2) => new { item1.Name, item2.Description }
     );
     ```

### Aggregation Methods

#### 1. **Count**

   - **Query Syntax**:
     ```csharp
     var count = (from item in list
                  select item).Count();
     ```

   - **Method Syntax**:
     ```csharp
     int count = list.Count();
     ```

#### 2. **Sum**

   - **Query Syntax**:
     ```csharp
     var sum = (from item in list
                select item).Sum();
     ```

   - **Method Syntax**:
     ```csharp
     int sum = list.Sum();
     ```

#### 3. **Min**

   - **Query Syntax**:
     ```csharp
     var min = (from item in list
                select item).Min();
     ```

   - **Method Syntax**:
     ```csharp
     int min = list.Min();
     ```

#### 4. **Max**

   - **Query Syntax**:
     ```csharp
     var max = (from item in list
                select item).Max();
     ```

   - **Method Syntax**:
     ```csharp
     int max = list.Max();
     ```

#### 5. **Average**

   - **Query Syntax**:
     ```csharp
     var average = (from item in list
                    select item).Average();
     ```

   - **Method Syntax**:
     ```csharp
     double average = list.Average();
     ```

### Element Methods

#### 1. **First**

   - **Query Syntax**:
     ```csharp
     var firstItem = (from item in list
                      select item).First();
     ```

   - **Method Syntax**:
     ```csharp
     var firstItem = list.First();
     ```

#### 2. **FirstOrDefault**
Returns the first element of the sequence that satisfies a condition, or a specified default value if no such element is found.
   - **Query Syntax**:
     ```csharp
     var firstItem = (from item in list
                      select item).FirstOrDefault();
     ```

   - **Method Syntax**:
     ```csharp
     var firstItem = list.FirstOrDefault();
     ```

#### 3. **Last**

   - **Query Syntax**:
     ```csharp
     var lastItem = (from item in list
                     select item).Last();
     ```

   - **Method Syntax**:
     ```csharp
     var lastItem = list.Last();
     ```

#### 4. **LastOrDefault**

   - **Query Syntax**:
     ```csharp
     var lastItem = (from item in list
                     select item).LastOrDefault();
     ```

   - **Method Syntax**:
     ```csharp
     var lastItem = list.LastOrDefault();
     ```

#### 5. **Single**

   - **Query Syntax**:
     ```csharp
     var singleItem = (from item in list
                       select item).Single();
     ```

   - **Method Syntax**:
     ```csharp
     var singleItem = list.Single();
     ```

#### 6. **SingleOrDefault**

   - **Query Syntax**:
     ```csharp
     var singleItem = (from item in list
                       select item).SingleOrDefault();
     ```

   - **Method Syntax**:
     ```csharp
     var singleItem = list.SingleOrDefault();
     ```

### Set Methods

#### 1. **Distinct**

   - **Query Syntax**:
     ```csharp
     var distinctItems = (from item in list
                          select item).Distinct();
     ```

   - **Method Syntax**:
     ```csharp
     var distinctItems = list.Distinct();
     ```

#### 2. **Union**

   - **Query Syntax**:
     ```csharp
     var unionList = (from item in list1
                      select item).Union(list2);
     ```

   - **Method Syntax**:
     ```csharp
     var unionList = list1.Union(list2);
     ```

#### 3. **Intersect**

   - **Query Syntax**:
     ```csharp
     var intersectList = (from item in list1
                          select item).Intersect(list2);
     ```

   - **Method Syntax**:
     ```csharp
     var intersectList = list1.Intersect(list2);
     ```

#### 4. **Except**

   - **Query Syntax**:
     ```csharp
     var exceptList = (from item in list1
                       select item).Except(list2);
     ```

   - **Method Syntax**:
     ```csharp
     var exceptList = list1.Except(list2);
     ```

### Conversion Methods

#### 1. **ToList**

   - **Query Syntax**:
     ```csharp
     var list = (from item in enumerable
                 select item).ToList();
     ```

   - **Method Syntax**:
     ```csharp
     List<int> list = enumerable.ToList();
     ```

#### 2. **ToArray**

   - **Query Syntax**:
     ```csharp
     var array = (from item in enumerable
                  select item).ToArray();
     ```

   - **Method Syntax**:
     ```csharp
     int[] array = enumerable.ToArray();
     ```

#### 3. **ToDictionary**

   - **Query Syntax**:
     ```csharp
     var dictionary = (from item in list
                       select item).ToDictionary(x => x.Id, x => x.Name);
     ```

   - **Method Syntax**:
     ```csharp
     Dictionary<int, string> dictionary = list.ToDictionary(x => x.Id, x => x.Name);
     ```

#### 4. **ToLookup**

   - **Query Syntax**:
     ```csharp
     var lookup = (from item in list
                   select item).ToLookup(x => x.Category);
     ```

   - **Method Syntax**:
     ```csharp
     var lookup = list.ToLookup(x => x.Category);
     ```

