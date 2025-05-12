
---

## 🎯 Entity Framework Fundamentals

### 1. What is an ORM and how does Entity Framework function as one?
**Understanding ORM (Object-Relational Mapping)**

* **Definition**: ORM stands for Object-Relational Mapping. It's a programming technique that helps developers interact with a database using object-oriented code instead of SQL queries.
* **Goal**: It bridges the gap between object-oriented languages like C# and relational databases like SQL Server.
* **How it Works**: ORM maps database tables to classes, table columns to class properties, and rows to objects. This way, you manipulate data as objects rather than writing SQL manually.

**Entity Framework as an ORM**

* **Abstraction Layer**: Entity Framework (EF) acts as an abstraction layer that handles database operations behind the scenes. Developers write LINQ queries, and EF converts them into SQL.
* **Model Creation**: EF uses models (classes) to represent database tables. You can create these models manually (Code First), generate them from an existing database (Database First), or use a hybrid (Model First).
* **Change Tracking**: EF tracks changes in objects and automatically generates the appropriate `INSERT`, `UPDATE`, or `DELETE` queries when `SaveChanges()` is called.
* **Context Class**: The `DbContext` class manages the connection and acts as a unit-of-work and repository. It keeps track of entities and coordinates database operations.
* **Lazy & Eager Loading**: EF supports different loading techniques to control when related data is fetched — lazy (on-demand), eager (with main query), and explicit loading.
* **Migration Support**: In Code First, EF provides migration tools to keep the database schema in sync with model changes without data loss.

**Answer Summary**

* ORM maps classes to tables and properties to columns.
* Entity Framework is an ORM that lets you work with databases using C# objects.
* It eliminates the need for most SQL queries by translating LINQ to SQL.
* Uses `DbContext` to manage data and track changes.
* Supports Code First, Database First, and Model First approaches.
* Automatically handles inserts, updates, and deletes via `SaveChanges()`.
* Offers lazy, eager, and explicit loading for related data.
* Code First includes migrations to manage schema changes.

<br>

### 2. Can you explain the architecture of Entity Framework?
**Entity Framework Architecture Overview**
Entity Framework architecture is layered and modular, helping developers interact with databases using object-oriented concepts. The main layers are:

---

**1. Conceptual Model (CSDL)**

* Represents the application’s domain using entity classes and relationships.
* This is the object-oriented view of your data — what your C# classes look like.
* Defines entities, their properties, and associations.

---

**2. Mapping Layer (MSL)**

* Acts as a bridge between the conceptual model and the storage model.
* Defines how the entities and properties in the conceptual model map to database tables and columns.
* Ensures EF knows how to translate between the two.

---

**3. Storage Model (SSDL)**

* Represents the actual database schema: tables, views, columns, keys, and relationships.
* Defines what exists in the underlying database.
* This is the database view used by EF for actual data operations.

---

**4. Object Services**

* Responsible for materializing data into entity objects.
* Handles query execution, change tracking, and identity resolution.
* Allows developers to write queries using LINQ and convert results into objects.

---

**5. Entity Client Data Provider**

* Translates LINQ or Entity SQL queries into commands understood by the underlying database.
* Acts like a bridge between EF and ADO.NET.
* Generates SQL commands from higher-level queries.

---

**6. ADO.NET Data Provider**

* Sends the generated SQL to the database and returns results back to the EF stack.
* Handles connection management, command execution, and reading data.
* Works similarly to how ADO.NET works directly without EF.

---

**7. Database**

* The physical storage of data.
* Could be SQL Server, MySQL, PostgreSQL, etc.
* Actual location where all CRUD operations are executed.

---

**DbContext and DbSet**

* `DbContext`: Central class coordinating EF functionality like tracking, change detection, and query execution.
* `DbSet<T>`: Represents a table in the database; used for CRUD operations on entities of type `T`.

---

**Answer Summary**

* EF architecture has multiple layers: Conceptual (CSDL), Mapping (MSL), and Storage (SSDL).
* `Object Services` handle queries, change tracking, and data shaping.
* `Entity Client` translates LINQ to SQL.
* `ADO.NET Provider` sends SQL to the actual database.
* `DbContext` is the main class for managing EF operations.
* EF abstracts the database through a clean, layered design.

<br>

### 3. What are the main differences between Entity Framework and LINQ to SQL?
**Supported Databases**

* **Entity Framework**: Supports multiple databases like SQL Server, MySQL, PostgreSQL, SQLite, Oracle, etc.
* **LINQ to SQL**: Works **only with Microsoft SQL Server**.

---

**Modeling Capabilities**

* **Entity Framework**: Supports **complex types**, **inheritance**, and **relationships like many-to-many**.
* **LINQ to SQL**: Limited modeling; supports **only one-to-one and one-to-many** relationships, no many-to-many support.
* EF provides a **richer and more flexible modeling system**.

---

**Approaches to Model Creation**

* **Entity Framework**: Offers **Code First**, **Database First**, and **Model First** approaches.
* **LINQ to SQL**: Only **Database First**, meaning models must be generated from an existing database.

---

**Architecture and Abstraction**

* **Entity Framework**: Multi-layered architecture with **CSDL, SSDL, and MSL layers**; follows **Entity Data Model (EDM)**.
* **LINQ to SQL**: Much simpler architecture; **tightly coupled with SQL Server schema**.

---

**Performance and Control**

* **LINQ to SQL**: Lightweight and may be **faster for small applications** due to less overhead.
* **Entity Framework**: Slightly heavier but **more powerful** for large-scale applications.

---

**Change Tracking & Lazy Loading**

* **Entity Framework**: Supports **lazy loading**, **eager loading**, and **explicit loading**; built-in **change tracking**.
* **LINQ to SQL**: Has **basic change tracking** and **limited support** for lazy loading.

---

**Community & Future Support**

* **Entity Framework**: Actively developed with full support from Microsoft. EF Core is the modern version.
* **LINQ to SQL**: Deprecated and **no longer actively maintained**.

---

**Answer Summary**

* EF supports many databases; LINQ to SQL only supports SQL Server.
* EF allows Code First, Database First, and Model First; LINQ to SQL supports only Database First.
* EF handles complex relationships, inheritance, and mapping; LINQ to SQL has limited capabilities.
* EF is better for enterprise apps; LINQ to SQL is simpler but outdated.
* EF is still supported and actively developed; LINQ to SQL is discontinued.

<br>

### 4. What is DbContext and how is it used in EF?
**What is DbContext?**

* `DbContext` is the **primary class** in Entity Framework responsible for interacting with the database.
* It acts as a **bridge** between your application’s domain classes and the underlying database.
* It is a wrapper over the `ObjectContext` class and provides a **simpler API** for developers.

---

**Core Responsibilities of DbContext**

1. **Querying**:

   * Allows querying data using **LINQ to Entities**.
   * Example: `context.Employees.Where(e => e.Department == "IT")`

2. **Change Tracking**:

   * Tracks changes made to objects so it knows what to insert, update, or delete when `SaveChanges()` is called.

3. **Persisting Data (SaveChanges)**:

   * Applies changes made to objects back to the database with `context.SaveChanges()`.

4. **Model Management**:

   * Builds and manages the in-memory model of your database schema.

5. **Relationship Management**:

   * Manages navigation properties and related data loading (lazy, eager, explicit).

---

**Common Members of DbContext**

* `DbSet<TEntity>`: Represents a table in the database and allows CRUD operations on the entity.
* `OnModelCreating(ModelBuilder modelBuilder)`: Allows you to customize model configuration using Fluent API.
* `SaveChanges()`: Commits all changes to the database.
* `Database`: Provides access to methods like `BeginTransaction`, `EnsureCreated`, `Migrate`, etc.

---

**Creating and Using a DbContext**

```csharp
public class AppDbContext : DbContext
{
    public DbSet<Employee> Employees { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Your_Connection_String");
    }
}
```

**Usage Example**

```csharp
using (var context = new AppDbContext())
{
    var employees = context.Employees.ToList();  // Query
    context.Employees.Add(new Employee { Name = "Sajid" });  // Add
    context.SaveChanges();  // Save to DB
}
```

---

**Answer Summary**

* `DbContext` is the main class for database communication in EF.
* Handles querying, change tracking, saving data, and managing relationships.
* Uses `DbSet<TEntity>` for working with tables.
* `SaveChanges()` applies object changes to the database.
* Supports configuration through `OnModelCreating` and connection setup in `OnConfiguring`.
* Simplifies working with data using object-oriented code.

<br>

### 5. What is the purpose of DbSet in Entity Framework?
**Definition and Purpose of DbSet**

* `DbSet<TEntity>` is a **generic class** in Entity Framework that represents a **collection of all entities of a given type** in the context or the database.
* It is essentially the **table representation** in the application’s code.
* Enables CRUD (Create, Read, Update, Delete) operations on the corresponding entity.

---

**How It Works**

* Each `DbSet` property inside a `DbContext` represents a **table in the database**.
* EF uses it to **track** and **query** entities of that type.
* Behind the scenes, EF translates operations on `DbSet` into SQL commands.

---

**Declaring DbSet in DbContext**

```csharp
public class AppDbContext : DbContext
{
    public DbSet<Employee> Employees { get; set; }
    public DbSet<Department> Departments { get; set; }
}
```

* `Employees` corresponds to the `Employee` table.
* `Departments` corresponds to the `Department` table.

---

**Common Operations with DbSet**

1. **Querying Data**

   ```csharp
   var employees = context.Employees.ToList();
   var singleEmp = context.Employees.FirstOrDefault(e => e.Id == 1);
   ```

2. **Adding Records**

   ```csharp
   context.Employees.Add(new Employee { Name = "Sajid" });
   context.SaveChanges();
   ```

3. **Updating Records**

   ```csharp
   var emp = context.Employees.Find(1);
   emp.Name = "Updated Name";
   context.SaveChanges();
   ```

4. **Deleting Records**

   ```csharp
   var emp = context.Employees.Find(1);
   context.Employees.Remove(emp);
   context.SaveChanges();
   ```

---

**Integration with LINQ**

* `DbSet` supports LINQ queries:

  ```csharp
  var itEmployees = context.Employees.Where(e => e.Department == "IT").ToList();
  ```

---

**Answer Summary**

* `DbSet<TEntity>` represents a table in the database.
* Enables querying, adding, updating, and deleting records.
* Works closely with `DbContext` to track and persist entity changes.
* Fully supports LINQ queries for easy and type-safe data access.
* Automatically maps entity classes to database tables.

<br>

### 6. Can you describe the concept of migrations in EF?
**What Are Migrations in Entity Framework?**

* Migrations in Entity Framework are a way to **keep the database schema in sync** with your model classes.
* They track changes to your entity classes and **apply those changes to the database schema automatically**.
* This avoids the need to manually update the database every time your model changes.

---

**Why Migrations Are Useful**

* Automates schema updates and version control for the database.
* Helps avoid manual SQL scripts.
* Allows teams to track database changes in source control.
* Ensures that production and development databases stay consistent.

---

**Types of Migrations**

1. **Code-Based Migrations**

   * Managed entirely in code using C# classes.
   * Can be customized using `Up()` and `Down()` methods.

2. **Automatic Migrations (EF 6)**

   * EF generates and applies migrations automatically.
   * Not available in EF Core (encouraged to use code-based migrations).

---

**Common Migration Commands (EF Core)**

1. `Add-Migration <MigrationName>`

   * Creates a new migration class based on changes in the model.

2. `Update-Database`

   * Applies the migration to the database.

3. `Remove-Migration`

   * Removes the last (unapplied) migration.

4. `Script-Migration`

   * Generates a SQL script for the migration.

---

