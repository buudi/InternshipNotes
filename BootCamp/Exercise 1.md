make a console app

**CRUD**

it has a menu of movies
use local memory structures, like `List<T>`

Functionalities:
- add movies
- edit movie
- delete movie
- print all of the movies

exhibit the use of 
- LINQ
- Classes
- Project Structures (folders -> one for the data schema (classes), one for the Extension methods)
- and Extension Methods (if appropriate)

I'll be assessed based on
- the functionalities
- Clean Code (separation of concerns and use of cleaner syntax)
- proper use of structure
- feel free to do any extra feature

you're free to use whatever data schema, but keep it simple as next exercises are built on top of this exercise

can directly contact Arif on Teams throughout the weekend
also I have been added to dev Group

By Monday Morning


advice by Zhi: Search Dotnet Coding Conventions (naming conventions) -> clean code (could be)
https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md

Ideas:
- https://www.c-sharpcorner.com/UploadFile/13048b/model-validation-in-Asp-Net-mvc909/
- [official c# coding conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)

Building a Menu-Driven Console Application in C#
- https://www.codeproject.com/Articles/5382189/Building-a-Menu-Driven-Console-Application-in-Csha

## Implementation Plan

### Rough Requirements
display a menu :
1. List Movies
	- Print all movies in a nice format
	- Table?
2. Add a movie
	- Handle user input
	- Suitable Prompts
	- Data Validation, use `int.TryParse`
	- Suitable Error messages
	- Cancel option present at all times
3. Edit Movie
	- Print a list of movie titles with their Ids
	- User inputs the Id for the respective movie title
	- Suitable Prompts
	- Data Validation
	- Suitable Error messages
	- Cancel option present at all times
4. Delete Movie
	- Print a list of movie titles with their Ids
	- User inputs the Id for the respective movie title
	- Confirmation message for deleting the movie
	- Option to Delete or Cancel
	- Cancel option present at all times

### Approach
- Store menu options in an `Enum`, use `DescriptionAttribute` for providing readable descriptions to menu option.
- Or 
- [Make an interactive Menu](https://stackoverflow.com/questions/60767909/c-sharp-console-app-how-do-i-make-an-interactive-menu)




```c#
using System.Collections.Generic;
using System;
using System.Linq;
using System.Threading;

namespace MoviesMenu
{
    class Program
    {
        public static List<Option> options;
        public static MoviesController moviesController;
        
        static void Main(string[] args)
        {
            moviesController = new MoviesController();
            
            // Movie Menu Options
            options = new List<Option>
            {
                new Option("List all available movies.", () => ListAllMovies()),
                new Option("Add a new movie to the list.", () => AddMovie()),
                new Option("Modify an existing movie.", () => ModifyMovie()),
                new Option("Remove a movie from the list", () => DeleteMovie()),
                new Option("Exit the program.", () => Environment.Exit(0)),
            };

            // Set the default index of the selected item to be the first
            int index = 0;

            // Write the menu out
            WriteMenu(options, options[index]);

            // Store key info in here
            ConsoleKeyInfo keyinfo;
            do
            {
                keyinfo = Console.ReadKey();

                // Handle each key input (down arrow will write the menu again with a different selected item)
                if (keyinfo.Key == ConsoleKey.DownArrow)
                {
                    if (index + 1 < options.Count)
                    {
                        index++;
                        WriteMenu(options, options[index]);
                    }
                }
                if (keyinfo.Key == ConsoleKey.UpArrow)
                {
                    if (index - 1 >= 0)
                    {
                        index--;
                        WriteMenu(options, options[index]);
                    }
                }
                // Handle different action for the option
                if (keyinfo.Key == ConsoleKey.Enter)
                {
                    options[index].Selected.Invoke();
                    index = 0;
                }
            }
            while (keyinfo.Key != ConsoleKey.X);
        }

        static void ListAllMovies()
        {
            WriteTemporaryMessage(moviesController.ListAllMovies());
        }

        static void AddMovie()
        {
            Console.Clear();
            Console.WriteLine("Enter movie title:");
            string title = Console.ReadLine();
            Console.WriteLine("Enter director name:");
            string director = Console.ReadLine();
            Console.WriteLine("Enter genre:");
            string genre = Console.ReadLine();
            Console.WriteLine("Enter release year:");
            int releaseYear = int.Parse(Console.ReadLine());
            Console.WriteLine("Enter price:");
            double price = double.Parse(Console.ReadLine());

            string result = moviesController.AddMovie(new Movie(0, title, director, genre, releaseYear, price));
            WriteTemporaryMessage(result);
        }

        static void ModifyMovie()
        {
            Console.Clear();
            Console.WriteLine("Enter the ID of the movie to modify:");
            int id = int.Parse(Console.ReadLine());

            Console.WriteLine("Enter new movie title:");
            string title = Console.ReadLine();
            Console.WriteLine("Enter new director name:");
            string director = Console.ReadLine();
            Console.WriteLine("Enter new genre:");
            string genre = Console.ReadLine();
            Console.WriteLine("Enter new release year:");
            int releaseYear = int.Parse(Console.ReadLine());
            Console.WriteLine("Enter new price:");
            double price = double.Parse(Console.ReadLine());

            string result = moviesController.ModifyMovie(new Movie(id, title, director, genre, releaseYear, price));
            WriteTemporaryMessage(result);
        }

        static void DeleteMovie()
        {
            Console.Clear();
            Console.WriteLine("Enter the ID of the movie to delete:");
            int id = int.Parse(Console.ReadLine());

            string result = moviesController.DeleteMovie(id);
            WriteTemporaryMessage(result);
        }

        static void WriteTemporaryMessage(string message)
        {
            Console.Clear();
            Console.WriteLine(message);
            Console.WriteLine("\nPress 'b' to go back to the main menu.");

            while (true)
            {
                var key = Console.ReadKey(true).Key;
                if (key == ConsoleKey.B)
                {
                    WriteMenu(options, options.First());
                    break;
                }
            }
        }

        static void WriteMenu(List<Option> options, Option selectedOption)
        {
            Console.Clear();

            foreach (Option option in options)
            {
                if (option == selectedOption)
                {
                    Console.Write("> ");
                }
                else
                {
                    Console.Write(" ");
                }

                Console.WriteLine(option.Name);
            }
        }
    }

    public class Option
    {
        public string Name { get; }
        public Action Selected { get; }

        public Option(string name, Action selected)
        {
            Name = name;
            Selected = selected;
        }
    }

    internal class MoviesController
    {
        private List<Movie> movies;

        public MoviesController()
        {
            movies = new List<Movie>
            {
                new Movie(0, "Inception", "Christopher Nolan", "Sci-fi/Action", 2010, 24.99),
                new Movie(1, "The Matrix", "Lilly Wachowski", "Sci-fi/Action", 1999, 19.99),
                new Movie(2, "Interstellar", "Christopher Nolan", "Sci-fi", 2014, 29.99)
            };
        }

        public string ListAllMovies()
        {
            if (movies.Count == 0)
            {
                return "No movies available.";
            }

            var result = "Movies:\n";
            foreach (var movie in movies)
            {
                result += $"ID: {movie.Id}, Title: {movie.Title}, Director: {movie.Director}, Genre: {movie.Genre}, Year: {movie.ReleaseYear}, Price: {movie.Price}\n";
            }
            return result;
        }

        public string AddMovie(Movie movie)
        {
            movie.Id = movies.Any() ? movies.Max(m => m.Id) + 1 : 0;
            movies.Add(movie);
            return "Movie added successfully.";
        }

        public string ModifyMovie(Movie updatedMovie)
        {
            var movie = movies.FirstOrDefault(m => m.Id == updatedMovie.Id);
            if (movie == null)
            {
                return "Movie not found.";
            }

            movie.Title = updatedMovie.Title;
            movie.Director = updatedMovie.Director;
            movie.Genre = updatedMovie.Genre;
            movie.ReleaseYear = updatedMovie.ReleaseYear;
            movie.Price = updatedMovie.Price;
            return "Movie updated successfully.";
        }

        public string DeleteMovie(int id)
        {
            var movie = movies.FirstOrDefault(m => m.Id == id);
            if (movie == null)
            {
                return "Movie not found.";
            }

            movies.Remove(movie);
            return "Movie deleted successfully.";
        }
    }

    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Director { get; set; }
        public string Genre { get; set; }
        public int ReleaseYear { get; set; }
        public double Price { get; set; }

        public Movie(int id, string title, string director, string genre, int releaseYear, double price)
        {
            Id = id;
            Title = title;
            Director = director;
            Genre = genre;
            ReleaseYear = releaseYear;
            Price = price;
        }
    }
}

```














### Modify Movie

```c#
 static void ModifyMovie()
        {
            Console.Clear();
            Console.WriteLine("Enter the ID of the movie to modify:");
            int id = int.Parse(Console.ReadLine());

            Console.WriteLine("Enter new movie title:");
            string title = Console.ReadLine();
            Console.WriteLine("Enter new director name:");
            string director = Console.ReadLine();
            Console.WriteLine("Enter new genre:");
            string genre = Console.ReadLine();
            Console.WriteLine("Enter new release year:");
            int releaseYear = int.Parse(Console.ReadLine());
            Console.WriteLine("Enter new price:");
            double price = double.Parse(Console.ReadLine());

            string result = moviesController.ModifyMovie(new Movie(id, title, director, genre, releaseYear, price));
            WriteTemporaryMessage(result);
        }
```



### Delete Movie
```c#
public string DeleteMovie(int id)
        {
            var movie = movies.FirstOrDefault(m => m.Id == id);
            if (movie == null)
            {
                return "Movie not found.";
            }

            movies.Remove(movie);
            return "Movie deleted successfully.";
        }
```



```c#
public string DeleteMovie(int id)
        {
            var movie = movies.FirstOrDefault(m => m.Id == id);
            if (movie == null)
            {
                return "Movie not found.";
            }

            movies.Remove(movie);
            return "Movie deleted successfully.";
        }
```


### delete movie
```c#
 static void DeleteMovie()
        {
            Console.Clear();
            Console.WriteLine("Enter the ID of the movie to delete:");
            int id = int.Parse(Console.ReadLine());

            string result = moviesController.DeleteMovie(id);
            WriteTemporaryMessage(result);
        }
```



```c#
 // Default action of all the options. You can create more methods
 static void WriteTemporaryMessage(string message)
 {
     Console.Clear();
     Console.WriteLine(message);
     Console.WriteLine("\nPress 'b' to go back to the main menu.");

     while (true)
     {
         var key = Console.ReadKey(true).Key;
         if (key == ConsoleKey.B)
         {
             WriteMenu(options, options[0]);
             break;
         }
     }
 }
```