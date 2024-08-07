- [Starting Point](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-8.0)
- Docs: [ControllerBase](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-8.0) class
- Tutorial: [Create web API with ASP.NET controllers](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/?source=recommendations)


**[A Very Nice Tutorial](https://dev.to/rasheedmozaffar/web-apis-crud-with-net-and-ef-core-52dp)** on controller based API CRUD With EF Core.

Making a new Controllers  API with .NET 8.0
```bash
dotnet new webapi -controllers -f net8.0
```

## Program Structure
we have:
- Models: a class for the data entities (to reflect the database)
- Services: performs operations on data (queries to db)
- Controllers: defines the routes 


## Making the controller
```c#
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class PizzaController : ControllerBase
{
	public PizzaController()
	{}

	// GET all action
	
	// GET by Id action
	
	// POST action
	
	// PUT action
	
	// DELETE action
	
}
```

`IActionResult`
`ActionResult`


### Methods from `ControllerBase` class

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


## Dependency Injection with `builder.Services.AddScoped<>();`
let's say we have two types of services, the regular service and mock service, regular service used for development and production and the mock service is used for testing (assume for some reason we require to use a different service)

#### `PizzaService.cs`
```c#
public interface IPizzaService {
	...
}
  

public class PizzaService : IPizzaService {
	...
{


public class MockPizzaService : IPizzaService {
	...
}
```

#### `Program.cs`
now we can inject the Pizza service. Regular pizza service:
```c#
builder.Services.AddScoped<IPizzaService, PizzaService>();
```
for testing we use mock pizza service:
```c#
builder.Services.AddScoped<IPizzaService, MockPizzaService>();
```

the same as above can be used but for singleton, [check this link](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/dependency-injection?view=aspnetcore-8.0)

## Model Validation with `RequiredAttribute` Class
- docs: [`requiredAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.requiredattribute?view=net-8.0)

in a model file we could use `RequiredAttriubte` in this way:
```c#
[Required(AllowEmptyStrings = false)]
public string Name { get; set; }
```

Other validations:
```c#
public class Movie
{
    public int ID { get; set; }

    [Required(ErrorMessage = "Title is required")]
    public string Title { get; set; }

    [Required(ErrorMessage = "Date is required")]
    public DateTime ReleaseDate { get; set; }

    [Required(ErrorMessage = "Genre must be specified")]
    public string Genre { get; set; }

    [Range(1, 100, ErrorMessage = "Price must be between $1 and $100")]
    public decimal Price { get; set; }

    [StringLength(5)]
    public string Rating { get; set; }
}
```


