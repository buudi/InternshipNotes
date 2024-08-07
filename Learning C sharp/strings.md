datatype: `string`
[documentation](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-8.0)
string data type is a [reference type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/reference-types)

strings are **immutable**, meaning no method can actually modify the content of the string, instead they will always return a new string.

####  string interpolation
just add `$` before the `"`
```c#
string fname = "Abdullah";
string lname = "Yaser";
Console.WriteLine($"My first name is {fname} and my last name is {lname}");
```
concatenation also works

#### Trimming the trailing white spaces
consider this:
```c#
string fname = "   Abdullah   ";
string lname = "Yaser";
```
can trim the trailing white spaces using `fname = fname.Trim();`
other methods for trimming are
- `TrimStart()`
- `TrimEnd()`

#### Common methods:
- **Length**
    - `int length = myString.Length;`
    - Gets the number of characters in the string.
- **Substring**
    - `string sub = myString.Substring(startIndex);`
    - `string sub = myString.Substring(startIndex, length);`
    - Retrieves a substring from the string.
- **Contains**
    - `bool contains = myString.Contains("text");`
    - Determines whether the string contains a specified substring.
- **IndexOf**
    - `int index = myString.IndexOf("text");`
    - Finds the index of the first occurrence of a substring.
- **LastIndexOf**
    - `int index = myString.LastIndexOf("text");`
    - Finds the index of the last occurrence of a substring.
- **Replace**
    - `string replaced = myString.Replace("oldText", "newText");`
    - Replaces all occurrences of a specified substring with another substring.
- **Split**
    - `string[] parts = myString.Split(' ');`
    - Splits the string into an array of substrings based on a delimiter.
- **Join**
    - `string joined = string.Join(", ", stringArray);`
    - Concatenates an array of strings into a single string, with a specified separator.
- **Trim**
    - `string trimmed = myString.Trim();`
    - Removes all leading and trailing white-space characters from the string.
- **TrimStart**
    - `string trimmedStart = myString.TrimStart();`
    - Removes all leading white-space characters from the string.
- **TrimEnd**
    - `string trimmedEnd = myString.TrimEnd();`
    - Removes all trailing white-space characters from the string.
- **ToLower**
    - `string lower = myString.ToLower();`
    - Converts the string to lowercase.
- **ToUpper**
    - `string upper = myString.ToUpper();`
    - Converts the string to uppercase.
- **StartsWith**
    - `bool starts = myString.StartsWith("text");`
    - Determines whether the beginning of the string matches a specified substring.
- **EndsWith**
    - `bool ends = myString.EndsWith("text");`
    - Determines whether the end of the string matches a specified substring.
- **Format**
    - `string formatted = string.Format("Hello, {0}!", name);`
    - Creates a new string by replacing placeholders with specified values.
- **IsNullOrEmpty**
    - `bool isEmpty = string.IsNullOrEmpty(myString);`
    - Indicates whether the specified string is `null` or an empty string.
- **IsNullOrWhiteSpace**
    - `bool isWhiteSpace = string.IsNullOrWhiteSpace(myString);`
    - Indicates whether the specified string is `null`, empty, or consists only of white-space characters.