**Migration Class Structure Example**

```csharp
public partial class AddEmployeeTable : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.CreateTable(
            name: "Employees",
            columns: table => new
            {
                Id = table.Column<int>(nullable: false),
                Name = table.Column<string>(nullable: true)
            },
            constraints: table => { table.PrimaryKey("PK_Employees", x => x.Id); });
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropTable(name: "Employees");
    }
}
```

---

**Migration History Table**

* EF adds a special table called `__EFMigrationsHistory` to track which migrations have been applied.
* Prevents reapplying the same migration multiple times.

---

**Best Practices**

* Name your migrations clearly (e.g., `AddEmployeeTable`, `UpdateSalaryColumn`).
* Always review and test migrations before applying them to production.
* Keep your `DbContext` and migration history in version control.

---

**Answer Summary**

* Migrations let you update your database schema to match your model changes.
* Use commands like `Add-Migration` and `Update-Database` to manage schema changes.
* Migrations are tracked in the `__EFMigrationsHistory` table.
* Provides a structured and version-controlled approach to evolving your database.
* Ideal for collaborative development and CI/CD pipelines.

<br>

### 7. How does EF implement code-first, database-first, and model-first approaches?
**Code-First Approach**

* In the **Code-First** approach, you define your domain model (entities) as C# classes first. The database is then generated based on these classes.
* **Migrations** help in evolving the database schema when the model changes.
* The **DbContext** class is used to manage the database connection and set up entity relationships.

**Steps for Code-First**

1. **Create Entity Classes**: Define your model as plain C# classes.

   ```csharp
   public class Employee
   {
       public int Id { get; set; }
       public string Name { get; set; }
   }
   ```

2. **Create DbContext Class**: The `DbContext` class represents the session between the application and the database.

   ```csharp
   public class AppDbContext : DbContext
   {
       public DbSet<Employee> Employees { get; set; }
   }
   ```

3. **Run Migration Commands**: Use the `Add-Migration` and `Update-Database` commands to generate and apply the database schema.

   * `Add-Migration InitialCreate`
   * `Update-Database`

**Advantages**

* Total control over the model and database schema.
* Easier to maintain in version control.
* Schema changes can be tracked with migrations.

---

**Database-First Approach**

* In the **Database-First** approach, you start with an existing database. EF generates the entity classes based on the existing schema.
* **Reverse Engineering** is done using tools like **EF Designer** or **Scaffolding** to generate the code.

**Steps for Database-First**

1. **Create Database**: First, create your database manually or through another tool.

2. **Scaffold Entity Classes**: Use the `Scaffold-DbContext` command to generate the entity classes and `DbContext` from the existing database schema.

   ```bash
   Scaffold-DbContext "YourConnectionString" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
   ```

3. **Use the Generated Code**: The generated classes now represent the database schema, and you can work with them using LINQ queries, CRUD operations, etc.

**Advantages**

* Ideal when working with an existing database.
* Generates a model based on actual data and schema.

---

**Model-First Approach**

* In the **Model-First** approach, you define the model visually using the **Entity Framework Designer** (EF Designer), and the database is generated from the model.
* This approach is less common in EF Core but can still be used in EF 6.

**Steps for Model-First**

1. **Create Model in Designer**: Use EF Designer to visually design your model by dragging entities and defining relationships.

2. **Generate Database**: After defining the model, you generate the database schema using the designer.

**Advantages**

* Visual representation of the model.
* Easier to understand for people who prefer a graphical approach.

---

**Answer Summary**

* **Code-First**: Defines models as C# classes, and the database is generated from these classes. Migrations help evolve the schema.
* **Database-First**: Works with an existing database, and entity classes are generated automatically using scaffolding.
* **Model-First**: Defines a model visually using EF Designer, and the database is generated from the model.
* **Code-First** is best for new projects, while **Database-First** is suitable for working with an existing database. **Model-First** is a less common approach but provides a visual tool for designing the schema.

<br>

### 8. What is the role of the EdmGen tool in EF?
**Role of the EdmGen Tool in EF**

* **EdmGen** (Entity Data Model Generator) is a command-line tool used in **Entity Framework** to generate **metadata files** for a model. It was primarily used in **EF 4.x** and earlier versions (EF 6 and older) to create **EDMX files** that describe the structure of the data model.

* **EDMX File**: The EDMX file is an XML file that defines the **Entity Data Model (EDM)**, which describes the entities, their properties, relationships, and mappings between the model and the database.

---

**Purpose and Functions**

1. **Generate EDMX Files**:

   * It was used to generate `.edmx` files for **Database-First** and **Model-First** approaches, representing the schema of a database in a visual format.

2. **Reverse Engineering**:

   * The EdmGen tool could reverse engineer an existing database schema and create a `.edmx` file that can then be used in the application.

3. **Metadata Generation**:

   * It helps in creating metadata that defines the shape of entities, their relationships, and mappings between the conceptual model and the storage model.

---

**EdmGen Tool in EF**

* The tool was primarily used in the **Database-First** approach to automatically generate the **Entity Framework model** based on an existing database.
* It could be used in scenarios where a custom **metadata file** was needed for a project. However, **EF Core** no longer uses `EdmGen` as it does not use `.edmx` files and follows a more code-first approach for modeling.

---

**Command Example**

```bash
EdmGen.exe /mode:edmx /connectionstring:"YourConnectionString" /out:Model.edmx
```

* This would generate an **EDMX file** for the database.

---

**Answer Summary**

* The **EdmGen tool** was used in **Entity Framework (EF 4.x and earlier)** to generate **EDMX metadata files** for models, primarily used in **Database-First** and **Model-First** approaches.
* It reverse-engineered the database schema and created an EDMX file to describe entities, their relationships, and mappings.
* **EF Core** does not use the EdmGen tool, as it no longer relies on EDMX files, focusing on a more code-first approach.

<br>

### 9. Can you describe the Entity Data Model (EDM) in EF?
**What is the Entity Data Model (EDM) in Entity Framework?**

* The **Entity Data Model (EDM)** is the core abstraction that defines the structure of data in Entity Framework (EF).
* It describes the **conceptual model**, **storage model**, and **mapping between the two**.

  * **Conceptual Model (CSDL)**: Describes the entities, their properties, and relationships in an application.
  * **Storage Model (SSDL)**: Describes the physical database structure, like tables and columns.
  * **Mapping (MSL)**: Maps the conceptual model to the storage model.

---

**Components of the EDM**

1. **Conceptual Schema Definition Language (CSDL)**

   * Defines the **entities**, **properties**, **relationships**, and **complex types** that are part of the application model.
   * This is the model you work with in your application.

   Example:

   ```xml
   <EntityType Name="Employee">
       <Key>
           <PropertyRef Name="EmployeeId" />
       </Key>
       <Property Name="EmployeeId" Type="Int32" Nullable="false" />
       <Property Name="Name" Type="String" Nullable="false" />
   </EntityType>
   ```

2. **Storage Schema Definition Language (SSDL)**

   * Represents the **physical structure** of the database (tables, columns, foreign keys, etc.).
   * It’s often generated automatically by EF based on the database.

   Example:

   ```xml
   <EntityContainer Name="YourDbContext">
       <EntitySet Name="Employees" EntityType="Self.Employee" />
   </EntityContainer>
   ```

3. **Mapping Specification Language (MSL)**

   * Defines how the **conceptual model** is mapped to the **storage model** (how entities are mapped to tables, properties to columns).
   * It's automatically generated in many cases when working with **Database-First** or **Model-First** approaches.

   Example:

   ```xml
   <EntitySetMapping Name="Employees">
       <EntityTypeMapping Type="Self.Employee">
           <MappingFragment StoreEntitySet="Employees">
               <ScalarProperty Name="EmployeeId" ColumnName="EmployeeId" />
               <ScalarProperty Name="Name" ColumnName="Name" />
           </MappingFragment>
       </EntityTypeMapping>
   </EntitySetMapping>
   ```

---

**How EDM Works in Entity Framework**

* **Conceptual Model** (CSDL) is what you work with in your application code (your classes).
* **Storage Model** (SSDL) corresponds to the actual database structure (e.g., tables, columns, and relationships).
* **Mapping** (MSL) ensures that when you query or update entities, EF correctly maps your model to the database.

---

**EDM and EDMX File**

* In **EF 6 and earlier**, the EDM was typically represented using an **EDMX file**, which contains the CSDL, SSDL, and MSL in XML format.
* In **EF Core**, EDMs are generated via **code-first** and **scaffolding** approaches, and EDMX is no longer used.

---

**Key Concepts**

* **Entities**: Classes that represent data (like `Employee`, `Product`).
* **Properties**: Fields or attributes of entities (like `EmployeeId`, `Name`).
* **Relationships**: Associations between entities (like `one-to-many`, `many-to-many`).
* **Complex Types**: Types that don't have a primary key but are part of the model (like `Address`).

---

**Answer Summary**

* The **Entity Data Model (EDM)** is a key abstraction in **Entity Framework** that defines the structure of data, including the **conceptual model**, **storage model**, and **mapping** between the two.
* It consists of **CSDL** (conceptual model), **SSDL** (storage model), and **MSL** (mapping).
* In **EF 6**, EDM is stored in an **EDMX** file, but **EF Core** uses a **code-first** approach instead.
* The EDM describes how entities are structured and how they relate to each other in the database.

<br>

### 10. How does lazy loading work in EF?
**Lazy Loading in Entity Framework**

**What is Lazy Loading?**

* **Lazy Loading** is a design pattern used in **Entity Framework** (EF) to load related data only when it's needed, rather than loading it upfront with the main query. This can help improve application performance by reducing the initial load time when you don’t need all related data immediately.

---

**How Lazy Loading Works in EF**

* When **lazy loading** is enabled, EF automatically loads related entities only when you access a navigation property for the first time. For example, if you have an `Order` entity with a `Customer` navigation property, the `Customer` data will not be loaded until you explicitly access it.

  Example:

  ```csharp
  var order = context.Orders.FirstOrDefault();
  var customer = order.Customer;  // Lazy load: the customer is loaded from the database at this point
  ```

* This is achieved using **proxy classes** that EF creates at runtime. These proxy classes override the navigation properties and add code to trigger the lazy loading when a property is accessed.

---

**Configuring Lazy Loading**

* **Enable Lazy Loading**:
  In EF 6 and earlier, lazy loading is enabled by default. For EF Core, it needs to be explicitly enabled by using **`Microsoft.EntityFrameworkCore.Proxies`**.

  Steps to enable lazy loading in EF Core:

  1. Install the required NuGet package:

     ```bash
     Install-Package Microsoft.EntityFrameworkCore.Proxies
     ```

  2. Enable lazy loading in `OnConfiguring` method of `DbContext`:

     ```csharp
     public class AppDbContext : DbContext
     {
         protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
         {
             optionsBuilder.UseLazyLoadingProxies();  // Enable lazy loading
         }
     }
     ```

  3. Make navigation properties **virtual**:

     ```csharp
     public virtual Customer Customer { get; set; }  // Mark navigation property as virtual
     ```

---

**How Lazy Loading Works Internally**

1. **Proxy Generation**:
   EF creates a proxy class for the entity that contains the navigation property.

2. **Intercept Access**:
   When you access the navigation property, EF checks if the related data is already loaded. If it is not, EF performs a database query to load the related data.

3. **Database Query**:
   EF issues a new query to load the related data only when the navigation property is accessed for the first time.

---

**Advantages of Lazy Loading**

1. **Improved Performance**:
   It helps improve initial load time by loading related data only when needed.

2. **Less Data Retrieval**:
   Reduces the amount of data retrieved from the database, particularly when you don't need all related data.

