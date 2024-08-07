

add required packages
```csharp
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Create Model
```c#
public class Movie
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Director { get; set; }
    public string Genre { get; set; }
    public int ReleaseYear { get; set; }
    public decimal Price { get; set; }
}
```

create the DbContext
```csharp
using Microsoft.EntityFrameworkCore;

public class MoviesContext : DbContext
{
    public DbSet<Movie> Movies { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(@"Server=your_server_name;Database=MoviesDB;Trusted_Connection=True;");
    }
}
```

Crud operations
```csharp
public class MoviesService
{
    private readonly MoviesContext _context;

    public MoviesService()
    {
        _context = new MoviesContext();
    }

    // Create
    public void AddMovie(Movie movie)
    {
        _context.Movies.Add(movie);
        _context.SaveChanges();
    }

    // Read
    public List<Movie> GetAllMovies()
    {
        return _context.Movies.ToList();
    }

    public Movie GetMovieById(int id)
    {
        return _context.Movies.Find(id);
    }

    // Update
    public void UpdateMovie(Movie movie)
    {
        _context.Movies.Update(movie);
        _context.SaveChanges();
    }

    // Delete
    public void DeleteMovie(int id)
    {
        var movie = _context.Movies.Find(id);
        if (movie != null)
        {
            _context.Movies.Remove(movie);
            _context.SaveChanges();
        }
    }
}
```

