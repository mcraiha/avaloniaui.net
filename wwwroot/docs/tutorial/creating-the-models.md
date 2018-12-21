Title: Creating the Models
Order: 20
---

The first thing to do is to create the models which represent our data as it would be stored in a database. Our model is going to be pretty simple: each TODO item will consist of a textual description and a boolean value representing whether the item is checked.

`Models/TodoItem.cs`:
```csharp
namespace Todo.Models
{
    public class TodoItem
    {
        public string Description { get; set; }
        public bool IsChecked { get; set; }
    }
}
```

 We're not actually going to be using a database for the sake of this example, we'll just populate our models from an array. We'll do this in a service called `Database` and put this in a `Services` directory:

`Services/Database.cs`:
 ```csharp
using System.Collections.Generic;
using Todo.Models;

 namespace Todo.Services
{
    public class Database
    {
        public IEnumerable<TodoItem> GetItems() => new[]
        {
            new TodoItem { Description = "Buy milk" },
            new TodoItem { Description = "Walk dog" },
        };
    }
}
 ```

 Usually if you're writing a CRUD application, this data would be read from a database using Entity Framework or a similar ORM, or it could be read from a a web API or a file etc.