---

**Disadvantages of Lazy Loading**

1. **Multiple Database Queries**:
   Lazy loading can result in **N+1 query problem**, where multiple database queries are issued to load related data, potentially leading to performance issues.

   Example:

   ```csharp
   var orders = context.Orders.ToList();  // N+1 problem: separate query for each order's customer
   foreach (var order in orders)
   {
       var customer = order.Customer;  // Lazy load: additional query for each order's customer
   }
   ```

2. **Unexpected Queries**:
   It can lead to unexpected database queries if you're not careful about when navigation properties are accessed.

---

**Avoiding N+1 Problem**
To mitigate the N+1 problem, you can use **Eager Loading** or **Explicit Loading**:

1. **Eager Loading**:
   Load related data upfront with the initial query using the `Include` method.

   ```csharp
   var orders = context.Orders.Include(o => o.Customer).ToList();  // Load customer data in the same query
   ```

2. **Explicit Loading**:
   Load related data after the main query with explicit calls.

   ```csharp
   var order = context.Orders.First();
   context.Entry(order).Reference(o => o.Customer).Load();  // Explicitly load customer data
   ```

---

**Answer Summary**

* **Lazy Loading** in EF loads related data only when the navigation property is accessed for the first time, improving initial performance by avoiding unnecessary data retrieval.
* In **EF 6**, lazy loading is enabled by default, while in **EF Core**, you need to enable it by using **Microsoft.EntityFrameworkCore.Proxies** and marking navigation properties as **virtual**.
* **Disadvantages** include the **N+1 query problem** and unexpected database queries.
* To mitigate issues, you can use **Eager Loading** (`Include`) or **Explicit Loading**.

<br>

## ⚙️ Entity Framework Configuration and Setup

### 11. How do you install or upgrade Entity Framework in a .NET project?
**Installing or Upgrading Entity Framework in a .NET Project**

**1. Installing Entity Framework**
To install **Entity Framework (EF)** in a **.NET project**, you can use **NuGet** (the package manager for .NET). Depending on whether you're using **EF 6** or **EF Core**, the installation steps differ slightly.

---

**For Entity Framework 6 (EF 6)**

1. **Using NuGet Package Manager Console**:
   Open the **Package Manager Console** in Visual Studio (`Tools` > `NuGet Package Manager` > `Package Manager Console`) and run the following command:

   ```bash
   Install-Package EntityFramework
   ```

   This will install the latest version of **EF 6** for your project.

2. **Using the NuGet Package Manager GUI**:

   * Right-click on your project in the **Solution Explorer**.
   * Select **Manage NuGet Packages**.
   * Search for **EntityFramework** and click **Install**.

---

**For Entity Framework Core (EF Core)**

1. **Using NuGet Package Manager Console**:
   Open the **Package Manager Console** and run the following command:

   ```bash
   Install-Package Microsoft.EntityFrameworkCore
   ```

   This will install the latest version of **EF Core** for your project.

2. **Using the NuGet Package Manager GUI**:

   * Right-click on your project and select **Manage NuGet Packages**.
   * Search for **Microsoft.EntityFrameworkCore** and click **Install**.

---

**2. Upgrading Entity Framework**
To **upgrade** an existing **Entity Framework** version in a .NET project, you need to update the NuGet package to the latest version or a specific version.

1. **Using NuGet Package Manager Console**:

   * Open the **Package Manager Console** and run the following command to update EF 6 or EF Core:

   ```bash
   Update-Package EntityFramework    // For EF 6
   ```

   or

   ```bash
   Update-Package Microsoft.EntityFrameworkCore    // For EF Core
   ```

2. **Using the NuGet Package Manager GUI**:

   * Open **Manage NuGet Packages** for your project.
   * Go to the **Updates** tab.
   * Find **EntityFramework** or **Microsoft.EntityFrameworkCore**, and click **Update**.

---

**3. Installing/Upgrading Additional EF Packages**
In some cases, you might need to install or upgrade additional **EF-related packages**, such as:

* **EF Tools** for command-line operations:

  ```bash
  Install-Package Microsoft.EntityFrameworkCore.Tools
  ```

* **EF Provider** for a specific database (e.g., SQL Server):

  ```bash
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* **EF Design** for design-time services:

  ```bash
  Install-Package Microsoft.EntityFrameworkCore.Design
  ```

---

**Answer Summary**

* **To install** Entity Framework (EF), use the NuGet Package Manager Console:

  * `Install-Package EntityFramework` (for EF 6)
  * `Install-Package Microsoft.EntityFrameworkCore` (for EF Core).
* **To upgrade** EF, use `Update-Package EntityFramework` (for EF 6) or `Update-Package Microsoft.EntityFrameworkCore` (for EF Core) in the Package Manager Console.
* You can also use the **NuGet GUI** to install, update, or upgrade packages.

<br>

### 12. What is the difference between local and global configuration in EF?
**Difference Between Local and Global Configuration in Entity Framework**

**Local Configuration**

* **Local configuration** refers to the **settings or configurations** that are applied to a specific **DbContext** or **entity class**.
* These configurations only apply to the context or entity where they are defined. They are typically defined inside the `OnModelCreating` method or via attributes on the entity classes.

  **Key Points of Local Configuration**:

  * Configurations are applied **only to a specific DbContext** or entity.
  * Defined within the **DbContext** class, usually inside the `OnModelCreating` method.
  * Provides flexibility to configure different models (tables) in different contexts.
  * Often used when you need specific configurations for a model, like setting a custom table name, configuring relationships, or modifying column properties.

  **Example**:

  ```csharp
  public class MyDbContext : DbContext
  {
      protected override void OnModelCreating(ModelBuilder modelBuilder)
      {
          modelBuilder.Entity<Customer>()
              .Property(c => c.Name)
              .HasMaxLength(100);  // Local configuration for the 'Customer' entity.
      }
  }
  ```

---

**Global Configuration**

* **Global configuration** applies to the **entire application** and affects **all DbContexts** or entity models.
* This configuration is usually set up globally at the time of DbContext configuration (like through `DbContextOptions`), or by using global filters, conventions, or services.

  **Key Points of Global Configuration**:

  * Configurations apply **across all DbContexts** or entity classes in the application.
  * Can be set globally during **DbContext initialization** or via a configuration class.
  * It is often used for common settings that should apply universally to all models, such as database connection strings, caching strategies, or global query filters.

  **Example**:

  ```csharp
  public class MyDbContext : DbContext
  {
      public MyDbContext(DbContextOptions<MyDbContext> options) : base(options) { }

      protected override void OnModelCreating(ModelBuilder modelBuilder)
      {
          modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());  // Apply all global configurations
      }
  }
  ```

  **Global Query Filters** (can be applied globally):

  ```csharp
  modelBuilder.Entity<Customer>().HasQueryFilter(c => !c.IsDeleted);  // Apply filter to all queries for Customer
  ```

---

**Key Differences**

1. **Scope**

   * **Local Configuration**: Specific to a particular `DbContext` or entity class.
   * **Global Configuration**: Affects the entire application or all `DbContexts` and entities.

2. **Configuration Location**

   * **Local Configuration**: Defined inside `OnModelCreating` for a specific `DbContext`.
   * **Global Configuration**: Can be set globally in the `DbContextOptions` or through conventions, services, or query filters.

3. **Use Case**

   * **Local Configuration**: When specific entity or context-level settings are needed.
   * **Global Configuration**: When common settings should be applied across the entire application.

---

**Answer Summary**

* **Local configuration** applies to a **specific DbContext or entity class** and is defined inside `OnModelCreating` in the `DbContext`.
* **Global configuration** applies **across the entire application**, affecting all `DbContexts` and entities, and is typically set during DbContext initialization or using conventions and global filters.
* **Local configuration** is useful for customizing behavior on a per-entity basis, while **global configuration** is used for universal settings or behavior.

<br>

### 13. What is the purpose of the Entity Framework connection string?
**Purpose of the Entity Framework Connection String**

The **connection string** in **Entity Framework (EF)** is a crucial component that specifies how to connect to the database. It provides the necessary information to establish a connection between the application and the underlying database system.

---

**Key Points of the Connection String in EF**

1. **Database Connection Information**

   * The connection string contains all the details needed to establish a connection to a specific database, such as:

     * **Server address** (e.g., localhost, IP address, or hostname)
     * **Database name**
     * **Authentication credentials** (username and password)
     * **Connection parameters** (e.g., timeout, encryption, etc.)

   Example of a typical SQL Server connection string:

   ```xml
   <connectionStrings>
       <add name="MyDbContext" 
            connectionString="Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;" 
            providerName="System.Data.SqlClient" />
   </connectionStrings>
   ```

2. **Establishing Database Context**

   * In EF, the connection string is often used in the `DbContext` class to link the application to the database. It is passed to the `DbContextOptions` during initialization, allowing EF to manage database connections and transactions.

   Example in code:

   ```csharp
   public class MyDbContext : DbContext
   {
       public MyDbContext(DbContextOptions<MyDbContext> options) : base(options) { }

       protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
       {
           optionsBuilder.UseSqlServer("Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;");
       }
   }
   ```

3. **Database Provider**

   * The connection string also defines the **database provider** (e.g., SQL Server, MySQL, PostgreSQL), specifying the technology EF should use to connect to the database. This is critical as it ensures the correct driver is used for communication between the application and the database.

4. **Switching Between Different Environments**

   * The connection string enables easy switching between different environments (e.g., development, testing, production). By changing the connection string in different configuration files or environment variables, you can connect to different databases without modifying the application code.

   For example, a development connection string might point to a local database, while a production connection string points to a remote database server.

5. **Security Considerations**

   * The connection string often contains sensitive information, like database credentials. It's essential to protect this information by using techniques such as **encrypted configuration files** or storing credentials in **environment variables** or **secret management systems** to prevent unauthorized access.

---

**Answer Summary**

* The **Entity Framework connection string** is used to define the connection parameters between the application and the database. It includes **server address**, **database name**, and **authentication credentials**.
* It allows EF to **establish a connection** to the database, configure the **database provider**, and manage transactions.
* The connection string helps easily switch between **different environments** (development, production) and **protect sensitive information** through secure storage mechanisms.

<br>

### 14. How do you switch between different databases using EF?
**Switching Between Different Databases Using Entity Framework**

Entity Framework (EF) provides flexibility to switch between different databases based on various environments or configurations, such as development, testing, and production. The method for switching databases typically involves **using different connection strings** or configuring multiple `DbContext` options at runtime. Below are several strategies to achieve this:

---

**1. Using Multiple Connection Strings (Per Environment)**
The most common way to switch between databases is by defining **multiple connection strings** for different environments (e.g., development, production). You can store these connection strings in the `appsettings.json` or `web.config` file and then switch between them depending on the environment.

**Steps**:

* Define multiple connection strings in your **configuration file**.
* Use **dependency injection** to inject the appropriate connection string based on the environment.

**Example** (`appsettings.json`):

```json
{
  "ConnectionStrings": {
    "DevelopmentDb": "Server=localhost;Database=DevDb;User Id=devUser;Password=devPass;",
    "ProductionDb": "Server=prodServer;Database=ProdDb;User Id=prodUser;Password=prodPass;"
  }
}
```

In **Startup.cs**, set the correct connection string:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var connectionString = Configuration.GetConnectionString("DevelopmentDb"); // Or use "ProductionDb" based on the environment
    services.AddDbContext<MyDbContext>(options =>
        options.UseSqlServer(connectionString));
}
```

**Answer Summary**

* You can switch between different databases by defining **multiple connection strings** in your configuration files (like `appsettings.json`) and selecting the appropriate one during runtime using **dependency injection**.

