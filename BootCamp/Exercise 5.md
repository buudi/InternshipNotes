## Completed
- *MoviesMenuApi:* Refactor your code to Implement dependency Injection for DB context and services **(DONE)**
	- use `AddScoped<>()`
	- know the difference between different types of dependency injection: Scoped, Singleton and transient - [StackOverflow](https://stackoverflow.com/questions/38138100/addtransient-addscoped-and-addsingleton-services-differences)

- *MoviesMenuApi:* call the context within the movie service, not directly from controller **(DONE)**
	- this way it becomes cleaner architecture, controller relies on movie service (business logic layer), movie service relies on DbContext (data access layer)
		- then our controller becomes what we call a thin controller, all it does is that it serves as an entry point for API calls (only and only).
> **Done** for `MoviesController` **only**
> if got time, can do for `MoviesAsyncController`, but have to transfer the EF async methods into movie service first.

- *MoviesMenuSql:* remove `connection.close()`, because `using` statement closes the connection automatically, read more about classes that implement the [`IDisposable`](https://reintech.io/blog/tutorial-working-idisposable-interface-net-csharp#google_vignette) Interface **(DONE)**

**(DONE)**
- [Learn MVC and templating with Razor .](https://dotnettutorials.net/lesson/introduction-asp-net-core-mvc/)
	- make a page that returns a list of all movies.
	- use [layouts](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/layout?view=aspnetcore-8.0)
	- [partial views](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/partial?view=aspnetcore-8.0).
- learn [HTTP fundamentals](https://dev.to/temiogundeji/http-the-fundamentals-339c)
## Deadline
- Internal Deadline: By Thursday 8th August
- Client Deadline: By Friday 9th August


