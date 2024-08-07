## 5/8/2024 Monday
- Don't forget to initialize Lists bruh
- We avoid making `List<T>` nullable because we almost always initialize it, otherwise we are more likely to run into `NullReferenceException` especially  when we need to use them in `for` and `foreach` loops, read more about [NullReferenceExecption](https://stackoverflow.com/questions/4660142/what-is-a-nullreferenceexception-and-how-do-i-fix-it)
- [Don't make database queries in constructors](https://softwareengineering.stackexchange.com/questions/392905/should-one-make-the-database-calls-in-the-constructor-or-method-of-a-class#:~:text=So%20if%20you%20put%20the,you%20are%20hitting%20the%20database.) Constructors should not do work, initialization of new objects should happen very quickly.
- Constructors should be limited to allocating new memory for the new object (like initializing empty collections or assigning default values to required fields) and ensuring the object is in a valid state upon first being created. Anything beyond that belongs in an instance method, or arguments passed into the constructor. 

To Watch
- [Exception Handling](https://www.youtube.com/watch?v=aBMfdTNwKBI) 

## 6/8/2024 Tuesday
- Two approaches to databases in ASP.Net
	- Db-First approach, we make the database and create the tables in SSMS
	- Code-First approach:
		- generate SQL from models
		- or use `context.Database.EnsureCreated()` to try to create the database and tables based on the models if they aren't already created. 
- learn to use your IDE: [C# productivity guide](https://learn.microsoft.com/en-us/visualstudio/ide/csharp-developer-productivity?view=vs-2022&utm_source=VisualStudio&utm_medium=aspnet-getstarted&utm_campaign=VisualStudio)

## 7/8/2024 Wednesday
- sometimes `IDENTITY(1,1)` jumps by 1000 when SQL Server gets rebooted - [StackOverflow](https://stackoverflow.com/questions/17587094/identity-column-value-suddenly-jumps-to-1001-in-sql-server)
- when to use `Find()` and `FirstOrDefault()` - [Reddit](https://www.reddit.com/r/dotnet/comments/1bvjubg/employ_the_usage_of_find_instead_of/)
- When reading docs: don't confuse between dotnet framework and dotnet core, and don't confuse between entity framework and entity framework core, its not the same bruhh
	- prefix your class with `sealed` when you know for sure your class won't be inherited by any other class, which improves performance.