---

**2. Using Environment Variables for Configuration**
You can configure your application to use **environment variables** to store the connection string. This is helpful for securing the connection string and making it flexible between different environments.

**Steps**:

* Store connection string in environment variables.
* Retrieve the connection string in your `DbContext` configuration.

**Example** (`appsettings.json`):

```json
{
  "ConnectionStrings": {
    "DbConnection": "Environment:DB_CONNECTION_STRING"
  }
}
```

In **Startup.cs**:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION_STRING");
    services.AddDbContext<MyDbContext>(options =>
        options.UseSqlServer(connectionString));
}
```

**Answer Summary**

* **Environment variables** can be used to securely manage and switch between databases by storing the connection string outside the application code.

---

**3. Dynamically Switching Database Connection at Runtime**
Another approach is to **dynamically change the connection string** at runtime, allowing the application to switch databases during execution.

**Steps**:

* Modify the connection string in the `OnConfiguring` method of the `DbContext`.
* You can switch the database connection conditionally based on specific application logic or user input.

**Example**:

```csharp
public class MyDbContext : DbContext
{
    private readonly string _connectionString;

    public MyDbContext(string connectionString)
    {
        _connectionString = connectionString;
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            optionsBuilder.UseSqlServer(_connectionString);
        }
    }
}
```

When creating the `DbContext` instance:

```csharp
var dbContext = new MyDbContext("YourDatabaseConnectionString");
```

**Answer Summary**

* You can **dynamically change the connection string** during runtime, allowing flexibility to switch databases as needed.

---

**4. Using EF Core's `DbContextOptions` for Database Switching**
EF Core allows passing the connection string through **`DbContextOptions`**. This can be done at runtime and allows the database to be switched as needed.

**Example**:

```csharp
public class MyDbContext : DbContext
{
    public MyDbContext(DbContextOptions<MyDbContext> options) : base(options) { }
}
```

During application startup:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var environment = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT");
    string connectionString = environment == "Production" 
                              ? "Server=prodServer;Database=ProdDb;User Id=prodUser;Password=prodPass;" 
                              : "Server=localhost;Database=DevDb;User Id=devUser;Password=devPass;";

    services.AddDbContext<MyDbContext>(options => options.UseSqlServer(connectionString));
}
```

**Answer Summary**

* **DbContextOptions** can be used to configure the connection string at runtime, allowing for easy switching between databases based on the application's environment.

---

**5. Using Conditional Database Context Configuration**
You can configure the `DbContext` to connect to different databases based on specific conditions or logic in your application.

**Example**:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    if (someCondition)
    {
        services.AddDbContext<MyDbContext>(options =>
            options.UseSqlServer("ConnectionStringForDatabase1"));
    }
    else
    {
        services.AddDbContext<MyDbContext>(options =>
            options.UseSqlServer("ConnectionStringForDatabase2"));
    }
}
```

**Answer Summary**

* **Conditional database context configuration** allows you to choose between multiple databases based on runtime conditions or specific logic.

---

**Overall Answer Summary**
To switch between different databases using Entity Framework, you can:

* Use multiple **connection strings** in the configuration files and choose based on the environment.
* Retrieve the **connection string from environment variables** for secure and flexible management.
* **Dynamically change the connection string** during runtime or via **`DbContextOptions`**.
* Use **conditional logic** to configure the database context based on specific application needs or logic.

These approaches allow you to easily manage and switch databases in different environments or situations.

<br>

### 15. Can you configure the pluralization and singularization conventions in EF?
**Configuring Pluralization and Singularization Conventions in Entity Framework**

Entity Framework (EF) automatically pluralizes or singularizes table names when mapping them to classes. By default, EF follows a set of naming conventions based on pluralizing class names to form table names. For example, a class named `Person` would be mapped to a table named `People`. However, there may be situations where you want to customize these conventions.

EF allows you to configure pluralization and singularization rules using the **`OnModelCreating`** method in your `DbContext`. You can either disable automatic pluralization and singularization or configure specific rules for how EF should handle naming conventions.

---

**Steps to Configure Pluralization and Singularization in EF**

### 1. **Disable Pluralization and Singularization**

By default, EF applies pluralization to table names. If you prefer to use **exact class names** for table names, you can turn off pluralization by overriding the `OnModelCreating` method.

**Example**:

```csharp
public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Disable pluralization (use class name as table name)
        modelBuilder.UseCollation("Latin1_General_CI_AS");  // Optional collation setting if needed
    }
}
```

### 2. **Custom Singularization and Pluralization Rules**

You can implement your own rules for singularization or pluralization using `modelBuilder`. For instance, you can manually set the table name in the `OnModelCreating` method.

**Example**:

```csharp
public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Manually set the table name for a class
        modelBuilder.Entity<Person>().ToTable("Person");  // No pluralization
    }
}
```

### 3. **Using Third-Party Libraries for Pluralization**

EF itself does not include a built-in pluralization or singularization service. However, you can use third-party libraries like **Humanizer** to manage pluralization and singularization rules.

**Example** using **Humanizer**:

```csharp
using Humanizer;

public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Use Humanizer to apply custom pluralization or singularization
        modelBuilder.Entity<Person>().ToTable("Person".ToPlural()); // Applies pluralization from Humanizer
    }
}
```

### 4. **Applying Global Naming Conventions**

To configure naming conventions globally, you can define how EF should handle table and column naming by using **conventions** in the `OnModelCreating` method.

**Example**:

```csharp
public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        // Example of applying a naming convention
        modelBuilder.Entity<Employee>().ToTable("Employees"); // Use plural table name explicitly
    }
}
```

---

**Answer Summary**

* Entity Framework uses **pluralization** by default for table names based on class names, but you can **disable or configure** this behavior.
* You can **manually configure** singular or plural table names for specific entities using `modelBuilder`.
* **Third-party libraries** like **Humanizer** can be used to apply custom pluralization or singularization rules.
* For global conventions, you can define consistent naming strategies across all entities within the `OnModelCreating` method.

This flexibility allows you to adhere to your preferred naming conventions for tables and entities in EF.

<br>

## 🗃️ Entity Framework Database Operations

### 16. How do you perform CRUD operations using EF?
**Performing CRUD Operations Using Entity Framework**

CRUD operations (Create, Read, Update, Delete) are essential for interacting with a database. In Entity Framework (EF), performing these operations is quite simple and involves the use of the `DbContext` class to interact with the database. Here's a breakdown of how to perform each operation:

---

### 1. **Create (Insert)**

To insert data into the database, you create an object of the entity class, set its properties, and then add it to the context using `DbSet.Add()`. After that, call `SaveChanges()` to persist the changes to the database.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var newPerson = new Person
    {
        Name = "John Doe",
        Age = 30
    };
    context.People.Add(newPerson);
    context.SaveChanges(); // Save changes to the database
}
```

**Answer Summary**:

* Use `DbSet.Add()` to add a new entity.
* Call `SaveChanges()` to persist the entity to the database.

---

### 2. **Read (Select)**

To read data from the database, you can use `DbSet.Find()`, `DbSet.FirstOrDefault()`, or LINQ queries. The `Find()` method is used to retrieve an entity by its primary key, while `FirstOrDefault()` can be used for more complex queries.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    // Retrieve a person by primary key
    var person = context.People.Find(1);
    
    // Retrieve the first person where Age is greater than 25
    var personByAge = context.People.FirstOrDefault(p => p.Age > 25);
}
```

**Answer Summary**:

* Use `DbSet.Find()` to find by primary key.
* Use `FirstOrDefault()` or LINQ to perform more complex queries.

---

### 3. **Update**

To update data, you first retrieve the entity from the database, modify its properties, and then call `SaveChanges()` to persist the changes.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.Find(1); // Retrieve the entity
    if (person != null)
    {
        person.Age = 31;  // Modify the property
        context.SaveChanges();  // Save the updated entity back to the database
    }
}
```

**Answer Summary**:

* Retrieve the entity using `Find()`.
* Modify the entity's properties.
* Call `SaveChanges()` to apply the updates to the database.

---

### 4. **Delete**

To delete an entity, retrieve it from the database using `Find()` or a query, then use `DbSet.Remove()` to mark it for deletion, followed by `SaveChanges()` to remove it from the database.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.Find(1); // Retrieve the entity
    if (person != null)
    {
        context.People.Remove(person); // Mark it for deletion
        context.SaveChanges();  // Save changes to remove it from the database
    }
}
```

**Answer Summary**:

* Retrieve the entity using `Find()`.
* Use `DbSet.Remove()` to mark it for deletion.
* Call `SaveChanges()` to apply the deletion to the database.

---

**Answer Summary**

* **Create**: Use `DbSet.Add()` to add new entities and `SaveChanges()` to persist them.
* **Read**: Use `DbSet.Find()` for primary key lookups or `FirstOrDefault()` for complex queries.
* **Update**: Retrieve the entity, modify its properties, and call `SaveChanges()`.
* **Delete**: Retrieve the entity, use `DbSet.Remove()` to mark it for deletion, and call `SaveChanges()`.

These CRUD operations enable basic database interaction with EF using `DbContext`.

<br>

### 17. What methods are used to read data in EF?
**Methods to Read Data in Entity Framework**

Entity Framework (EF) provides several methods to retrieve data from the database. These methods offer flexibility in querying the database using LINQ or built-in EF methods. Here's an overview of the most common methods used to read data:

---

### 1. **`DbSet.Find()`**

The `Find()` method is used to retrieve an entity by its primary key. It performs a lookup using the primary key and returns the entity if found; otherwise, it returns `null`.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.Find(1);  // Finds the person with primary key 1
}
```

**Answer Summary**:

* `Find()` retrieves an entity based on its primary key.
* It is the most efficient method for looking up a single entity.

---

### 2. **`DbSet.FirstOrDefault()`**

This method returns the first entity in the sequence or a default value if no entity matches the provided condition. It's useful for querying with LINQ conditions.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.FirstOrDefault(p => p.Age > 25);  // Returns the first person with Age > 25
}
```

**Answer Summary**:

* `FirstOrDefault()` returns the first entity matching the condition or `null` if no match is found.
* Useful for queries where you need to fetch the first record based on certain criteria.

---

### 3. **`DbSet.SingleOrDefault()`**

This method is similar to `FirstOrDefault()`, but it expects the query to return a single result. If the query returns more than one result, it throws an exception. It returns `null` if no entity is found.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.SingleOrDefault(p => p.Email == "john.doe@example.com");
}
```

**Answer Summary**:

* `SingleOrDefault()` ensures that only one entity matches the condition.
* Throws an exception if there are multiple results, making it suitable for queries where only one result is expected.

---

### 4. **`DbSet.ToList()`**

The `ToList()` method executes the query and returns the results as a list. It retrieves all entities matching the provided condition and loads them into memory.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var peopleOver25 = context.People.Where(p => p.Age > 25).ToList();  // Retrieves all people with Age > 25
}
```

**Answer Summary**:

* `ToList()` executes the query and returns the results as a list.
* Suitable for retrieving multiple entities and loading them into memory.

---

### 5. **`DbSet.Where()`**

This method is used to filter the data based on the specified condition. It can be combined with other methods like `ToList()`, `FirstOrDefault()`, etc., to retrieve a filtered set of data.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = context.People.Where(p => p.Age > 25);  // Returns an IQueryable of people over 25
}
```

**Answer Summary**:

* `Where()` filters the data based on the provided condition.
* It returns an `IQueryable`, which can be further processed.

---

### 6. **`DbSet.OrderBy()` / `DbSet.OrderByDescending()`**

