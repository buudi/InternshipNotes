Sources:
- [Asynchronous Programming](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/)
- [Real world Asynchronous Programming Scenarios](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/async-scenarios)
- [Task Asynchronous Programming model (TAP)](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/task-asynchronous-programming-model?source=recommendations)
- [`Task` Class](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=net-8.0)
- [`Data.Entity.QueryableExtensions` Class in Entity Framework](https://learn.microsoft.com/en-us/dotnet/api/system.data.entity.queryableextensions?view=entity-framework-6.2.0) 
- [`DbContext` class in Entity Framework](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-8.0)

Further reading:
- Read on **Concurrency** in C# : [Concurrency in C#: Step-by-Step Tutorial 2024 - zenrows](https://www.zenrows.com/blog/concurrency-c-sharp)
## Asynchronous Programming
- [Task Asynchronous Programming model (TAP)](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/task-asynchronous-programming-model?source=recommendations): Provides abstraction over asynchronous code.
- TAP is a language level model.
- Compiler performs many transformations, cuz some statements may start work and return a `Task` that represents the ongoing work.
- [`Task` Class](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=net-8.0) : Represents an **Asynchronous** operation, has 8 overrides

> [!NOTE] basically the whole point of async await
> 	enable code that reads like a sequence of statements but executes in a much more complicated order based on external resource allocation and when tasks are complete.

- in a parallel algorithm we'd need multiple threads to handle multiple tasks, each thread would be blocked synchronously (waiting for the task to be completed)
- in asynchronous programming, a single thread could handle multiple tasks asynchronously
	- i.e. start a task, while its doing work start another task and so on (no need wait for a task to be completed, can start other tasks as earlier ones are still doing their work)

## Real world scenarios for Asynchronous Programming
Read: [Real world Asynchronous Programming Scenarios](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/async-scenarios)

1. any I/O-bound needs :
	- requesting data from network
	- accessing a database
	- reading and writing to a file system
	- etc...
2. CPU-bound code (for performance)
	- expensive calculation

## Async Await in C sharp
- for I/O-bound code, we **await** an operation that returns a `Task` or `Task<T>` inside of an **async method**.
- for CPU-bound code, we **await** an operation that is started on a **background thread** with the `Task.Run()` method.

the `await` keyword yields control to the caller of the method that performed the await.

#### I/O-bound example: downloading data from a web service

when a button is pressed it downloads some data, we don't want the download to **block** the UI thread so we 
```csharp
s_downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await s_httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```
s_httpClient is of type [`HttpClient`](https://learn.microsoft.com/en-us/dotnet/api/system.net.http.httpclient?view=net-8.0) which allows to download data from the web
 - `HttpClient` is a class that allows to send/receive HTTP requests/responses

- can read on CPU-bound: [CPU-bound example](https://www.zenrows.com/blog/concurrency-c-sharp)

## `Task` Class
[docs](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=net-8.0)
represents an Asynchronous Operation
has 8 overrides, for this section we use `Task<T>`
### here is how we could use it in Controller Based Web APIs with CRUD endpoints: 

> [!NOTE] 
> Be wary of the `DbContext` async methods and `EntityFrameworkQueryableExtensions` async methods, we we use them in this section, don't confuse between the two.
> - A [`DbContext`](https://learn.microsoft.com/en-us/dotnet/api/system.data.entity.dbcontext?view=entity-framework-6.2.0) instance represents a session with the database, its methods can be used **to query and save an entity instance.**
> - while [`EntityFrameworkQueryableExtensions`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions?view=efcore-8.0) provide useful extension methods for use with **Entity Framework LINQ queries**.
> 

> [!warning] Documentation WARNING
> Make sure when you are referring the documentation you are actually referring to the **Entity Framework Core 8.0** and **NOT Entity Framework 6.2.0** 
> - `QueryableExtensions` belongs to Entity Framework 6.2.0.
> - `EntityFrameworkQueryableExtensions` belongs to Entity Framework Core 8.0.
> 
> **Always check what you are referring!**!

Just Follow [`DbSet<TEntity>`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbset-1?view=efcore-8.0)

#### Get all
```csharp
[HttpGet]
public async Task<IActionResult> Get()
{
    var moviesToReturn = await _dbContext.Movies.ToListAsync();
    return Ok(moviesToReturn);
}
```
- we used `ToListAsync()` instead of `ToList()` on the `List<Movie>` Movies.
	- a method of ``EntityFrameworkQueryableExtensions`` class
- notice we prefixed the method type with the keyword `async` and we prefixed the asynchronous expression with `await`.
- the `Task<IActionResult>` returned `Ok()` which is from the [`ControllerBase`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-8.0) class

##### Commonly used methods from the [`ControllerBase`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-8.0) class include:

| Status Code      | Corresponding Method  |
| ---------------- | --------------------- |
| 200 Ok           | `Ok()`                |
| 201 Created      | `Created()`           |
|                  | `CreatedAtAction()`   |
|                  | `CreatedAtRoute()`    |
| 400 Bad request  | `BadRequest()`        |
|                  | `ValidationProblem()` |
| 401 Unauthorized | `Unauthorized()`      |
| 403 Forbidden    | `Forbid()`            |
| 404 NotFound     | `NotFound()`          |

#### Get by Id
```csharp
[HttpGet("{id}")]
public async Task<IActionResult> Get(int id)
{
    var movieItem = await _dbContext.Movies.FirstOrDefaultAsync(m => m.Id == id);
    if (movieItem == null)
        return NotFound();
    return Ok(movieItem);
}
```
- we used `FirstOrDefaultAsync` here instead of `FirstOrDefault`
	- a method of `QueryableExtensions` class
#### Create
```csharp
[HttpPost]
public async Task<IActionResult> Post(Movie movie)
{
    if (movie == null)
        return BadRequest("Please Enter Valid Information");

    await _dbContext.Movies.AddAsync(movie);
    await _dbContext.SaveChangesAsync();

    return CreatedAtAction(nameof(Get), new { id = movie.Id }, movie);

}
```
- we used `AddAsync()` instead of `Add()`
	- both are methods of `DbContext` class
- we used `SaveChangesAsync()` instead of `SaveChanges()`
- we returned `CreatedAtAction()`, a method of the `ControllerBase` class

#### Update
```csharp
[HttpPut("{id}")]
public async Task<IActionResult> Put(int id, Movie updatedMovie)
{
    var movieToUpdate = await _dbContext.Movies.FirstOrDefaultAsync(m => m.Id == id);
    if (movieToUpdate == null)
        return NotFound($"Invalid Movie Id");

    updatedMovie.Id = id;
    movieToUpdate.Title = updatedMovie.Title;
    movieToUpdate.Director = updatedMovie.Director;
    movieToUpdate.ReleaseYear = updatedMovie.ReleaseYear;
    movieToUpdate.Genre = updatedMovie.Genre;
    movieToUpdate.Price = updatedMovie.Price;

    await _dbContext.SaveChangesAsync();
    return NoContent();
}
```

#### Delete
```csharp
[HttpDelete("{id}")]
public async Task<IActionResult> Delete(int id)
{
	var movie = await _dbContext.Movies.FirstOrDefaultAsync(m => m.Id == id);
	if (movie == null) return BadRequest(string.Empty);

	_dbContext.Movies.Remove(movie);
	await _dbContext.SaveChangesAsync();

	return Ok($"Movie: {movie.Title} is deleted from the database successfully");
}
```

## Summary
- **Problem:** some tasks like **I/O and CPU bound tasks** take some time to process, blocking other tasks from being processed.
- **Solution:** Asynchronous programming allows multiple tasks to be run in parallel.
- `await` yields control to the `async` method that calls it
- in C# TAP (Task Asynchronous Programming Model) provides abstraction over asynchronous code
- in C# we define a Task with `Task<T>` think of it as opening a new thread for the process. (but not exactly exactly a thread, remember TAP)
- we use async await for **I/O and CPU bound tasks**.   
