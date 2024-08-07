datatypes:
`int`, `float`, `long`

- [working with numbers](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/tutorials/numbers-in-csharp-local#explore-integer-math)
- [integral type](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/tutorials/numbers-in-csharp-local#explore-integer-math)
- [floating-point types](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/tutorials/numbers-in-csharp-local#explore-integer-math)

int is for integers, can take up to 2^32, appox, 2.1 billion
if need a bigger number can use `long` 2^64, basically every possible number, kinda.
```c#
int firstNumber = 2100000000; // 2.1 billion: allowed
int secondNumber = 2100000000; // 2.1 billion: allowed

int sum = firstNumber + secondNumber; // NOT ALLOWED, cuz sum is more than 2^32
```
above summation is not allowed, instead we must use `long` to store up to 2^64, but that alone is not enough if we use `firstNumber + secondNumber`, because we are adding two *integers* which are reserved in memory as 2^32.

To tackle the above problem we do type casting, which is like tricking the compiler to use long when adding the values.
```c#
int firstNumber = 2100000000; // 2.1 billion: allowed
int secondNumber = 2100000000; // 2.1 billion: allowed

long sum = (long)firstNumber + (long)secondNumber; // the right way to do it
```

lets say if we are not really sure about the values, then we can use `checked()` to give an exception if calculation would go overflow
 
```c#
int firstNumber = 2100000000; // 2.1 billion: allowed
int secondNumber = 2100000000; // 2.1 billion: allowed

long sum = checked(firstNumber + secondNumber);
Console.WriteLine(sum);
```

this will produce the following:
```bash
Unhandled exception. System.OverflowException: Arithmetic operation resulted in an overflow.
   at Program.<Main>$(String[] args) in C:\Users\abda7\Documents\code\helloWorld\Program.cs:line 4
```
`checked()` can be useful when debugging stuff with large numbers.