These methods are used to order the results in ascending or descending order. They can be combined with other methods like `ToList()` or `FirstOrDefault()`.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var orderedPeople = context.People.OrderBy(p => p.Name).ToList();  // Orders people by name
}
```

**Answer Summary**:

* `OrderBy()` orders results in ascending order, and `OrderByDescending()` orders them in descending order.
* Combined with methods like `ToList()` or `FirstOrDefault()` for ordered data retrieval.

---

### 7. **`DbSet.Include()`**

This method is used for **eager loading** related entities. It is useful when you need to load related entities along with the primary entity.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var personWithOrders = context.People.Include(p => p.Orders).FirstOrDefault(p => p.Id == 1);  // Includes related orders
}
```

**Answer Summary**:

* `Include()` is used to eagerly load related entities along with the main entity.
* Useful for retrieving data with related objects, preventing lazy loading issues.

---

### 8. **`DbSet.Select()`**

This method projects data from one type to another (e.g., selecting specific columns). It is commonly used in LINQ queries to shape the data.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var names = context.People.Select(p => p.Name).ToList();  // Selects only the names of all people
}
```

**Answer Summary**:

* `Select()` projects specific data from entities.
* It is useful when you need a subset of data rather than the entire entity.

---

**Answer Summary**

* **`Find()`**: Retrieves an entity by its primary key.
* **`FirstOrDefault()`**: Returns the first matching entity or `null`.
* **`SingleOrDefault()`**: Returns a single matching entity or throws an exception if more than one is found.
* **`ToList()`**: Executes a query and returns the results as a list.
* **`Where()`**: Filters data based on the provided condition.
* **`OrderBy()` / `OrderByDescending()`**: Orders results in ascending or descending order.
* **`Include()`**: Eagerly loads related entities.
* **`Select()`**: Projects specific data from entities.

These methods give you flexibility in querying data in EF, allowing you to perform simple to complex queries efficiently.

<br>

### 18. Can you perform bulk insert operations in EF?
**Bulk Insert Operations in Entity Framework**

Entity Framework (EF) provides a straightforward approach for inserting data, but by default, it does not support bulk insert operations directly. However, EF allows you to insert multiple entities one by one, but this can be inefficient when working with large datasets. For handling bulk insert operations efficiently, there are several approaches you can use:

---

### 1. **Using `DbSet.AddRange()`**

The `AddRange()` method allows you to add multiple entities at once to the context. This method is useful for performing bulk insert operations, but keep in mind that it still inserts the records one by one during the `SaveChanges()` call.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 },
        new Person { Name = "Doe", Age = 35 }
    };

    context.People.AddRange(people);
    context.SaveChanges();  // Executes the insert operation
}
```

**Answer Summary**:

* `AddRange()` adds multiple entities to the context.
* When `SaveChanges()` is called, EF will insert them one by one.
* This method is efficient for moderate-sized datasets but not optimal for very large ones.

---

### 2. **Using Third-Party Libraries (e.g., `EFCore.BulkExtensions`)**

For better performance with large datasets, you can use third-party libraries such as `EFCore.BulkExtensions`. These libraries optimize bulk insert operations by reducing the number of database round-trips.

**Example with `EFCore.BulkExtensions`**:

```csharp
using (var context = new MyDbContext())
{
    var people = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 },
        new Person { Name = "Doe", Age = 35 }
    };

    context.BulkInsert(people);  // Bulk insert operation
}
```

**Answer Summary**:

* Libraries like `EFCore.BulkExtensions` offer optimized bulk insert operations.
* They significantly reduce the overhead compared to individual inserts by using batch processing.

---

### 3. **Using Raw SQL for Bulk Insert**

Another option for performing bulk insert operations is to use raw SQL commands. This method bypasses EF's change tracking and works directly with SQL commands to insert the data in bulk.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 },
        new Person { Name = "Doe", Age = 35 }
    };

    var query = "INSERT INTO People (Name, Age) VALUES (@Name, @Age)";
    context.Database.ExecuteSqlRaw(query, people);
}
```

**Answer Summary**:

* Raw SQL bypasses EF and directly inserts data into the database.
* This method offers high performance but lacks the abstraction and tracking provided by EF.

---

### 4. **Batching with `SaveChanges()`**

When inserting multiple entities, you can reduce the number of database round-trips by calling `SaveChanges()` in batches. This is a manual approach that inserts entities in smaller batches to avoid the overhead of one large insert operation.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 },
        new Person { Name = "Doe", Age = 35 },
        // More entities
    };

    const int batchSize = 100;  // Define the batch size
    for (int i = 0; i < people.Count; i += batchSize)
    {
        var batch = people.Skip(i).Take(batchSize).ToList();
        context.People.AddRange(batch);
        context.SaveChanges();  // Commit the batch to the database
    }
}
```

**Answer Summary**:

* Batch inserts using `SaveChanges()` in smaller chunks.
* It helps to manage memory and avoid timeouts with large datasets.

---

**Answer Summary**

* **`AddRange()`**: Adds multiple entities to the context and saves them one by one.
* **Third-Party Libraries**: Use libraries like `EFCore.BulkExtensions` for efficient bulk inserts with minimal overhead.
* **Raw SQL**: Directly execute bulk inserts with SQL for maximum performance.
* **Batching**: Break the insert operation into smaller batches using `SaveChanges()` to avoid performance issues with large datasets.

For handling large volumes of data efficiently, using third-party libraries or raw SQL for bulk operations provides the best performance compared to standard `AddRange()` methods.

<br>

### 19. How do you execute a stored procedure using EF?
**Executing Stored Procedures in Entity Framework**

Entity Framework (EF) allows you to execute stored procedures in several ways, depending on your requirements and EF version. The most common methods include executing stored procedures through raw SQL queries or using EF's built-in methods for executing procedures that return scalar values or collections.

---

### 1. **Using `Database.ExecuteSqlRaw()` or `Database.ExecuteSqlInterpolated()`**

If the stored procedure does not return any result set (i.e., it performs an operation such as an update or delete), you can use `ExecuteSqlRaw()` or `ExecuteSqlInterpolated()`. These methods execute raw SQL directly against the database.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var result = context.Database.ExecuteSqlRaw("EXEC dbo.UpdatePerson @p0, @p1", 1, "John Doe");
}
```

**Answer Summary**:

* `ExecuteSqlRaw()` is used to execute stored procedures that do not return data.
* It executes commands that affect the database like `INSERT`, `UPDATE`, or `DELETE`.
* Parameters are passed as arguments to prevent SQL injection.

---

### 2. **Using `FromSqlRaw()` or `FromSqlInterpolated()` for Result Sets**

If your stored procedure returns a result set (e.g., a list of entities), you can use `FromSqlRaw()` or `FromSqlInterpolated()`. These methods are used to execute stored procedures that return data and map the results to entity types.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = context.People
        .FromSqlRaw("EXEC dbo.GetPeopleByAge @p0", 30)
        .ToList();  // Retrieves a list of people aged 30 or above
}
```

**Answer Summary**:

* `FromSqlRaw()` is used to execute stored procedures that return data and map it to an entity.
* The result is returned as a collection (e.g., a list of entities).
* Use parameters to avoid SQL injection.

---

### 3. **Using `FromSqlInterpolated()` for Dynamic SQL**

`FromSqlInterpolated()` is similar to `FromSqlRaw()`, but it allows you to use interpolated strings to pass parameters dynamically. This is useful when you need to create queries dynamically.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var age = 30;
    var people = context.People
        .FromSqlInterpolated($"EXEC dbo.GetPeopleByAge {age}")
        .ToList();
}
```

**Answer Summary**:

* `FromSqlInterpolated()` allows dynamic SQL execution with parameters using interpolated strings.
* It automatically protects against SQL injection when using parameters.

---

### 4. **Using `ExecuteSqlCommand()` (EF 7 and earlier)**

In EF Core 7 and earlier, you can use the `ExecuteSqlCommand()` method to execute a stored procedure that does not return any data.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    context.Database.ExecuteSqlCommand("EXEC dbo.DeleteInactiveUsers");
}
```

**Answer Summary**:

* `ExecuteSqlCommand()` is used for stored procedures that do not return data.
* It was used in EF 7 and earlier versions but is replaced by `ExecuteSqlRaw()` in later versions.

---

### 5. **Using `SqlQuery()` (EF 6 and Earlier)**

In EF 6 and earlier, you can use `SqlQuery()` to execute stored procedures that return a collection of results.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = context.Database.SqlQuery<Person>("EXEC dbo.GetPeopleByAge @p0", 30).ToList();
}
```

**Answer Summary**:

* `SqlQuery()` is used to execute stored procedures and map the results to a collection of entities.
* It is available in EF 6 and earlier versions.

---

### 6. **Using Asynchronous Methods**

You can also execute stored procedures asynchronously in EF using methods like `ExecuteSqlRawAsync()` for operations without a result set, and `FromSqlRawAsync()` for retrieving data.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var people = await context.People
        .FromSqlRaw("EXEC dbo.GetPeopleByAge @p0", 30)
        .ToListAsync();
}
```

**Answer Summary**:

* Use asynchronous methods like `ExecuteSqlRawAsync()` and `FromSqlRawAsync()` for non-blocking execution of stored procedures.
* This is especially useful in web applications to avoid blocking the thread.

---

**Answer Summary**

* **`ExecuteSqlRaw()` / `ExecuteSqlInterpolated()`**: Execute stored procedures without a result set (e.g., `INSERT`, `UPDATE`, `DELETE`).
* **`FromSqlRaw()` / `FromSqlInterpolated()`**: Execute stored procedures that return a result set and map it to entities.
* **`ExecuteSqlCommand()`**: For EF 7 and earlier to execute stored procedures without results (replaced by `ExecuteSqlRaw()` in newer versions).
* **`SqlQuery()`**: Available in EF 6 and earlier to execute stored procedures returning results.
* **Asynchronous Methods**: Use `ExecuteSqlRawAsync()` and `FromSqlRawAsync()` for asynchronous execution.

By using these methods, you can execute stored procedures in EF and map the results back to your entities efficiently.

<br>

### 20. Explain how transactions are handled in EF.
**Transactions in Entity Framework**

In Entity Framework (EF), transactions ensure that a series of database operations either complete successfully as a unit or fail together, maintaining data integrity. EF handles transactions at both the **context level** (with automatic transactions) and the **database level** (using explicit transaction management).

---

### 1. **Implicit Transactions (Default Behavior)**

By default, EF wraps `SaveChanges()` in an implicit transaction. This means that when `SaveChanges()` is called, EF automatically starts a transaction, performs the operations (such as `INSERT`, `UPDATE`, or `DELETE`), and commits the transaction if no errors occur. If an error is thrown, EF automatically rolls back the transaction.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = new Person { Name = "John", Age = 30 };
    context.People.Add(person);
    context.SaveChanges();  // EF will handle the transaction automatically
}
```

**Answer Summary**:

* **Implicit transactions**: EF automatically starts, commits, and rolls back transactions when you call `SaveChanges()`.
* It ensures that the changes are made atomically, providing automatic rollback if an error occurs.

---

### 2. **Explicit Transactions with `IDbContextTransaction`**

For scenarios where you need more control, such as executing multiple operations as a single transaction or managing complex transactions, you can manually control transactions using the `IDbContextTransaction` interface.

You can start, commit, and roll back transactions explicitly. This is useful when you need to perform multiple `SaveChanges()` calls and ensure that they all succeed or fail together.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    using (var transaction = context.Database.BeginTransaction())
    {
        try
        {
            var person1 = new Person { Name = "John", Age = 30 };
            var person2 = new Person { Name = "Jane", Age = 25 };

            context.People.Add(person1);
            context.People.Add(person2);
            context.SaveChanges();  // First commit to the database

            // Perform more operations
            var person3 = new Person { Name = "Doe", Age = 35 };
            context.People.Add(person3);
            context.SaveChanges();  // Second commit to the database

            transaction.Commit();  // Commit the transaction if all operations are successful
        }
        catch
        {
            transaction.Rollback();  // Rollback if any operation fails
        }
    }
}
```

