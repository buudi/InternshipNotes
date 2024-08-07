[Link given by Arif](https://community.ibm.com/community/user/powerdeveloper/blogs/sapana-khemkar/2023/03/28/connect-dotnet-app-sql-db-on-ibm-power)
[[CRUD Queries with ADO.NET]]
similar to SERIAL in Postgres:
`Id int NOT NULL IDENTITY(1,1) PRIMARY KEY,`

use your code from exercise 1
feedback for exercise 1: refactor your code (use a separate class  for console services)


Set up MS SQL database, connect it to the console app
and make a table for the movies.

- use the database instead of the local structures.
- use ADO.Net (for now don't use EF Core).
- Ensure parametrization and use of best practices throughout the program.
### Architecture:
![[Pasted image 20240805103123.png]]

## steps:

make a new console app
```csharp
dotnet new console -o SqlServerSample
```

add SqlClient package
```csharp
cd SqlServerSample/
dotnet add package System.Data.SqlClient
```


### 1. DbService Class

This class will be responsible for managing the database connection. It will provide a method to get a `SqlConnection` object.

```csharp
using System.Data.SqlClient;

public class DbService
{
    private readonly string _connectionString;

    public DbService(string connectionString)
    {
        _connectionString = connectionString;
    }

    public SqlConnection GetConnection()
    {
        return new SqlConnection(_connectionString);
    }
}
```

### 2. MovieService Class

This class will contain all the SQL queries related to movies. It will use `DbService` to get the connection.

```csharp
using System;
using System.Collections.Generic;
using System.Data.SqlClient;

public class MovieService
{
    private readonly DbService _dbService;

    public MovieService(DbService dbService)
    {
        _dbService = dbService;
    }

    public List<Movie> GetAllMovies()
    {
        List<Movie> movies = new List<Movie>();

        string query = "SELECT * FROM Movies";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            connection.Open();
            SqlDataReader reader = command.ExecuteReader();

            while (reader.Read())
            {
                movies.Add(new Movie
                {
                    Id = (int)reader["Id"],
                    Title = (string)reader["Title"],
                    Director = (string)reader["Director"],
                    ReleaseDate = (DateTime)reader["ReleaseDate"]
                });
            }
        }
        return movies;
    }

    public void AddMovie(Movie movie)
    {
        string query = "INSERT INTO Movies (Title, Director, ReleaseDate) VALUES (@Title, @Director, @ReleaseDate)";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Title", movie.Title);
            command.Parameters.AddWithValue("@Director", movie.Director);
            command.Parameters.AddWithValue("@ReleaseDate", movie.ReleaseDate);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }

    public void UpdateMovie(Movie movie)
    {
        string query = "UPDATE Movies SET Title = @Title, Director = @Director, ReleaseDate = @ReleaseDate WHERE Id = @Id";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Title", movie.Title);
            command.Parameters.AddWithValue("@Director", movie.Director);
            command.Parameters.AddWithValue("@ReleaseDate", movie.ReleaseDate);
            command.Parameters.AddWithValue("@Id", movie.Id);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }

    public void DeleteMovie(int id)
    {
        string query = "DELETE FROM Movies WHERE Id = @Id";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Id", id);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }
}
```

### 3. Movie Class

Define a `Movie` class to represent the movie entity.

```csharp
public class Movie
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Director { get; set; }
    public DateTime ReleaseDate { get; set; }
}
```

### 4. Usage Example

Here is how you would use the `DbService` and `MovieService` in your application.

```csharp
class Program
{
    static void Main(string[] args)
    {
        string connectionString = "Server=your_server_name;Database=your_database_name;User Id=your_username;Password=your_password;";
        DbService dbService = new DbService(connectionString);
        MovieService movieService = new MovieService(dbService);

        // Add a new movie
        Movie newMovie = new Movie
        {
            Title = "Inception",
            Director = "Christopher Nolan",
            ReleaseDate = new DateTime(2010, 7, 16)
        };
        movieService.AddMovie(newMovie);

        // Get all movies
        List<Movie> movies = movieService.GetAllMovies();
        foreach (var movie in movies)
        {
            Console.WriteLine($"{movie.Id} - {movie.Title} - {movie.Director} - {movie.ReleaseDate}");
        }

        // Update a movie
        Movie updatedMovie = movies[0];
        updatedMovie.Title = "Updated Title";
        movieService.UpdateMovie(updatedMovie);

        // Delete a movie
        movieService.DeleteMovie(updatedMovie.Id);
    }
}
```

In this architecture:
- `DbService` encapsulates the logic for creating and managing database connections.
- `MovieService` handles all the SQL operations (CRUD) related to movies, utilizing the `DbService` for database connections.

This separation of concerns improves the maintainability and testability of your code.




Or:


If you prefer to handle the database connection setup inside the `DbService` class, you can adjust your design accordingly. In this setup, `DbService` will be responsible for managing the connection string and providing a connection, while `MovieService` will interact with the `DbService` without needing to handle connection details.

Here’s how you can modify the code:

### 1. `DbService` Class

The `DbService` class will be responsible for initializing the connection string and providing the connection object.

```csharp
using System.Data.SqlClient;

public class DbService
{
    private readonly string _connectionString;

    public DbService()
    {
        // Initialize the connection string here
        _connectionString = "Server=your_server_name;Database=your_database_name;User Id=your_username;Password=your_password;";
    }

    public SqlConnection GetConnection()
    {
        return new SqlConnection(_connectionString);
    }
}
```

### 2. `MovieService` Class

The `MovieService` class will interact with the `DbService` to get the connection. It will not need to manage or pass the connection string.

```csharp
using System;
using System.Collections.Generic;
using System.Data.SqlClient;

public class MovieService
{
    private readonly DbService _dbService;

    public MovieService(DbService dbService)
    {
        _dbService = dbService;
    }

    public List<Movie> GetAllMovies()
    {
        List<Movie> movies = new List<Movie>();

        string query = "SELECT * FROM Movies";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            connection.Open();
            SqlDataReader reader = command.ExecuteReader();

            while (reader.Read())
            {
                movies.Add(new Movie
                {
                    Id = (int)reader["Id"],
                    Title = (string)reader["Title"],
                    Director = (string)reader["Director"],
                    ReleaseDate = (DateTime)reader["ReleaseDate"]
                });
            }
        }
        return movies;
    }

    public void AddMovie(Movie movie)
    {
        string query = "INSERT INTO Movies (Title, Director, ReleaseDate) VALUES (@Title, @Director, @ReleaseDate)";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Title", movie.Title);
            command.Parameters.AddWithValue("@Director", movie.Director);
            command.Parameters.AddWithValue("@ReleaseDate", movie.ReleaseDate);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }

    public void UpdateMovie(Movie movie)
    {
        string query = "UPDATE Movies SET Title = @Title, Director = @Director, ReleaseDate = @ReleaseDate WHERE Id = @Id";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Title", movie.Title);
            command.Parameters.AddWithValue("@Director", movie.Director);
            command.Parameters.AddWithValue("@ReleaseDate", movie.ReleaseDate);
            command.Parameters.AddWithValue("@Id", movie.Id);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }

    public void DeleteMovie(int id)
    {
        string query = "DELETE FROM Movies WHERE Id = @Id";
        using (SqlConnection connection = _dbService.GetConnection())
        {
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@Id", id);

            connection.Open();
            command.ExecuteNonQuery();
        }
    }
}
```

### 3. `Movie` Class

The `Movie` class remains the same as before.

```csharp
public class Movie
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Director { get; set; }
    public DateTime ReleaseDate { get; set; }
}
```

### 4. Usage Example

You can now create instances of `DbService` and `MovieService` without passing connection details from `Program.cs`.

```csharp
class Program
{
    static void Main(string[] args)
    {
        DbService dbService = new DbService();
        MovieService movieService = new MovieService(dbService);

        // Add a new movie
        Movie newMovie = new Movie
        {
            Title = "Inception",
            Director = "Christopher Nolan",
            ReleaseDate = new DateTime(2010, 7, 16)
        };
        movieService.AddMovie(newMovie);

        // Get all movies
        List<Movie> movies = movieService.GetAllMovies();
        foreach (var movie in movies)
        {
            Console.WriteLine($"{movie.Id} - {movie.Title} - {movie.Director} - {movie.ReleaseDate}");
        }

        // Update a movie
        Movie updatedMovie = movies[0];
        updatedMovie.Title = "Updated Title";
        movieService.UpdateMovie(updatedMovie);

        // Delete a movie
        movieService.DeleteMovie(updatedMovie.Id);
    }
}
```

In this updated architecture:
- `DbService` handles the connection setup internally.
- `MovieService` uses `DbService` to obtain a connection and perform SQL operations.

This approach centralizes connection management in the `DbService`, simplifying the interaction in other parts of your application.