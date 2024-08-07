
My Connection String (work laptop): 
- `Server=localhost\SQLEXPRESS;Database=master;Trusted_Connection=True;`
- `Server=localhost\SQLEXPRESS;Database=MyBootcamp;User Id=Bootcamp;Password=password;TrustServerCertificate=True;`
- [More connection string formats](https://www.connectionstrings.com/sql-server/)
### 1. Adding ADO.NET to the Project

```bash
dotnet add package System.Data.SqlClient
```
Or add Nuget package in visual studio
### 2. Establishing a Connection

A connection string typically looks like this:

For my case it is:
```c#
string connectionString = "Server=localhost\\SQLEXPRESS;Database=MyBootcamp;Trusted_Connection=True;"; 
```

Another connection string could be:
```csharp
string connectionString = "Server=your_server_name;Database=your_database_name;User Id=your_username;Password=your_password;";
```

```c#
optionsBuilder.UseSqlServer("Server=localhost\\SQLEXPRESS;Database=MyBootcamp;User Id=Bootcamp;Password=password;TrustServerCertificate=True;");
```

### 3. Creating the Connection and Executing Commands

using `SqlConnection`, `SqlCommand`, and `SqlDataReader`:

```csharp
using System;
using System.Data.SqlClient;

namespace AdoNetExample
{
    class Program
    {
        static void Main(string[] args)
        {
            string connectionString = "Server=your_server_name;Database=your_database_name;User Id=your_username;Password=your_password;";

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                try
                {
                    connection.Open();
                    Console.WriteLine("Connection opened successfully.");

                    string query = "SELECT * FROM YourTable";
                    SqlCommand command = new SqlCommand(query, connection);

                    SqlDataReader reader = command.ExecuteReader();

                    while (reader.Read())
                    {
                        Console.WriteLine($"{reader["ColumnName1"]}, {reader["ColumnName2"]}");
                    }
                    reader.Close();
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Error: " + ex.Message);
                }
                finally
                {
                    connection.Close();
                    Console.WriteLine("Connection closed.");
                }
            }
        }
    }
}
```

### 5. Performing CRUD Operations

#### Create (Insert)

```csharp
string insertQuery = "INSERT INTO YourTable (ColumnName1, ColumnName2) VALUES (@Value1, @Value2)";
using (SqlCommand command = new SqlCommand(insertQuery, connection))
{
    command.Parameters.AddWithValue("@Value1", value1);
    command.Parameters.AddWithValue("@Value2", value2);
    int result = command.ExecuteNonQuery();
    Console.WriteLine($"{result} rows inserted.");
}
```

#### Read (Select)

```csharp
string selectQuery = "SELECT * FROM YourTable";
using (SqlCommand command = new SqlCommand(selectQuery, connection))
{
    using (SqlDataReader reader = command.ExecuteReader())
    {
        while (reader.Read())
        {
            Console.WriteLine($"{reader["ColumnName1"]}, {reader["ColumnName2"]}");
        }
    }
}
```

#### Update

```csharp
string updateQuery = "UPDATE YourTable SET ColumnName1 = @Value1 WHERE ColumnName2 = @Value2";
using (SqlCommand command = new SqlCommand(updateQuery, connection))
{
    command.Parameters.AddWithValue("@Value1", newValue1);
    command.Parameters.AddWithValue("@Value2", conditionValue2);
    int result = command.ExecuteNonQuery();
    Console.WriteLine($"{result} rows updated.");
}
```

#### Delete

```csharp
string deleteQuery = "DELETE FROM YourTable WHERE ColumnName1 = @Value1";
using (SqlCommand command = new SqlCommand(deleteQuery, connection))
{
    command.Parameters.AddWithValue("@Value1", value1);
    int result = command.ExecuteNonQuery();
    Console.WriteLine($"{result} rows deleted.");
}
```