**Answer Summary**:

* **Explicit transactions**: Use `BeginTransaction()` to manually manage the transaction with `Commit()` and `Rollback()`.
* Useful for handling multiple database operations that should be treated as a single unit.

---

### 3. **Transactions with Asynchronous Operations**

EF supports asynchronous operations for database interactions, including transactions. The asynchronous methods work the same way as synchronous ones but help avoid blocking the main thread, especially in web applications.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    using (var transaction = await context.Database.BeginTransactionAsync())
    {
        try
        {
            var person = new Person { Name = "John", Age = 30 };
            context.People.Add(person);
            await context.SaveChangesAsync();  // First commit to the database

            // More operations
            var person2 = new Person { Name = "Jane", Age = 25 };
            context.People.Add(person2);
            await context.SaveChangesAsync();  // Second commit

            await transaction.CommitAsync();  // Commit the transaction
        }
        catch
        {
            await transaction.RollbackAsync();  // Rollback on failure
        }
    }
}
```

**Answer Summary**:

* **Asynchronous transactions**: Use `BeginTransactionAsync()` and `CommitAsync()` for non-blocking database operations.
* Ensures performance in web applications by freeing up threads during long-running database operations.

---

### 4. **Transactions in a Unit of Work Pattern**

In some scenarios, especially in complex systems, you may follow the **Unit of Work** design pattern to manage transactions. The Unit of Work pattern consolidates multiple operations within a single transactional scope, allowing you to handle database changes as a single logical unit.

**Example**:

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly MyDbContext _context;

    public UnitOfWork(MyDbContext context)
    {
        _context = context;
    }

    public async Task CommitAsync()
    {
        await _context.SaveChangesAsync();
    }

    public void BeginTransaction()
    {
        _context.Database.BeginTransaction();
    }

    public void CommitTransaction()
    {
        _context.Database.CurrentTransaction.Commit();
    }

    public void RollbackTransaction()
    {
        _context.Database.CurrentTransaction.Rollback();
    }
}
```

**Answer Summary**:

* **Unit of Work**: Provides an abstraction layer for managing transactions, allowing for better separation of concerns in large applications.

---

### 5. **Handling Nested Transactions**

Entity Framework does not support nested transactions directly. However, you can simulate nested transactions by using **Savepoints** in raw SQL. Savepoints allow you to create logical points within a transaction where you can roll back to without affecting the entire transaction.

**Example** (using raw SQL savepoint):

```csharp
using (var context = new MyDbContext())
{
    using (var transaction = context.Database.BeginTransaction())
    {
        try
        {
            context.Database.ExecuteSqlRaw("SAVE TRANSACTION SavePoint1");

            // First operation
            context.SaveChanges();

            context.Database.ExecuteSqlRaw("ROLLBACK TRANSACTION SavePoint1");

            // Second operation
            context.SaveChanges();
            transaction.Commit();
        }
        catch
        {
            transaction.Rollback();
        }
    }
}
```

**Answer Summary**:

* **Nested transactions**: Simulated using SQL savepoints to allow rollback to specific points in a transaction.
* EF does not natively support nested transactions, but savepoints can help achieve this.

---

**Answer Summary**

* **Implicit Transactions**: Automatically handled by EF when you call `SaveChanges()`. Rolls back on failure.
* **Explicit Transactions**: Managed using `IDbContextTransaction` for manual commit and rollback of multiple operations.
* **Asynchronous Transactions**: Use `BeginTransactionAsync()` and `CommitAsync()` for async operations to improve performance.
* **Unit of Work**: A design pattern that encapsulates transaction handling across multiple operations for better control and organization.
* **Nested Transactions**: Simulated using raw SQL savepoints, as EF does not support them natively.

EF provides flexibility in transaction management, allowing for both automatic and manual control depending on the complexity of the operations and the application's requirements.

<br>

## 🧩 Entity Framework Modelling

### 21. What are Entities and how do you define them in EF?
**Entities in Entity Framework**

In Entity Framework (EF), **entities** represent the data models that map to the tables in the database. They are essentially classes in your application that correspond to database tables, where each property of the class represents a column in the table. EF allows you to interact with the database using these entities, making CRUD (Create, Read, Update, Delete) operations easier and more intuitive.

---

### 1. **Defining Entities in EF**

To define an entity in EF, you create a class that represents a table in your database. The class typically has properties that match the columns in the table. You can also use **data annotations** or **fluent API** to configure relationships, constraints, and other mappings between the class and the database table.

**Example of a simple entity**:

```csharp
public class Person
{
    public int Id { get; set; }  // Represents the Primary Key column
    public string Name { get; set; }
    public int Age { get; set; }
}
```

**Answer Summary**:

* **Entities**: Classes that represent database tables.
* **Properties**: Represent columns in the table.
* Each entity corresponds to a table in the database, where each instance of the class corresponds to a row in the table.

---

### 2. **Entity Configuration using Data Annotations**

You can use **data annotations** to configure the entity properties. Data annotations are attributes that you apply to the properties or the class itself to define how the class should be mapped to the database.

**Common data annotations**:

* `[Key]`: Marks the primary key.
* `[Required]`: Ensures a property is not null.
* `[StringLength]`: Limits the length of a string property.
* `[Column]`: Maps a property to a specific column name.
* `[ForeignKey]`: Specifies a foreign key relationship.

**Example**:

```csharp
public class Person
{
    [Key]
    public int Id { get; set; }

    [Required]
    [StringLength(100)]
    public string Name { get; set; }

    public int Age { get; set; }
}
```

**Answer Summary**:

* **Data Annotations**: Attributes used to configure entity properties, such as `Key`, `Required`, `StringLength`.
* They provide an easy way to map properties to database constraints without the need for additional configuration.

---

### 3. **Entity Configuration using Fluent API**

If you prefer more flexibility or need to configure more complex mappings, you can use **Fluent API**. Fluent API allows for detailed and complex configurations, such as defining relationships, constraints, and mappings, in the `OnModelCreating` method inside the `DbContext`.

**Example using Fluent API**:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasKey(p => p.Id);  // Defines the primary key

        modelBuilder.Entity<Person>()
            .Property(p => p.Name)
            .IsRequired()  // Marks the Name property as required
            .HasMaxLength(100);  // Limits the length of Name to 100 characters
    }
}
```

**Answer Summary**:

* **Fluent API**: Used for more complex or advanced configuration, especially for relationships and constraints.
* **OnModelCreating**: Configuration is done in this method for each entity.

---

### 4. **Navigational Properties in Entities**

Entities can also include **navigation properties**, which are used to represent relationships between entities, such as one-to-many or many-to-many relationships. These properties are not directly mapped to columns in the database but represent related data.

**Example of a one-to-many relationship**:

```csharp
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Order> Orders { get; set; }  // Navigation property
}

public class Order
{
    public int Id { get; set; }
    public int PersonId { get; set; }
    public Person Person { get; set; }  // Navigation property to Person
}
```

**Answer Summary**:

* **Navigation Properties**: Represent relationships between entities (e.g., one-to-many, many-to-many).
* They do not map to columns but represent related data that can be loaded alongside the entity.

---

### 5. **Entity Tracking**

EF tracks entities that are loaded into the `DbContext`. This means EF knows when changes have been made to an entity and can automatically update the database when `SaveChanges()` is called. EF uses different states to track entities:

* **Added**: New entity, to be inserted.
* **Modified**: Entity that has been changed, to be updated.
* **Deleted**: Entity marked for deletion.
* **Unchanged**: Entity that has not been modified.

**Example**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.First();
    person.Name = "John Updated";  // EF tracks this change
    context.SaveChanges();  // EF updates the database
}
```

**Answer Summary**:

* **Entity Tracking**: EF tracks changes to entities and automatically applies changes to the database during `SaveChanges()`.
* Entities have different states, such as `Added`, `Modified`, `Deleted`, and `Unchanged`.

---

### 6. **Primary and Foreign Keys**

EF uses **primary keys** to uniquely identify each entity in the database. You can specify a property as the primary key using data annotations or Fluent API. **Foreign keys** define the relationships between entities (e.g., one-to-many, many-to-many).

**Example with foreign key**:

```csharp
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int PersonId { get; set; }  // Foreign key
    public Person Person { get; set; }  // Navigation property
}
```

**Answer Summary**:

* **Primary Keys**: Uniquely identify each entity in the database.
* **Foreign Keys**: Represent relationships between entities (e.g., one-to-many).

---

**Answer Summary**

* **Entities**: Classes that represent database tables in EF, where class properties correspond to table columns.
* **Data Annotations**: Use attributes to configure entity properties (e.g., `Key`, `Required`).
* **Fluent API**: Offers more advanced configuration options, especially for relationships and complex mappings.
* **Navigation Properties**: Represent relationships between entities (e.g., one-to-many).
* **Entity Tracking**: EF tracks entity changes (e.g., Added, Modified) and applies them during `SaveChanges()`.
* **Primary/Foreign Keys**: Used for unique identification and defining relationships between entities.

<br>

### 22. How does EF handle complex types?
In Entity Framework (EF), **complex types** refer to objects that are not directly mapped to a table in the database. Instead, complex types are used to represent properties of an entity that can be grouped together as a cohesive unit, but do not have a corresponding primary key. These types can be used to encapsulate multiple values into a single property of an entity. Complex types are commonly used for value objects, such as addresses or contact information, which do not require a separate table.

EF supports **complex types** through both **Code-First** and **Model-First** approaches.

---

### 1. **Defining Complex Types**

To define a complex type in EF, you create a class that represents the group of related properties. Complex types cannot be tracked by EF independently—they are always part of an entity and depend on the entity they are part of. EF will store these properties as columns in the table that corresponds to the containing entity.

**Example of a Complex Type**:

```csharp
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string PostalCode { get; set; }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // Complex type as a property
    public Address Address { get; set; }
}
```

In this example, the `Address` class is a complex type that is used as a property in the `Person` entity.

---

### 2. **Configuring Complex Types with Fluent API**

To tell EF that a class should be treated as a complex type, you must configure it using the **Fluent API** inside the `OnModelCreating` method in your `DbContext` class. You do this using the `OwnsOne` or `OwnsMany` methods, which indicate that the type is a complex type owned by the entity.

**Example using Fluent API**:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .OwnsOne(p => p.Address);  // Configure Address as a complex type owned by Person
    }
}
```

**Answer Summary**:

* **Fluent API**: Use `OwnsOne` to configure complex types as owned by an entity, indicating that they are not independent entities.

---

### 3. **Storing Complex Types in the Database**

When a complex type is added as a property of an entity, EF maps its properties to separate columns in the same table as the parent entity. The columns are typically named as a combination of the parent entity’s property name and the complex type’s property names.

For the example above, EF will store the `Address` fields (i.e., `Street`, `City`, `State`, and `PostalCode`) as columns in the `Person` table, typically with names like `Address_Street`, `Address_City`, etc.

---

### 4. **Querying Complex Types**

EF allows you to query entities with complex types in a way that is transparent to the developer. When querying an entity, EF will automatically handle the complex type as part of the result.

**Example of querying a Person with Address**:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People
                        .Where(p => p.Name == "John")
                        .FirstOrDefault();
    Console.WriteLine(person.Address.City);  // Access complex type property
}
```

EF will automatically retrieve the `Address` properties as part of the `Person` entity and allow you to access them directly.

---

### 5. **Limitations of Complex Types**

* **No Key**: Complex types do not have primary keys. They are not tracked independently, and their lifecycle is tied to the lifecycle of the parent entity.
* **No Separate Table**: Complex types are stored as columns in the same table as the parent entity and do not have their own table in the database.
* **Not Independent**: You cannot query or manipulate complex types independently in EF. They always exist as part of the entity.

---

### 6. **Complex Types with Collections**

If you have a property that is a collection of complex types, you can use the `OwnsMany` method in Fluent API to configure it. This represents a one-to-many relationship between the parent entity and the collection of complex types.

**Example of a Complex Collection**:

```csharp
public class Order
{
    public int Id { get; set; }
    public string OrderNumber { get; set; }

    public ICollection<LineItem> LineItems { get; set; }
}

public class LineItem
{
    public string Product { get; set; }
    public int Quantity { get; set; }
}

public class MyDbContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Order>()
            .OwnsMany(o => o.LineItems);  // Configure LineItems as a collection of complex types
    }
}
```

In this case, `LineItems` is a collection of complex types, and EF will store the properties of `LineItem` as columns in the `Order` table, with column names like `LineItems_Product`, `LineItems_Quantity`, etc.

---

### 7. **Difference Between Complex Types and Entities**

* **Complex Types**:

  * Do not have a primary key.
  * Cannot be queried or saved independently.
  * Stored as columns in the same table as the parent entity.
* **Entities**:

  * Have a primary key.
  * Can be queried, updated, and saved independently.
  * Represent a full table in the database.

---

### Summary:

* **Complex Types** in EF are classes that represent groups of properties but do not have a separate table or primary key in the database.
* They are defined as part of an entity and are stored as columns in the entity's table.
* **Fluent API** (`OwnsOne` and `OwnsMany`) is used to configure them.
* They can be used for value objects that logically group properties together but are not independent entities.
* EF automatically handles querying, updating, and saving complex types as part of the parent entity.

<br>

### 23. What are navigation properties in EF?
In **Entity Framework (EF)**, **navigation properties** are properties in an entity class that represent relationships between that entity and other entities. They allow you to navigate from one entity to another within your object model, enabling you to work with related data easily.

### 1. **Types of Navigation Properties**

There are primarily two types of navigation properties in EF:

* **Reference Navigation Properties**: These represent a one-to-one or many-to-one relationship. A reference navigation property points to a single related entity.
* **Collection Navigation Properties**: These represent a one-to-many or many-to-many relationship. A collection navigation property is a collection (usually a `ICollection<T>`, `IEnumerable<T>`, or `List<T>`) of related entities.

### 2. **Example of Navigation Properties**

Consider the following example with two entities: `Person` and `Order`. A person can have multiple orders (one-to-many relationship).

```csharp
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // Collection Navigation Property (one-to-many relationship)
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public string OrderNumber { get; set; }
    
    // Reference Navigation Property (many-to-one relationship)
    public int PersonId { get; set; }
    public Person Person { get; set; }  // Navigation to the related Person
}
```

In this example:

* The `Orders` property in the `Person` class is a **collection navigation property** that holds multiple `Order` entities related to the `Person`.
* The `Person` property in the `Order` class is a **reference navigation property**, which holds a reference to a single `Person` entity, representing the many-to-one relationship.

### 3. **How Navigation Properties Work**

EF uses navigation properties to manage relationships between entities. EF uses these properties to automatically load related data (via **lazy loading**, **eager loading**, or **explicit loading**) when you query the entities.

**Lazy Loading**: EF will automatically load related entities when you access the navigation property.

* **Eager Loading**: You can specify related entities to be loaded immediately along with the main entity by using `Include()` in the query.
* **Explicit Loading**: You can manually load related entities after the initial query using methods like `Load()`.

#### Example of Lazy Loading:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.First();
    Console.WriteLine(person.Orders.Count);  // Orders are automatically loaded when accessed (Lazy Loading)
}
```

#### Example of Eager Loading:

```csharp
using (var context = new MyDbContext())
{
    var personWithOrders = context.People
                                   .Include(p => p.Orders)  // Eagerly load Orders
                                   .First();
    Console.WriteLine(personWithOrders.Orders.Count);
}
```

#### Example of Explicit Loading:

```csharp
using (var context = new MyDbContext())
{
    var person = context.People.First();
    context.Entry(person).Collection(p => p.Orders).Load();  // Explicitly load Orders
    Console.WriteLine(person.Orders.Count);
}
```

### 4. **Configuring Navigation Properties Using Fluent API**

You can configure navigation properties using the **Fluent API** in the `OnModelCreating` method in `DbContext`. This configuration includes defining relationships between entities, such as one-to-many, many-to-many, and one-to-one.

#### One-to-Many Relationship:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Order>()
            .HasOne(o => o.Person)  // Order has one Person
            .WithMany(p => p.Orders)  // Person has many Orders
            .HasForeignKey(o => o.PersonId);  // Foreign key on Order
    }
}
```

#### One-to-One Relationship:

```csharp
modelBuilder.Entity<Order>()
    .HasOne(o => o.Person)  // One Order has one Person
    .WithOne(p => p.Order)   // One Person has one Order
    .HasForeignKey<Order>(o => o.PersonId);
```

#### Many-to-Many Relationship (EF 5 and later):

```csharp
modelBuilder.Entity<Order>()
    .HasMany(o => o.Products)  // An Order can have many Products
    .WithMany(p => p.Orders)   // A Product can have many Orders
    .UsingEntity<OrderProduct>();  // Link table to manage the many-to-many relationship
```

### 5. **Why Are Navigation Properties Important?**

* **Easier Data Manipulation**: Navigation properties simplify the process of working with related data. You can access related entities directly, reducing the amount of SQL code and manual joins.
* **Automatic Relationship Management**: EF automatically manages relationships and updates related entities when changes are made.
* **Optimized Queries**: EF uses navigation properties to generate optimized SQL queries, ensuring that only the necessary data is fetched from the database.

### 6. **Summary of Navigation Properties**:

* **Reference Navigation Property**: A property that points to a single related entity, typically used in one-to-one or many-to-one relationships.
* **Collection Navigation Property**: A property that points to a collection of related entities, used in one-to-many or many-to-many relationships.
* **Relationship Management**: EF uses navigation properties to manage relationships between entities and load related data efficiently (via lazy, eager, or explicit loading).
* **Fluent API Configuration**: Navigation properties can be configured using Fluent API to define complex relationships and behaviors.

In short, **navigation properties** in EF allow you to traverse relationships between entities easily, providing a high level of abstraction that makes working with related data more intuitive and efficient.

<br>

### 24. Can you explain Entity Framework’s Fluent API?
In Entity Framework (EF), **Fluent API** is a way to configure the model of your application using code, without relying on attributes in your entity classes. It provides a more flexible and powerful approach for configuring the behavior of the model and its mappings to the database schema. Fluent API is accessed through the `OnModelCreating` method in your `DbContext` class, and it allows you to configure things like relationships, constraints, table names, indexes, etc.

### 1. **What is Fluent API?**

The Fluent API in Entity Framework is a set of methods and properties provided by the `ModelBuilder` class that allows you to configure your entity classes and relationships between entities in a more declarative and customizable manner. It is called "fluent" because of the method chaining pattern, where you can call methods in a continuous sequence.

Fluent API configurations are applied in the `OnModelCreating` method of the `DbContext` class.

### 2. **Why Use Fluent API?**

While Data Annotations provide a simple way to configure your entities, Fluent API is more powerful and flexible. It allows you to:

* Define more complex relationships.
* Set up more advanced configurations that cannot be achieved with data annotations (e.g., composite keys, many-to-many relationships).
* Customize table and column names, schema, and indexes.
* Configure behavior for many-to-many, one-to-many, and one-to-one relationships.

### 3. **Basic Configuration Using Fluent API**

The basic syntax for Fluent API configuration is to use the `ModelBuilder` object inside the `OnModelCreating` method in your `DbContext` class.

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Define entity configurations here
    }
}
```

### 4. **Common Fluent API Configurations**

#### a. **Setting Table Names and Schemas**

You can customize the table names and schema using the `ToTable` method.

```csharp
modelBuilder.Entity<Person>()
    .ToTable("Persons", "HumanResources");  // Sets the table name to "Persons" in "HumanResources" schema
```

#### b. **Setting Column Names and Data Types**

You can configure column names and data types using the `Property` method.

```csharp
modelBuilder.Entity<Person>()
    .Property(p => p.Name)
    .HasColumnName("FullName")
    .HasMaxLength(200)
    .IsRequired();
```

This configures the `Name` property to be mapped to a column named `FullName`, with a maximum length of 200 and marked as required.

#### c. **Configuring Primary Key**

You can set the primary key of an entity using the `HasKey` method.

```csharp
modelBuilder.Entity<Person>()
    .HasKey(p => p.Id);  // Configures the primary key of the Person entity to be "Id"
```

#### d. **Configuring Relationships**

Fluent API allows you to define relationships between entities. These relationships can be one-to-one, one-to-many, or many-to-many.

* **One-to-Many Relationship**:

```csharp
modelBuilder.Entity<Order>()
    .HasOne(o => o.Person)  // One Order has one Person
    .WithMany(p => p.Orders)  // One Person has many Orders
    .HasForeignKey(o => o.PersonId);  // Foreign key on Order
```

* **One-to-One Relationship**:

```csharp
modelBuilder.Entity<Order>()
    .HasOne(o => o.Customer)  // One Order has one Customer
    .WithOne(c => c.Order)    // One Customer has one Order
    .HasForeignKey<Order>(o => o.CustomerId);
```

* **Many-to-Many Relationship** (EF Core 5 and above):

```csharp
modelBuilder.Entity<Order>()
    .HasMany(o => o.Products)  // One Order has many Products
    .WithMany(p => p.Orders)   // One Product has many Orders
    .UsingEntity<OrderProduct>();  // Link table for many-to-many
```

#### e. **Defining Constraints**

You can define constraints like unique indexes using Fluent API.

```csharp
modelBuilder.Entity<Person>()
    .HasIndex(p => p.Email)
    .IsUnique();  // Ensures that the Email column is unique
```

#### f. **Composite Keys**

Fluent API can be used to define composite keys, where multiple properties form the primary key.

```csharp
modelBuilder.Entity<Order>()
    .HasKey(o => new { o.OrderId, o.OrderDate });  // Composite key using OrderId and OrderDate
```

### 5. **Method Chaining in Fluent API**

One of the strengths of Fluent API is method chaining. You can chain multiple configuration methods together for more concise code.

```csharp
modelBuilder.Entity<Person>()
    .ToTable("Persons")
    .HasKey(p => p.Id)
    .Property(p => p.Name)
    .HasMaxLength(200)
    .IsRequired();
```

### 6. **Advanced Fluent API Configurations**

#### a. **Configure Value Conversions**

You can use Fluent API to convert property values when reading from or writing to the database.

```csharp
modelBuilder.Entity<Person>()
    .Property(p => p.Status)
    .HasConversion<string>();  // Converts the Status property to a string in the database
```

#### b. **Configuring Defaults**

You can configure default values for properties.

```csharp
modelBuilder.Entity<Person>()
    .Property(p => p.CreatedDate)
    .HasDefaultValueSql("GETDATE()");  // Sets the default value of CreatedDate to the current date/time
```

#### c. **Indexing and Optimizing Queries**

You can define indexes for optimizing queries on frequently used columns.

```csharp
modelBuilder.Entity<Order>()
    .HasIndex(o => o.OrderNumber)  // Creates an index on OrderNumber column
    .IsUnique();  // Enforces uniqueness on the OrderNumber column
```

---

### Summary:

* **Fluent API** is a configuration method for Entity Framework that allows you to set up entity mappings, relationships, constraints, and more without using data annotations.
* It is configured inside the `OnModelCreating` method in `DbContext`.
* **Advantages**: Fluent API offers greater flexibility and supports configurations that data annotations cannot handle, such as composite keys, many-to-many relationships, and table mappings.
* Common configurations include defining table names, column names, setting primary/foreign keys, and relationships (one-to-one, one-to-many, many-to-many).
* **Method Chaining** in Fluent API allows concise and readable code, enabling multiple configurations to be defined in one statement.

<br>

### 25. What are Entity keys and how are they defined and used in EF?
### **Entity Keys in Entity Framework (EF)**

In **Entity Framework (EF)**, an **entity key** is a property or a set of properties that uniquely identify an entity in the database. Keys are fundamental in EF for managing relationships and ensuring that each entity instance is uniquely identifiable.

### **1. What is an Entity Key?**

An **entity key** is a property (or a group of properties) of an entity that uniquely identifies an instance of that entity. It ensures the entity can be distinguished from other instances in the database.

#### **Types of Entity Keys:**

1. **Primary Key**: The key that uniquely identifies each record in a database table.
2. **Composite Key**: A key made up of multiple properties (columns) that together uniquely identify an entity.
3. **Foreign Key**: A key that represents a relationship between two entities, linking a child entity to a parent entity.

### **2. Defining Entity Keys in EF**

Entity Framework automatically identifies the key by convention, but you can also configure it explicitly using **data annotations** or the **Fluent API**.

#### **a. Convention-Based (Automatic)**

By convention, EF assumes that the property named `Id` (or `<EntityName>Id`) in an entity class is the primary key. For example:

```csharp
public class Person
{
    public int Id { get; set; }  // Automatically recognized as the primary key
    public string Name { get; set; }
}
```

In this case, the `Id` property will be treated as the **primary key**.

#### **b. Data Annotations-Based (Explicit Configuration)**

You can use the `[Key]` attribute to explicitly mark a property as the primary key:

```csharp
using System.ComponentModel.DataAnnotations;

public class Person
{
    [Key]
    public int PersonId { get; set; }  // Explicitly marking PersonId as the primary key
    public string Name { get; set; }
}
```

#### **c. Fluent API-Based (Explicit Configuration)**

You can use Fluent API in the `OnModelCreating` method of the `DbContext` class to configure keys.

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasKey(p => p.PersonId);  // Explicitly configuring PersonId as the primary key
    }
}
```

### **3. Composite Keys**

If an entity requires multiple properties to uniquely identify it, you can configure a **composite key**. This is done by using Fluent API to specify a set of properties that together form the primary key.

```csharp
public class Order
{
    public int OrderId { get; set; }
    public int ProductId { get; set; }
    public DateTime OrderDate { get; set; }
}

public class MyDbContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Order>()
            .HasKey(o => new { o.OrderId, o.ProductId });  // Composite key using OrderId and ProductId
    }
}
```

In this example, the combination of `OrderId` and `ProductId` will serve as the composite key.

### **4. Using Entity Keys in EF**

Entity keys are used to uniquely identify, track, and retrieve entities from the database. EF uses the primary key for several purposes:

* **Tracking Changes**: EF uses the primary key to track the state of entities in memory (added, modified, deleted).
* **Relationships**: Foreign keys are used to represent relationships between entities, and they are linked using primary keys.
* **Querying**: EF uses the primary key to efficiently query and retrieve entities.

#### **Example of Using Keys for CRUD Operations**

Here’s an example of how Entity Framework uses keys to manage entity data:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Person> People { get; set; }
}

public class Program
{
    public static void Main(string[] args)
    {
        using (var context = new MyDbContext())
        {
            // Create a new Person
            var person = new Person { PersonId = 1, Name = "John Doe" };
            context.People.Add(person);
            context.SaveChanges();  // PersonId is the key, used to insert the record

            // Retrieve a Person by Primary Key (PersonId)
            var retrievedPerson = context.People.Find(1);  // Uses PersonId as the key

            // Update the Person
            retrievedPerson.Name = "John Smith";
            context.SaveChanges();  // EF tracks changes and updates the correct entity

            // Delete the Person
            context.People.Remove(retrievedPerson);  // Finds the entity by key
            context.SaveChanges();
        }
    }
}
```

### **5. Entity Keys in Relationships**

In relationships, **foreign keys** link related entities. For example, in a one-to-many relationship between `Person` and `Order`, the `Order` entity would have a foreign key to the `Person` entity:

```csharp
public class Order
{
    public int OrderId { get; set; }
    public int PersonId { get; set; }  // Foreign Key referencing Person
    public Person Person { get; set; }  // Navigation property
}
```

Here, `PersonId` is the **foreign key** that references the `Person` entity’s **primary key** (`PersonId`).

### **6. Key Points to Remember**

* **Primary Key**: Uniquely identifies an entity in the database (e.g., `Id`, `PersonId`).
* **Composite Key**: A combination of multiple properties that uniquely identify an entity.
* **Foreign Key**: A key that establishes a relationship between two entities, linking a child entity to a parent entity.
* **Fluent API** and **Data Annotations** can be used to define keys, but Fluent API provides more flexibility and advanced configurations (e.g., composite keys, relationships).
* **Entity Framework’s Tracking**: EF uses primary keys for tracking changes and ensuring that data is correctly updated or deleted.

### **Summary**

* **Entity keys** are used to uniquely identify an entity in the database.
* **Primary keys** are typically used to ensure each record is unique, while **composite keys** and **foreign keys** are used in complex relationships and references.
* You can define keys using **data annotations** or **Fluent API**.
* Keys play an essential role in EF's tracking, querying, and relationship management processes.

<br>

## 🔗 Entity Framework Relationships and Associations

### 26. How do you map associations in EF?

<br>

### 27. Can you explain the concept of table per hierarchy and table per type inheritance in EF?

<br>

### 28. How do you handle one-to-one, one-to-many, and many-to-many relationships in EF?

<br>

### 29. What is cascade delete and how is it configured in EF?

<br>

### 30. How do you create a self-referencing association in EF?

<br>

## 🔍 Entity Framework Queries and LINQ

### 31. How do you perform LINQ queries in EF?

<br>

### 32. Explain the difference between eager loading, lazy loading, and explicit loading.

<br>

### 33. How does EF translate LINQ-to-Entities queries into SQL queries?

<br>

### 34. Can you use LINQ to project results into a non-entity type?

<br>

### 35. What is LINQ to Entities?

<br>

## 🧬 Entity Framework Migrations and Versioning

### 36. How do you enable and use code-first migrations in EF?

<br>

### 37. Can you downgrade to a previous database schema using EF migrations?

<br>

### 38. How do you seed a database in EF?

<br>

### 39. Describe a scenario where you would use the Update-Database command.

<br>

### 40. How do you resolve migration conflicts in a team environment?

<br>

## 🚀 Entity Framework Performance

### 41. What is the N+1 problem and how can you avoid it in EF?

<br>

### 42. How does change tracking impact performance in EF?

<br>

### 43. What are some ways to optimize EF’s performance?

<br>

### 44. Can you explain how to use compiled queries in EF?

<br>

### 45. How do you manage the connection lifecycle for better performance in EF?

<br>

## 🔐 Entity Framework Concurrency

### 46. How does EF handle concurrency?

<br>

### 47. What is optimistic concurrency control in EF?

<br>

### 48. Explain how EF handles concurrency conflicts.

<br>

### 49. How do you use timestamps/row versions for concurrency in EF’s code-first approach?

<br>

### 50. What are the types of concurrency patterns that can be implemented with EF?

<br>

## 🧠 Entity Framework Advanced Features

### 51. Can you explain T4 templates in the context of EF?

<br>

### 52. What are interceptors in EF and when would you use them?

<br>

### 53. How do you implement inheritance in EF models?

<br>

### 54. What is Code-First Data Annotations and how do they work?

<br>

### 55. Can EF interact with view-models or must it always use database models?

<br>

## 🔗 Entity Framework and .NET Integration

### 56. How do you handle transactions across different data contexts in EF?

<br>

### 57. Can you unit test EF code?

<br>

### 58. How does EF fit into the .NET Core ecosystem?

<br>

### 59. What is dependency injection and how do you use it with EF?

<br>

### 60. How does EF work with asynchronous programming in .NET?

<br>

## 🧰 Entity Framework Troubleshooting

### 61. How do you troubleshoot performance issues in EF?

<br>

### 62. What are the common exceptions in EF and how do you resolve them?

<br>

### 63. How can you debug EF’s generated SQL queries?

<br>

### 64. What are the steps to resolve issues with entity states not updating as expected?

<br>

### 65. How do you troubleshoot issues with migrations in EF?

<br>

## ✅ Entity Framework Best Practices

### 66. What are some best practices for managing DbContext life cycle?

<br>

### 67. How do you manage connection strings securely when using EF?

<br>

### 68. What are the recommended approaches for large-scale enterprise EF applications?

<br>

### 69. How should you organize your EF code-first migrations?

<br>

### 70. What are some practices to avoid when working with EF?

<br>

## 🔐 Entity Framework Security

### 71. How does EF handle SQL injection prevention?

<br>

### 72. What are the security considerations regarding EF in a multi-user environment?

<br>

### 73. How do you secure sensitive data within EF models?

<br>

### 74. Can you explain the role of encryption in EF?

<br>

### 75. What security measures does EF provide out of the box?

<br>

## ☁️ Entity Framework and Azure

### 76. How do you deploy an EF application to Azure?

<br>

### 77. What are the considerations when using EF with Azure SQL Database?

<br>

### 78. Can you use EF with Azure Cosmos DB?

<br>

### 79. How do you manage scalability concerns with EF on Azure?

<br>

### 80. Explain how EF can be part of a microservices architecture in Azure.

<br>

## 🌐 Entity Framework and Cross-Platform Development

### 81. Is Entity Framework cross-platform compatible?

<br>

### 82. How do you develop with EF on non-Windows platforms?

<br>

### 83. What limitations should you be aware of when using EF Core over EF 6?

<br>

### 84. Explain how to run EF on a Mac or Linux system.

<br>

### 85. How do you handle platform-specific database functions in a cross-platform EF application?

<br>

## 🧩 Entity Framework and Third-Party Solutions

### 86. How does EF integrate with third-party libraries and frameworks?

<br>

### 87. What third-party tools are available for managing EF migrations?

<br>

### 88. Can you extend EF with custom providers?

<br>

### 89. How do you incorporate EF with popular JavaScript front-end frameworks?

<br>

### 90. What ORM alternatives to EF are there and when might you use them?

<br>

## 🔄 Entity Framework Updates and Evolution

### 91. Describe the key improvements of EF Core compared to previous versions of EF.

<br>

### 92. How does EF handle versioning and compatibility with the .NET framework?

<br>

### 93. What new features are expected in upcoming versions of EF?

<br>

### 94. How does community feedback influence the development of EF?

<br>

### 95. Discuss the process of upgrading from EF 6 to EF Core.

<br>

## 🏗️ Entity Framework and Database Interactions

### 96. How do you manage database initialization strategies in EF?

<br>

### 97. Can EF be used with nonsupported databases through custom providers?

<br>

### 98. How does EF work with database views and stored procedures?

<br>

### 99. Explain how EF approaches SQL server-specific features like hierarchyID and geography types.

<br>

### 100. Discuss the support of Entity Framework for NoSQL databases.

<br>

---
