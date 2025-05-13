
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
### 🔗 Mapping Associations in Entity Framework

In Entity Framework, **associations** define how two or more entities relate to each other. EF supports three main types of relationships: **One-to-One**, **One-to-Many**, and **Many-to-Many**. These associations can be configured using **Data Annotations** or **Fluent API**.

---

### 🧍‍♂️ One-to-One (1:1)

#### 📌 Description:

Each record in one table relates to one record in another.

#### ✅ Example:

```csharp
public class Person
{
    public int PersonId { get; set; }
    public string Name { get; set; }
    public virtual Passport Passport { get; set; }
}

public class Passport
{
    [Key, ForeignKey("Person")]
    public int PersonId { get; set; }
    public string Number { get; set; }
    public virtual Person Person { get; set; }
}
```

* Uses shared primary key as foreign key.
* Can also be done using Fluent API:

```csharp
modelBuilder.Entity<Person>()
    .HasOne(p => p.Passport)
    .WithOne(p => p.Person)
    .HasForeignKey<Passport>(p => p.PersonId);
```

---

### 👨‍👩‍👧 One-to-Many (1\:N)

#### 📌 Description:

One record in the parent relates to many in the child.

#### ✅ Example:

```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public virtual ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int OrderId { get; set; }
    public int CustomerId { get; set; }
    public virtual Customer Customer { get; set; }
}
```

* `Order.CustomerId` is the foreign key.
* EF creates the relationship automatically if navigation and key are set properly.

---

### 🔄 Many-to-Many (N\:N)

#### 📌 EF Core 5+ Supports Direct Mapping Without a Join Class:

```csharp
public class Student
{
    public int StudentId { get; set; }
    public ICollection<Course> Courses { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public ICollection<Student> Students { get; set; }
}
```

EF auto-generates the join table.

#### 📌 If Additional Fields Needed in Join Table:

Create a join entity:

```csharp
public class Enrollment
{
    public int StudentId { get; set; }
    public int CourseId { get; set; }
    public DateTime EnrolledOn { get; set; }

    public Student Student { get; set; }
    public Course Course { get; set; }
}
```

Configure using Fluent API:

```csharp
modelBuilder.Entity<Enrollment>()
    .HasKey(e => new { e.StudentId, e.CourseId });

modelBuilder.Entity<Enrollment>()
    .HasOne(e => e.Student)
    .WithMany(s => s.Enrollments)
    .HasForeignKey(e => e.StudentId);
```

---

### 🛠 Configuration Methods

#### ✅ Data Annotations

* `[ForeignKey]`
* `[InverseProperty]`
* `[Required]`

#### ✅ Fluent API

```csharp
modelBuilder.Entity<Order>()
    .HasOne(o => o.Customer)
    .WithMany(c => c.Orders)
    .HasForeignKey(o => o.CustomerId);
```

* Offers more control and customization.

---

### 🔁 Cascade Delete Configuration

You can define how deletions should cascade:

```csharp
modelBuilder.Entity<Order>()
    .HasOne(o => o.Customer)
    .WithMany(c => c.Orders)
    .OnDelete(DeleteBehavior.Cascade);
```

---

### ✅ Answer Summary

* EF supports **1:1**, **1\:N**, and **N\:N** associations.
* Use **navigation properties** and **foreign keys** to define relationships.
* Configure associations using **data annotations** or **Fluent API**.
* EF Core 5+ supports **many-to-many without a join entity**.
* Use Fluent API for complex setups and better control over mapping behavior.

<br>

### 27. Can you explain the concept of table per hierarchy and table per type inheritance in EF?
### 🏛 Inheritance Mapping in Entity Framework: TPH vs TPT

Entity Framework supports **inheritance** in the data model, allowing related classes to be stored in the database. Two main strategies for mapping inheritance are:

---

### 🧬 Table per Hierarchy (TPH)

#### 📌 Concept:

* All classes in the inheritance tree are mapped to **a single table** in the database.
* A **discriminator column** is used to identify which derived class a row belongs to.

#### ✅ Example:

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Manager : Employee
{
    public int TeamSize { get; set; }
}

public class Developer : Employee
{
    public string Language { get; set; }
}
```

All the above classes will be saved in a **single `Employees` table**:

| Id | Name  | TeamSize | Language | Discriminator |
| -- | ----- | -------- | -------- | ------------- |
| 1  | Alice | 10       | NULL     | Manager       |
| 2  | Bob   | NULL     | C#       | Developer     |

#### ✅ Configuration:

EF Core automatically uses TPH by default. You can customize the discriminator:

```csharp
modelBuilder.Entity<Employee>()
    .HasDiscriminator<string>("Discriminator")
    .HasValue<Manager>("Manager")
    .HasValue<Developer>("Developer");
```

#### ⚠️ Pros:

* Better **performance** due to fewer joins.
* Simpler schema.

#### ⚠️ Cons:

* Lots of **null columns**.
* Harder to enforce **strict schema rules** per derived type.

---

### 🧾 Table per Type (TPT)

#### 📌 Concept:

* Each class in the hierarchy is mapped to **its own table**.
* Derived tables contain only properties declared in the derived class.
* Joined at runtime using **inner joins**.

#### ✅ Example Tables:

* `Employees`: Id, Name
* `Managers`: Id, TeamSize
* `Developers`: Id, Language

#### ✅ Configuration:

```csharp
modelBuilder.Entity<Manager>().ToTable("Managers");
modelBuilder.Entity<Developer>().ToTable("Developers");
```

#### ⚠️ Pros:

* Clean separation of concerns.
* No nulls — each table has only relevant columns.

#### ⚠️ Cons:

* **Slower performance** due to joins.
* More **complex schema**.

---

### ⚖️ Key Differences

| Feature              | Table per Hierarchy (TPH) | Table per Type (TPT) |
| -------------------- | ------------------------- | -------------------- |
| Tables Used          | Single table              | One per class        |
| Schema Simplicity    | High                      | Low                  |
| Query Performance    | Fast                      | Slower (joins)       |
| Null Columns         | Many                      | None                 |
| Schema Normalization | Low                       | High                 |

---

### 💡 When to Use

* Use **TPH** for **performance-sensitive** applications and simple hierarchies.
* Use **TPT** for **data clarity**, **strict schema validation**, or when the hierarchy is large and complex.

---

### ✅ Answer Summary

* **TPH (Table per Hierarchy)**: One table for all classes, with a discriminator column. Faster, but can include many nulls.
* **TPT (Table per Type)**: Separate table per class. Cleaner schema, but slower due to joins.
* EF Core uses **TPH by default**; TPT must be explicitly configured.
* Choose based on performance needs vs schema design clarity.

<br>

### 28. How do you handle one-to-one, one-to-many, and many-to-many relationships in EF?
Entity Framework supports **three types of relationships** between entities: **One-to-One**, **One-to-Many**, and **Many-to-Many**. These are defined using **navigation properties**, **foreign keys**, and optionally configured using **Data Annotations** or **Fluent API**.

---

### 🧍‍♂️ One-to-One Relationship

#### 📌 Concept:

Each record in one table corresponds to one in another.

#### ✅ Example:

```csharp
public class User
{
    public int UserId { get; set; }
    public virtual Profile Profile { get; set; }
}

public class Profile
{
    [Key, ForeignKey("User")]
    public int UserId { get; set; }
    public string Bio { get; set; }
    public virtual User User { get; set; }
}
```

#### ✅ Fluent API:

```csharp
modelBuilder.Entity<User>()
    .HasOne(u => u.Profile)
    .WithOne(p => p.User)
    .HasForeignKey<Profile>(p => p.UserId);
```

#### 🔑 Notes:

* Uses a **shared primary key** as a foreign key.
* Not commonly used but supported.

---

### 👨‍👩‍👧 One-to-Many Relationship

#### 📌 Concept:

One record in a parent table relates to multiple in a child table.

#### ✅ Example:

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

#### ✅ Fluent API:

```csharp
modelBuilder.Entity<Post>()
    .HasOne(p => p.Blog)
    .WithMany(b => b.Posts)
    .HasForeignKey(p => p.BlogId);
```

#### 🔑 Notes:

* EF automatically detects one-to-many if naming conventions are followed.
* Parent must expose a collection; child must reference parent.

---

### 🔁 Many-to-Many Relationship

#### 📌 EF Core 5+ (No Join Entity Required):

```csharp
public class Student
{
    public int StudentId { get; set; }
    public ICollection<Course> Courses { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public ICollection<Student> Students { get; set; }
}
```

EF automatically creates the join table `StudentCourse`.

#### 📌 With Join Entity (Needed in EF Core < 5 or for extra columns):

```csharp
public class Enrollment
{
    public int StudentId { get; set; }
    public int CourseId { get; set; }
    public DateTime EnrolledOn { get; set; }

    public Student Student { get; set; }
    public Course Course { get; set; }
}
```

#### ✅ Fluent API:

```csharp
modelBuilder.Entity<Enrollment>()
    .HasKey(e => new { e.StudentId, e.CourseId });

modelBuilder.Entity<Enrollment>()
    .HasOne(e => e.Student)
    .WithMany(s => s.Enrollments)
    .HasForeignKey(e => e.StudentId);
```

#### 🔑 Notes:

* Preferred when you need extra data in the join table.
* Always configure join entity using Fluent API.

---

### ✅ Answer Summary

* **One-to-One**: Use a shared key as a foreign key; configure with `[ForeignKey]` or Fluent API.
* **One-to-Many**: Parent has collection, child has reference; EF auto-detects if conventions are followed.
* **Many-to-Many**:

  * EF Core 5+ supports direct mapping without join entity.
  * Use a **join class** if extra fields are needed.
* Use **Fluent API** for detailed control over relationships.

<br>

### 29. What is cascade delete and how is it configured in EF?
### 🧨 What is Cascade Delete in Entity Framework?

Cascade delete means that when a **parent entity is deleted**, its **related child entities are automatically deleted** from the database. This ensures **referential integrity** and prevents orphaned records.

EF can configure cascade delete behavior either through **conventions**, **data annotations**, or **Fluent API**.

---

### 🔁 How Cascade Delete Works

#### 📌 Example:

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

If a `Blog` is deleted, all its `Posts` can be automatically deleted with cascade delete enabled.

---

### ⚙️ Configuring Cascade Delete

#### ✅ By Convention (One-to-Many):

* EF Core enables cascade delete **by default** for required relationships (non-nullable foreign keys).

#### ✅ Using Fluent API:

```csharp
modelBuilder.Entity<Post>()
    .HasOne(p => p.Blog)
    .WithMany(b => b.Posts)
    .HasForeignKey(p => p.BlogId)
    .OnDelete(DeleteBehavior.Cascade);
```

#### ❌ Other Delete Behaviors:

* `DeleteBehavior.Restrict` – Prevents delete if child exists.
* `DeleteBehavior.SetNull` – Sets FK to null on delete (for optional relationships).
* `DeleteBehavior.NoAction` – Leaves child rows untouched (manual delete required).

---

### ⚠️ Notes & Best Practices

* Cascade delete should be **used cautiously**, especially with large data sets to avoid unintended mass deletions.
* SQL Server also enforces foreign key cascade behavior; EF's config should match the DB schema.
* In **EF Core**, cascade delete is enforced **on the database and in memory**.
* Be explicit with `OnDelete(...)` in complex models to avoid ambiguity.

---

### ✅ Answer Summary

* **Cascade delete** automatically removes related child entities when a parent is deleted.
* Configurable via **conventions**, **Fluent API**, or **data annotations**.
* Use `OnDelete(DeleteBehavior.Cascade)` in Fluent API to enable it.
* Use alternate behaviors like `Restrict`, `SetNull`, or `NoAction` based on business needs.
* Enabled by default for required relationships in EF Core.

<br>

### 30. How do you create a self-referencing association in EF?
### 🔄 Self-Referencing Association in Entity Framework

A **self-referencing association** occurs when an entity has a relationship with **itself**. This is useful in hierarchical data structures like **organization charts**, **categories**, or **tree-like structures**.

---

### 🧱 Use Case Example: Employee-Manager Relationship

Each employee may have **one manager**, and a manager may have **many subordinates**.

#### ✅ Entity Example:

```csharp
public class Employee
{
    public int EmployeeId { get; set; }
    public string Name { get; set; }

    public int? ManagerId { get; set; } // FK to self
    public Employee Manager { get; set; } // Navigation to manager
    public ICollection<Employee> Subordinates { get; set; } // Navigation to subordinates
}
```

---

### ⚙️ Configuring with Fluent API

Use Fluent API in `OnModelCreating` to configure the self-reference:

```csharp
modelBuilder.Entity<Employee>()
    .HasOne(e => e.Manager)
    .WithMany(e => e.Subordinates)
    .HasForeignKey(e => e.ManagerId)
    .OnDelete(DeleteBehavior.Restrict); // Prevent cascade delete if needed
```

---

### ✅ Behavior

* EF will create a **foreign key** in the same table (`ManagerId`) that points to the **primary key** (`EmployeeId`).
* You can **navigate up and down** the hierarchy using the navigation properties.

---

### 🔒 Optional FK

* `ManagerId` is nullable (`int?`) to support **top-level employees** (e.g., CEO) who don’t have a manager.

---

### 🧠 Common Scenarios

* **Category/Subcategory**
* **Menu/MenuItem**
* **Department/ParentDepartment**

---

### 🧪 Query Example

```csharp
var manager = context.Employees
    .Include(e => e.Subordinates)
    .FirstOrDefault(e => e.EmployeeId == 1);
```

---

### ✅ Answer Summary

* **Self-referencing associations** are used when an entity relates to itself (e.g., employee-manager).
* Requires a **nullable foreign key** and **navigation properties** pointing to the same entity.
* Use **Fluent API** to configure `.HasOne().WithMany().HasForeignKey()`.
* Common in hierarchical data like categories, menus, or organizational structures.

<br>

## 🔍 Entity Framework Queries and LINQ

### 31. How do you perform LINQ queries in EF?
### 🧾 LINQ Queries in Entity Framework

Entity Framework allows querying the database using **LINQ (Language Integrated Query)**, providing a strongly typed and expressive way to retrieve data using C# syntax. EF translates these queries into **SQL** and executes them against the database.

You can use two styles:

* **LINQ Method Syntax** (fluent style)
* **LINQ Query Syntax** (SQL-like)

---

### 🔍 LINQ Method Syntax

Most common in EF Core and modern C# codebases.

#### ✅ Example:

```csharp
var activeUsers = context.Users
    .Where(u => u.IsActive)
    .OrderBy(u => u.Name)
    .ToList();
```

#### 🔁 Includes and Joins:

```csharp
var blogs = context.Blogs
    .Include(b => b.Posts)
    .ToList();
```

---

### 📜 LINQ Query Syntax

More readable for those familiar with SQL.

#### ✅ Example:

```csharp
var activeUsers = from u in context.Users
                  where u.IsActive
                  orderby u.Name
                  select u;
```

---

### 🧠 Common Query Operations

* `.Where()` – Filters records.
* `.Select()` – Projects specific fields.
* `.Include()` – Loads related data (eager loading).
* `.FirstOrDefault()` / `.SingleOrDefault()` – Returns one item.
* `.Any()` / `.All()` – Checks for existence or condition.
* `.GroupBy()` – Groups records.
* `.Join()` – Joins two tables.

---

### 📦 Projection Example

You can project to an anonymous or custom type:

```csharp
var userSummaries = context.Users
    .Where(u => u.IsActive)
    .Select(u => new { u.Name, u.Email })
    .ToList();
```

---

### 🛠 Tips

* Use `ToList()` to execute the query and materialize the results.
* Avoid `ToList()` too early if you're planning to chain multiple operations (for performance).
* Always prefer `AsNoTracking()` for read-only queries to improve performance.

```csharp
var users = context.Users.AsNoTracking().Where(u => u.IsActive).ToList();
```

---

### ✅ Answer Summary

* LINQ in EF is used to write **strongly-typed, SQL-like queries** using C#.
* Supports **method syntax** (common) and **query syntax** (SQL-style).
* Use `.Where()`, `.Select()`, `.Include()`, `.Join()`, and other LINQ methods for data operations.
* LINQ queries are translated into **SQL** by EF and executed on the database.
* Use `AsNoTracking()` for faster read-only queries.

<br>

### 32. Explain the difference between eager loading, lazy loading, and explicit loading.
### ⚡ Eager Loading

Eager loading loads the **main entity and its related data** in a **single query**. This avoids additional trips to the database.

#### ✅ How to Use:

```csharp
var blogs = context.Blogs
    .Include(b => b.Posts)
    .ToList();
```

#### 🔍 Use When:

* You **know** you'll need related data.
* You want to **reduce the number of queries** (especially in loops).

---

### 🐌 Lazy Loading

Lazy loading loads related data **only when you access** the navigation property. EF makes a separate database call **on demand**.

#### ✅ How to Enable (EF Core):

* Install `Microsoft.EntityFrameworkCore.Proxies`
* Enable lazy loading:

```csharp
optionsBuilder.UseLazyLoadingProxies();
```

* Mark navigation property as `virtual`:

```csharp
public virtual ICollection<Post> Posts { get; set; }
```

#### ⚠️ Drawbacks:

* Can cause **N+1 query issues**.
* Harder to debug and less efficient if used carelessly.

---

### ✋ Explicit Loading

Explicit loading lets you manually load related data when you choose to, using `.Entry().Reference()` or `.Collection()`.

#### ✅ Example:

```csharp
var blog = context.Blogs.First(b => b.Id == 1);
context.Entry(blog)
    .Collection(b => b.Posts)
    .Load();
```

#### 🧠 Use When:

* You need control over when and what related data gets loaded.
* You want to avoid lazy loading but don’t want to always eagerly load everything.

---

### ✅ Answer Summary

* **Eager Loading**: Loads related data **with the main query** using `.Include()`. Best for known, needed relationships.
* **Lazy Loading**: Loads related data **on access** via navigation properties. May cause performance issues if misused.
* **Explicit Loading**: Loads related data **manually** using `.Entry().Load()`. Gives full control over loading timing and logic.

<br>

### 33. How does EF translate LINQ-to-Entities queries into SQL queries?
### 🧠 LINQ-to-Entities Translation in Entity Framework

Entity Framework translates **LINQ-to-Entities** queries into **SQL queries** that the underlying database can understand. This is done through **expression tree parsing and query providers**.

---

### 🛠 Expression Tree Parsing

* When you write a LINQ query (e.g., `.Where()`, `.Select()`), EF **builds an expression tree**.
* This tree represents the query structure in a form EF can analyze.

#### ✅ Example:

```csharp
var users = context.Users
    .Where(u => u.IsActive && u.Role == "Admin")
    .ToList();
```

* EF does **not execute immediately**. It builds an expression tree and waits for execution (e.g., `ToList()`).

---

### ⚙️ Query Provider

* EF uses a **query provider** (like `IQueryProvider`) that understands how to **convert the expression tree** to SQL.
* It walks the expression tree and translates it into a **parameterized SQL query** that maintains **SQL injection safety**.

---

### 🏁 Execution

* Once the query is executed (`ToList()`, `FirstOrDefault()`, etc.), EF:

  1. Sends the generated SQL to the database.
  2. Maps the result set back to your entity classes.

---

### 🧪 Sample Translation

#### LINQ Query:

```csharp
var users = context.Users
    .Where(u => u.IsActive)
    .OrderBy(u => u.Name)
    .ToList();
```

#### SQL Generated:

```sql
SELECT [u].[Id], [u].[Name], [u].[IsActive]
FROM [Users] AS [u]
WHERE [u].[IsActive] = 1
ORDER BY [u].[Name]
```

---

### 🚫 Unsupported Methods

* EF can only translate **supported .NET methods** (e.g., `Contains`, `StartsWith`, `Sum`).
* If a method can't be translated, EF throws a runtime exception unless it's evaluated in memory **after** materialization.

---

### 🛑 Client Evaluation (EF Core)

* In EF Core, any parts of the query that can't be translated will be evaluated **client-side** (after data is loaded).
* Use this **carefully**, especially with large datasets.

---

### ✅ Answer Summary

* EF translates LINQ-to-Entities into SQL using **expression trees** and a **query provider**.
* Queries are **not executed immediately**—they are parsed and translated when executed (e.g., with `ToList()`).
* Translation results in **parameterized SQL**, improving **performance** and **security**.
* EF **maps the result back** to entities after executing the SQL.
* Only **supported methods** are translated; unsupported ones may cause errors or trigger **client-side evaluation**.

<br>

### 34. Can you use LINQ to project results into a non-entity type?
### 🧾 Projecting Results into a Non-Entity Type Using LINQ

Yes, you can use **LINQ** to project results into a **non-entity type** (e.g., DTOs, anonymous objects, or custom types). This is useful when you need only a subset of data from the database or when you want to shape the data in a way that suits your needs, without fetching the entire entity.

---

### ✅ Projection to Anonymous Types

You can create **anonymous types** directly within your LINQ query. This is useful for simple scenarios where you don't need a strongly typed result.

#### Example:

```csharp
var userSummaries = context.Users
    .Where(u => u.IsActive)
    .Select(u => new { u.Name, u.Email })
    .ToList();
```

* This creates an **anonymous type** with the `Name` and `Email` properties.
* The query will still run efficiently since EF can translate it into SQL.

---

### ✅ Projection to Custom Types (DTOs)

For more complex scenarios, it's common to project the query results into a **custom class or DTO** (Data Transfer Object). This helps separate the data layer from your application's domain models.

#### Example (using DTO):

```csharp
public class UserSummaryDTO
{
    public string Name { get; set; }
    public string Email { get; set; }
}

var userSummaries = context.Users
    .Where(u => u.IsActive)
    .Select(u => new UserSummaryDTO { Name = u.Name, Email = u.Email })
    .ToList();
```

* This projection returns a list of `UserSummaryDTO` objects instead of the full `User` entities.

---

### 🚀 Benefits of Projecting into Non-Entity Types

* **Optimized performance**: You can avoid loading unnecessary data (like navigation properties or large fields).
* **Decouples data layer**: Helps in separating the internal domain model from external views (especially in APIs).
* **Cleaner code**: DTOs or anonymous types can give you the flexibility to structure data in a way that makes sense for your application.

---

### 🧠 Important Notes

* **LINQ projection** doesn't load the full entity; it only loads the fields you specify, making the query **more efficient**.
* EF **supports projections** directly into anonymous types or custom objects.
* When using **DTOs**, ensure that only the properties required for the query are selected.

---

### ✅ Answer Summary

* You can use **LINQ** to project query results into **non-entity types**, such as anonymous types or custom DTOs.
* **Anonymous types** are useful for simple projections, while **DTOs** allow for more flexibility and separation of concerns.
* Projecting only the needed fields improves **performance** by not fetching unnecessary data.

<br>

### 35. What is LINQ to Entities?
### ⚡ LINQ to Entities Overview

**LINQ to Entities** is a **part of Entity Framework (EF)** that enables you to use **LINQ (Language Integrated Query)** to query and manipulate data stored in a **relational database**. It allows you to write strongly-typed queries against the **Entity Data Model (EDM)** using LINQ syntax, and EF automatically translates these queries into **SQL** that the underlying database can execute.

---

### 🧠 Key Features of LINQ to Entities

1. **Object-Relational Mapping (ORM)**:

   * LINQ to Entities is part of the ORM functionality in EF.
   * It allows you to work with **entities** (like classes) rather than working directly with SQL statements or database records.

2. **Strongly Typed Queries**:

   * LINQ to Entities provides **compile-time checking** and **intellisense** support in Visual Studio.
   * It makes your queries more **type-safe** by ensuring they match the entity's structure.

3. **Automatic SQL Translation**:

   * EF automatically translates LINQ queries into **SQL** queries.
   * You write LINQ code in C#, and EF handles the translation to SQL.

4. **Deferred Execution**:

   * LINQ to Entities uses **deferred execution**, meaning that queries are not executed until the data is actually requested (e.g., with `.ToList()`, `.FirstOrDefault()`, etc.).

---

### ✅ Example of LINQ to Entities Query

```csharp
var activeUsers = context.Users
    .Where(u => u.IsActive)
    .OrderBy(u => u.Name)
    .ToList();
```

* **Explanation**:

  * This query gets all active users from the `Users` table, ordered by name.
  * EF translates it into SQL, like:

    ```sql
    SELECT [u].[Id], [u].[Name], [u].[IsActive]
    FROM [Users] AS [u]
    WHERE [u].[IsActive] = 1
    ORDER BY [u].[Name]
    ```

---

### 🛠 LINQ to Entities Supports:

* **Filtering** (`Where`)
* **Sorting** (`OrderBy`, `ThenBy`)
* **Grouping** (`GroupBy`)
* **Projection** (`Select`)
* **Aggregation** (`Sum`, `Count`, `Max`, `Min`)
* **Joining** (`Join`)

---

### 🚀 Benefits of LINQ to Entities

1. **Productivity**: LINQ provides a familiar, readable syntax that simplifies querying.
2. **Safety**: Type safety is guaranteed, reducing runtime errors.
3. **Abstraction**: You don't have to write raw SQL queries, making your code cleaner and more maintainable.
4. **Performance**: EF can optimize the queries it generates, reducing the need for manual query tuning.

---

### 🧠 Important Notes

* **LINQ to Entities** can only query data that **can be translated into SQL**. Some complex operations may not be supported or require **client-side evaluation**.
* It works only with **entity models** defined using EF.
* It provides **deferred execution**, so the query only executes when enumerated.

---

### ✅ Answer Summary

* **LINQ to Entities** is an EF feature that allows you to query a database using LINQ syntax.
* It translates LINQ queries into **SQL** that is executed against the database.
* LINQ to Entities supports **strongly-typed** queries, **deferred execution**, and various LINQ operations like filtering, sorting, grouping, and joining.
* It simplifies querying and improves productivity, while also ensuring **type safety**.

<br>

## 🧬 Entity Framework Migrations and Versioning

### 36. How do you enable and use code-first migrations in EF?
### 🔧 Enabling and Using Code-First Migrations in Entity Framework

**Code-First Migrations** in Entity Framework (EF) is a technique that allows you to define your data model (entities) using C# code, and then use **migrations** to apply changes to the database schema. It helps you manage database schema changes over time as your data model evolves.

---

### 🧠 Steps to Enable and Use Code-First Migrations

#### 1. **Enable Migrations in Your Project**

To use migrations, you first need to enable it in your project. This can be done using the **Package Manager Console** in Visual Studio.

* **Command**: Run the following command to enable migrations in your project:

  ```bash
  Enable-Migrations
  ```

* This creates a `Migrations` folder in your project with a **Configuration.cs** file. This file contains settings that control the migration behavior.

---

#### 2. **Configure the Migration Settings (Optional)**

In the `Configuration.cs` file, you can define whether migrations should be **enabled** or **disabled** for the project, and also specify the **automatic database migration** behavior.

For example:

```csharp
internal sealed class Configuration : DbMigrationsConfiguration<MyDbContext>
{
    public Configuration()
    {
        AutomaticMigrationsEnabled = true; // Automatically apply migrations on model changes
    }

    protected override void Seed(MyDbContext context)
    {
        // Optional: Add seed data here if necessary
    }
}
```

* `AutomaticMigrationsEnabled = true` will automatically apply migrations when the application starts.
* You can disable this and manually control the migration process.

---

#### 3. **Add a New Migration**

Once migrations are enabled, you can create a migration to capture changes made to your data model.

* **Command**: Run the following command to add a new migration:

  ```bash
  Add-Migration MigrationName
  ```

* **Explanation**: Replace `MigrationName` with a descriptive name that reflects the changes made (e.g., `AddCustomerTable`).

* This generates a new migration file inside the `Migrations` folder, containing the SQL needed to update the database schema.

---

#### 4. **Apply the Migration to the Database**

After creating the migration, you can apply it to the database using the following command:

* **Command**: Run the following command to update the database schema:

  ```bash
  Update-Database
  ```

* This applies the **latest migration** to your database, ensuring the schema is up to date with your model.

---

#### 5. **Viewing and Managing Migrations**

* **List Migrations**: To see all applied migrations, use:

  ```bash
  Get-Migrations
  ```

* **Reverting Migrations**: You can revert to a previous migration if necessary:

  ```bash
  Update-Database -TargetMigration MigrationName
  ```

* **Rollback All Migrations**: You can rollback all migrations with:

  ```bash
  Update-Database -TargetMigration:0
  ```

---

### 🚀 Benefits of Using Code-First Migrations

* **Version Control**: Migrations help track database changes alongside your application code.
* **Flexibility**: You can evolve the database schema over time, even after the application has been deployed.
* **Easy Deployment**: Migrations make it easy to update or roll back database changes in different environments (development, staging, production).

---

### 🧠 Important Notes

* Always **test migrations** on a local or staging database before applying them to production.
* Be careful when using `AutomaticMigrationsEnabled`, especially in production environments, as it can lead to unintended schema changes.
* Migrations can handle **data transformations** (e.g., renaming columns) in addition to schema updates.

---

### ✅ Answer Summary

* **Enable migrations** using `Enable-Migrations` in the Package Manager Console.
* **Create migrations** by running `Add-Migration MigrationName` after making changes to your data model.
* **Apply migrations** to the database using `Update-Database`.
* You can manage and revert migrations with **commands** like `Get-Migrations` and `Update-Database -TargetMigration`.
* Code-first migrations allow you to **track and apply database schema changes** alongside your application code, providing flexibility and version control for your database.

<br>

### 37. Can you downgrade to a previous database schema using EF migrations?
### ⚡ Downgrading to a Previous Database Schema Using EF Migrations

Yes, you can downgrade to a previous database schema using **Entity Framework (EF) migrations**. This is typically done when you want to **revert the database** to a previous state due to a schema change being problematic or not needed anymore.

---

### 🧠 Steps to Downgrade the Database Schema

#### 1. **Identify the Target Migration**

To downgrade your database to a previous schema, you need to know the migration you want to revert to. You can view a list of all migrations by running the following command in the **Package Manager Console**:

* **Command**:

  ```bash
  Get-Migrations
  ```

This will show you all the migrations that have been applied to the project, and you can select the one you want to revert to.

---

#### 2. **Revert to a Specific Migration**

Once you have identified the migration you want to revert to, you can use the `Update-Database` command with the `-TargetMigration` option to downgrade the database schema.

* **Command**:

  ```bash
  Update-Database -TargetMigration MigrationName
  ```

Replace `MigrationName` with the name of the migration you want to revert to. EF will generate the SQL required to undo the changes made in the migrations after the specified migration.

For example, if you want to downgrade to a migration called `AddCustomerTable`, you would run:

```bash
Update-Database -TargetMigration AddCustomerTable
```

---

#### 3. **Rolling Back All Migrations**

If you want to completely **reset** the database and undo all migrations, you can specify `0` as the target migration. This will remove all migrations and revert the database to its **initial state**.

* **Command**:

  ```bash
  Update-Database -TargetMigration:0
  ```

This command will remove all changes made by migrations, effectively reverting the database schema to the state it was in before any migrations were applied.

---

### 🚀 Important Points to Consider

* **Data Loss**: Downgrading migrations can result in **data loss** if migrations involved dropping tables or columns. Be cautious when rolling back migrations, especially in production environments.
* **Testing**: Always test downgrades on a **local or staging** environment before applying them to production to ensure no data integrity issues occur.
* **Seed Data**: If you’ve used the `Seed` method in your migrations, reverting to an older migration may also require adjustments to your seed data.

---

### ✅ Answer Summary

* **Yes**, you can downgrade to a previous database schema using EF migrations.
* Use the `Update-Database -TargetMigration MigrationName` command to revert to a specific migration.
* To reset the entire database schema, use `Update-Database -TargetMigration:0`.
* Always **test** the downgrade process carefully to avoid potential data loss or integrity issues.

<br>

### 38. How do you seed a database in EF?
### 🌱 Seeding a Database in Entity Framework

**Database seeding** in Entity Framework (EF) refers to the process of populating the database with initial data when the database is created or migrated. This is useful for adding default or sample data, such as system settings, admin users, or lookup tables.

---

### 🧠 Steps to Seed a Database in Entity Framework

#### 1. **Override the `Seed` Method in the Configuration Class**

In **EF Code First** approach, the seeding process is typically done by overriding the `Seed` method in the **Configuration class** located in the `Migrations` folder. This method allows you to insert default or test data into the database when the migrations are applied.

* The `Seed` method is automatically called after each migration is applied to the database.

Here’s an example of how to override the `Seed` method in your `Configuration.cs` file:

```csharp
internal sealed class Configuration : DbMigrationsConfiguration<MyDbContext>
{
    public Configuration()
    {
        AutomaticMigrationsEnabled = true; // Optional: enable auto migrations
    }

    protected override void Seed(MyDbContext context)
    {
        // Add initial data to the database
        context.Users.AddOrUpdate(
            u => u.Email,  // Ensure unique Email for update or insert
            new User { Email = "admin@example.com", Name = "Admin User", Role = "Administrator" },
            new User { Email = "user@example.com", Name = "Sample User", Role = "User" }
        );

        // Save the changes to the database
        context.SaveChanges();
    }
}
```

* In this example, `AddOrUpdate` ensures that if the data already exists (based on the `Email` field), it will be updated, otherwise, it will be inserted.

---

#### 2. **Add Data During Database Initialization (Optional)**

In older versions of EF (prior to EF 7), you could also use the **Database.SetInitializer** method to seed the database. This approach is **less common** with recent EF versions, but it’s useful in some scenarios.

```csharp
Database.SetInitializer(new MyDbInitializer());
```

However, **EF 7 and later** mainly rely on the `Seed` method in the migration configuration class for seeding.

---

#### 3. **Running Migrations and Seeding Data**

Once the `Seed` method is implemented, whenever you **apply migrations** using `Update-Database`, the `Seed` method is executed, and the database is populated with the defined data.

* **Command**:

  ```bash
  Update-Database
  ```

---

#### 4. **Seeding Data for Testing or Development Purposes**

If you need to seed data **only for development or testing** purposes, you can use conditions inside the `Seed` method to check whether the data already exists in the database, avoiding duplicate entries.

For example, checking if a user exists before adding:

```csharp
if (!context.Users.Any(u => u.Email == "admin@example.com"))
{
    context.Users.Add(new User { Email = "admin@example.com", Name = "Admin User", Role = "Administrator" });
}
```

---

### 🚀 Key Points to Remember

* **Override the `Seed` method** in the **Configuration class** to define default data.
* Use **`AddOrUpdate`** to ensure data is not duplicated and to update if necessary.
* Seeding is applied automatically when running **migrations** with `Update-Database`.
* **Testing or development** specific seed data can be inserted conditionally to avoid duplication.
* The `Seed` method can be a powerful way to set up your database with the necessary initial data.

---

### ✅ Answer Summary

* **Seed the database** by overriding the `Seed` method in the `Configuration.cs` class in EF.
* Use methods like `AddOrUpdate` to avoid duplicating data when running migrations.
* Apply seeding automatically through the `Update-Database` command.
* Seed data can be added conditionally for specific environments like development or testing.

<br>

### 39. Describe a scenario where you would use the Update-Database command.
### 🧠 Scenario for Using the `Update-Database` Command

The **`Update-Database`** command in Entity Framework (EF) is commonly used when working with **migrations** in a Code First approach. It helps to apply pending migrations to the database, ensuring the database schema is in sync with the application's data model.

---

### 🔄 Common Scenario: Applying Database Schema Changes

**Situation:**
You are working on a web application, and your development team has added new features that require changes to the database schema, such as adding new tables, columns, or relationships. You have already created migrations to reflect these changes in the EF model.

**Steps in the Workflow:**

1. **Create Migrations for Schema Changes**
   After you modify the models, you create a migration using the **`Add-Migration`** command.

   * **Command**:

     ```bash
     Add-Migration AddCustomerTable
     ```

2. **Review and Apply Migrations**
   After the migration is created, it needs to be applied to the database to reflect the changes in the database schema.

3. **Use the `Update-Database` Command**
   To apply the migration, use the **`Update-Database`** command to update the database schema to the latest version.

   * **Command**:

     ```bash
     Update-Database
     ```

   This command:

   * Applies any **pending migrations** to the database.
   * Ensures the **database schema** matches the model defined in the application.
   * Runs the SQL statements generated by migrations to update the database.

---

### 🚀 Additional Scenarios for Using `Update-Database`

#### 1. **Deploying to a New Environment**

When deploying an application to a **production or staging environment**, you may need to apply migrations to update the database schema.

* **Example Command**:

  ```bash
  Update-Database -ConnectionString "ProductionConnectionString"
  ```

#### 2. **Rolling Back to a Specific Migration**

You can also use `Update-Database` to **downgrade** the database to a previous migration if needed.

* **Example Command**:

  ```bash
  Update-Database -TargetMigration PreviousMigrationName
  ```

This helps if you need to revert a schema change due to an issue.

#### 3. **Seeding Data**

If you have configured data seeding, using **`Update-Database`** will also run the **`Seed`** method to populate the database with initial data.

---

### ✅ Answer Summary

* The **`Update-Database`** command is used to **apply pending migrations** and synchronize the database schema with the application's data model.
* It is typically used after creating migrations to reflect changes in the database schema.
* You can also use it to **downgrade** the database to a previous state or **seed initial data** if configured.
* The **`Update-Database`** command is essential in both development and deployment scenarios for keeping the database up-to-date.

<br>

### 40. How do you resolve migration conflicts in a team environment?
### 🧠 Resolving Migration Conflicts in a Team Environment

In a team environment, **migration conflicts** can occur when multiple developers are working on the same project and create different migrations that modify the same parts of the database schema (such as adding, deleting, or modifying tables/columns). These conflicts can result in errors or unexpected behavior when applying migrations to the database.

Here's how to handle these migration conflicts effectively:

---

### 🔧 Steps to Resolve Migration Conflicts

#### 1. **Communicate with the Team**

* **Prevention is better than cure**. Encourage team members to communicate frequently about schema changes to avoid working on the same areas.
* Use **branching strategies** where migrations are created on feature branches and merged into the main branch when complete.

#### 2. **Identify the Conflict**

* When a conflict occurs, you will often see a message indicating that two migrations modify the same part of the schema.
* **Review the generated migrations**. The conflicting migrations might try to modify the same tables, fields, or relationships, leading to a conflict.

#### 3. **Revert or Delete Conflicting Migrations**

* **Delete the migration that is causing the conflict**:
  If one migration is redundant or no longer needed, you can delete it using the `Remove-Migration` command before re-running the migration process.

  * **Command**:

    ```bash
    Remove-Migration
    ```
  * This will **delete the last migration** and roll back any pending schema changes from the migration.
* If multiple migrations are conflicting, you may need to **recreate a new migration** that consolidates the changes.

#### 4. **Rebase the Migrations**

* If two developers have created migrations based on the same initial state, and there are schema changes in both migrations, **rebase the migrations**.
* This involves:

  1. **Rolling back to the baseline** migration.
  2. **Recreating migrations** that merge the changes made by both developers.

#### 5. **Merge the Migrations Manually**

* If conflicts are complex, you might need to **merge the migrations manually**:

  * Look at the **SQL generated** by each migration.
  * Combine both migrations into a new migration file by manually editing it.
  * Ensure that the database schema is in sync with both developers' changes before applying it.

#### 6. **Apply the Merged Migration**

* Once the conflict is resolved and the migrations are merged, apply the **new migration** using the `Update-Database` command.

* **Command**:

  ```bash
  Update-Database
  ```

#### 7. **Test the Migrations**

* After resolving the conflict and applying the new migration, ensure that the database schema works as expected.
* Run your application, and perform **integration or acceptance tests** to verify that the database changes were applied correctly.

---

### 🚀 Best Practices to Prevent Migration Conflicts

* **Frequent Migrations**: Create migrations **frequently** to avoid multiple changes piling up, which increases the risk of conflicts.
* **Clear Branching Strategy**: Adopt a clear branching strategy where each developer works on separate areas of the schema. Merge changes carefully into the main branch.
* **Review Migrations Before Applying**: Always review generated migrations before committing them to avoid introducing unnecessary changes.
* **Coordinate Changes**: If multiple developers are working on related parts of the schema, **coordinate changes** to ensure no one is modifying the same part.

---

### ✅ Answer Summary

* **Migrations conflict** when multiple developers change the same part of the schema.
* **Communicate** within the team and review migrations before committing.
* Resolve conflicts by **removing or reverting migrations**, and **rebase or merge** changes manually if necessary.
* After resolving conflicts, **test the migrations** to ensure they work as expected.
* Use best practices like **frequent migrations** and **clear branching strategies** to prevent conflicts.

<br>

## 🚀 Entity Framework Performance

### 41. What is the N+1 problem and how can you avoid it in EF?
### 🧠 N+1 Problem in Entity Framework

The **N+1 problem** occurs when an application issues one query to retrieve a set of records (let's say "N" records) and then issues an additional query for each individual record (thus generating N additional queries). This results in a total of N+1 queries, which can significantly degrade performance, especially when working with large datasets.

---

### 🔄 How the N+1 Problem Happens

Consider the following scenario where you have a list of **Orders**, and each **Order** has a related **Customer**.

* You retrieve a list of orders: `db.Orders.ToList()`
* For each order, you then fetch the related customer: `order.Customer.Name`

If **N orders** are retrieved, the **N+1 queries** are as follows:

1. A query to retrieve all orders.
2. **N additional queries** to retrieve the customer for each order.

This results in **N+1** queries instead of just **1 query** for all related data.

---

### 🔧 How to Avoid the N+1 Problem in EF

#### 1. **Use Eager Loading (`Include`)**

* **Eager Loading** loads the related data along with the main query in a single query.
* Use the **`Include`** method to load related entities along with the main entity in one go.

```csharp
var ordersWithCustomers = db.Orders.Include(o => o.Customer).ToList();
```

This will generate a **single query** that retrieves all orders along with their associated customers in a **JOIN** operation, eliminating the N+1 problem.

#### 2. **Use Select with Projections**

* If you only need specific fields from the related entity, **project** the results into a custom object or a DTO (Data Transfer Object).
* This approach reduces the amount of data fetched and avoids unnecessary columns or joins.

```csharp
var orderCustomerDetails = db.Orders
    .Select(o => new { o.OrderId, o.Customer.Name })
    .ToList();
```

By using projections, you fetch only the required fields and avoid fetching the entire related entity.

#### 3. **Lazy Loading Considerations**

* **Lazy Loading** can contribute to the N+1 problem if it's used incorrectly.
* By default, EF Core will automatically load related data only when you access a navigation property, causing additional queries for each related entity.
* To prevent lazy loading from contributing to N+1 queries, either **disable lazy loading** or use **eager loading** explicitly where needed.

```csharp
dbContext.ChangeTracker.LazyLoadingEnabled = false;
```

#### 4. **Use Explicit Loading**

* If you need to load related entities after the main entity is retrieved, use **explicit loading**.
* This is useful when you want to load data on-demand but avoid the N+1 problem by fetching related data in batches.

```csharp
var order = db.Orders.First();
db.Entry(order).Reference(o => o.Customer).Load();
```

You can also use **`Collection.Load`** for loading related collections.

---

### 🚀 Best Practices to Avoid N+1 Problem

* **Always use `Include` for related data** when you know you'll need it, to avoid lazy loading multiple queries.
* **Optimize queries by selecting only the necessary fields** to avoid fetching excess data.
* **Use projections** to reduce the amount of data transferred from the database, especially for large datasets.
* **Disable Lazy Loading** if not required or if it leads to N+1 queries.
* Regularly **profile and monitor** your application's database calls to identify potential performance bottlenecks caused by N+1 queries.

---

### ✅ Answer Summary

* The **N+1 problem** occurs when an application makes N+1 queries to fetch related data, leading to performance issues.
* **Eager Loading** with **`Include`** eliminates the N+1 problem by fetching related data in a single query.
* **Projections** reduce the amount of data fetched by selecting only necessary fields.
* **Lazy Loading** can cause N+1 queries if used incorrectly, so it’s best to disable it when not needed or use eager loading.
* **Explicit Loading** can be used to load related entities on-demand while avoiding multiple queries.

<br>

### 42. How does change tracking impact performance in EF?
### 🧠 Impact of Change Tracking on Performance in EF

**Change tracking** is a core feature in **Entity Framework** (EF) that keeps track of changes made to entities during the lifecycle of the context. It allows EF to determine what data needs to be updated when calling `SaveChanges()`. While change tracking provides a lot of convenience, it can also impact performance, especially with large datasets or complex queries.

---

### 🔄 How Change Tracking Works in EF

When you retrieve entities from the database using EF, it tracks the state of those entities. EF manages their state using one of the following states:

* **Added**: Entity is new and should be inserted.
* **Modified**: Entity has been updated and should be updated in the database.
* **Deleted**: Entity is marked for deletion.
* **Unchanged**: No changes have been made to the entity.

EF monitors changes to properties of entities and ensures that only the modified data is sent to the database when `SaveChanges()` is called.

---

### 🔧 Impact on Performance

#### 1. **Memory Consumption**

* **Change tracking requires memory** to keep track of all entities loaded into the context. If you're working with large datasets, EF will need to store metadata about each entity, including its state, original values, and current values.
* **Solution**: If you only need to read data and don’t need to track changes, you can disable change tracking for performance gains.

```csharp
dbContext.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
```

This will make EF fetch the data without keeping track of the changes, improving performance in read-heavy scenarios.

#### 2. **Overhead for Large Datasets**

* When querying large datasets, **change tracking can create overhead** as EF tracks the state of all loaded entities. This can significantly slow down the application, especially in cases where many entities are being retrieved without modification.
* **Solution**: Use **`AsNoTracking()`** for read-only queries to avoid change tracking. This will help in scenarios where you just need to display data or perform calculations without modifying it.

```csharp
var data = dbContext.Products.AsNoTracking().ToList();
```

This improves performance by reducing memory usage and eliminating the need for EF to track each entity’s state.

#### 3. **Complex Queries**

* For complex queries that retrieve **multiple related entities** (with `Include` or nested navigation properties), EF has to track and compare each entity’s state. This can introduce a performance hit.
* **Solution**: Use **`AsNoTracking()`** in complex queries if you're not going to modify the data, especially when using multiple joins or loading large sets of related data.

#### 4. **Saving Changes**

* When you call `SaveChanges()`, EF has to **compare the current values with the original values** for each tracked entity to generate the necessary SQL commands (INSERT, UPDATE, DELETE). This comparison process can be costly for large numbers of entities.
* **Solution**: If only a few entities are modified, you can **detach** other entities from the change tracker to reduce overhead.

```csharp
dbContext.Entry(entity).State = EntityState.Detached;
```

Detaching entities ensures that EF no longer tracks them, reducing the work done during `SaveChanges()`.

#### 5. **Tracking Entity Relationships**

* **Tracking related entities** (e.g., one-to-many, many-to-many) can introduce additional overhead. EF will need to maintain references to child or parent entities, which increases memory consumption and processing time.
* **Solution**: **Disable tracking** when you only need the top-level entities, and you don’t need to modify or access the related entities.

---

### 🚀 Best Practices to Optimize Change Tracking

* **Use `AsNoTracking()`** for read-only queries to avoid the overhead of change tracking.
* **Detach entities** after they are no longer needed to free up memory and reduce overhead.
* **Limit the data loaded** by selecting only the necessary columns or entities to reduce the number of objects EF needs to track.
* For **batch updates or inserts**, consider using **raw SQL** or **stored procedures** to bypass change tracking entirely.
* Use **projection** (e.g., `Select`) to load only the required data, rather than entire entities when working with large datasets.

---

### ✅ Answer Summary

* **Change tracking** in EF allows EF to track modifications to entities but can impact performance due to memory usage and overhead for large datasets.
* Use **`AsNoTracking()`** for read-only queries to improve performance by avoiding tracking.
* Change tracking creates overhead when working with large datasets or complex queries, so avoid it when modifications are not needed.
* **Detaching entities** and selecting only the necessary data can help optimize performance in scenarios where tracking is unnecessary.

<br>

### 43. What are some ways to optimize EF’s performance?
### 🧠 Optimizing Entity Framework Performance

Optimizing Entity Framework (EF) performance is crucial to ensure your application runs efficiently, especially as the complexity of your database grows. Here are some effective ways to improve EF performance.

---

### 🔄 1. **Disable Change Tracking for Read-Only Queries**

EF tracks changes to entities to detect modifications and perform updates on `SaveChanges()`. However, for read-only scenarios, tracking unnecessary data changes consumes memory and processing power.

* **How to Optimize**: Use `AsNoTracking()` for read-only queries to bypass change tracking.

  ```csharp
  var products = dbContext.Products.AsNoTracking().ToList();
  ```

  This will increase the performance of queries that do not require any modifications to the data.

---

### ⚡ 2. **Batch Insert/Update/Delete**

EF can sometimes perform individual database operations for each entity when performing multiple insertions, updates, or deletions. This can cause performance issues, especially when working with large datasets.

* **How to Optimize**: Perform batch insertions, updates, or deletions, or use raw SQL for batch operations when possible.

  ```csharp
  dbContext.BulkInsert(entities);  // Use libraries like EFCore.BulkExtensions
  ```

  This reduces the number of database round trips and improves performance significantly.

---

### 📉 3. **Limit Data Fetching**

Loading unnecessary data can harm performance. EF queries can sometimes return more data than needed (e.g., fetching all columns of a table when only a few are required).

* **How to Optimize**: **Use projections** to select only the necessary fields instead of entire entities. This minimizes the data transferred from the database.

  ```csharp
  var products = dbContext.Products
                          .Where(p => p.Price > 100)
                          .Select(p => new { p.Name, p.Price })
                          .ToList();
  ```

  This reduces the overhead of loading large datasets and only fetches what is necessary.

---

### ⚙️ 4. **Use Compiled Queries**

EF generates SQL queries dynamically at runtime, which may result in overhead. In high-performance scenarios, especially with repeated queries, it’s beneficial to **compile the query** so that it is not generated repeatedly.

* **How to Optimize**: Use **compiled queries** to improve performance when you are executing the same query multiple times.

  ```csharp
  var query = EF.CompileQuery((MyDbContext context) => context.Products.Where(p => p.Price > 100));
  var results = query(dbContext);
  ```

  This avoids re-compiling the query every time it is executed, improving performance.

---

### ⏳ 5. **Eager Loading vs Lazy Loading**

When dealing with relationships between entities, EF supports **lazy loading**, **eager loading**, and **explicit loading**. While lazy loading fetches related data only when it is accessed, **eager loading** fetches all related data upfront, reducing the number of database queries.

* **How to Optimize**: **Use eager loading (`Include`)** when you know you need the related data upfront to avoid additional queries.

  ```csharp
  var products = dbContext.Products.Include(p => p.Category).ToList();
  ```

  Be careful not to overuse eager loading, as it can lead to excessive data retrieval, particularly when fetching large object graphs.

---

### ⚡ 6. **Optimize LINQ Queries**

LINQ queries in EF are converted to SQL, and inefficient LINQ queries can result in poorly optimized SQL queries. Inefficient queries can increase the time it takes to retrieve data.

* **How to Optimize**: Write efficient LINQ queries by:

  * Avoiding **selecting too many entities** or large result sets unnecessarily.
  * Using **`Where`** clauses early to filter data before doing anything else.

  ```csharp
  var products = dbContext.Products
                          .Where(p => p.Stock > 0)
                          .OrderBy(p => p.Name)
                          .ToList();
  ```

---

### 🚀 7. **Use Indexes in Database**

Indexes on frequently queried columns can speed up database reads and improve performance, reducing the need to scan the entire table.

* **How to Optimize**: Ensure that **indexes** are in place for frequently queried columns, such as foreign keys and search fields. Create compound indexes where necessary.

---

### 📦 8. **Limit the Size of Query Results**

Fetching large amounts of data can negatively impact performance by overwhelming memory and processing power. EF will load the entire result set into memory, which can degrade performance for large queries.

* **How to Optimize**: **Use pagination** (e.g., `Skip` and `Take`) to retrieve data in smaller chunks.

  ```csharp
  var products = dbContext.Products.Skip(0).Take(100).ToList();
  ```

  This ensures that only a small portion of the data is loaded at a time, which improves overall performance.

---

### 🔧 9. **Minimize Database Round Trips**

Each interaction with the database introduces overhead, especially if your code is making multiple database round trips. Avoid making excessive calls to the database in a loop or calling `SaveChanges()` multiple times.

* **How to Optimize**: Group operations together and call `SaveChanges()` once instead of multiple times.

  ```csharp
  dbContext.SaveChanges();
  ```

---

### ⛔ 10. **Avoid N+1 Query Problem**

The **N+1 problem** occurs when EF executes one query to retrieve a list of entities and then executes additional queries for each related entity. This can severely affect performance.

* **How to Optimize**: **Use eager loading** (with `Include`) to load related entities in a single query.

  ```csharp
  var orders = dbContext.Orders.Include(o => o.OrderItems).ToList();
  ```

---

### ✅ Answer Summary

* **Disable change tracking** for read-only queries using `AsNoTracking()` to reduce overhead.
* **Batch insert/update/delete** operations to reduce round trips to the database.
* **Limit data** fetched by using projections (`Select`) to only load necessary columns.
* **Use compiled queries** to avoid recompiling queries multiple times.
* **Eager loading** (`Include`) is useful for loading related entities in a single query, but avoid overusing it.
* **Optimize LINQ queries** to write efficient queries and reduce SQL overhead.
* Ensure proper **indexing** in the database for frequently queried fields.
* **Paginate queries** to limit data and avoid loading large datasets into memory.
* **Minimize database round trips** by grouping operations and calling `SaveChanges()` once.
* Prevent the **N+1 problem** by eager loading related entities with `Include()`.

<br>

### 44. Can you explain how to use compiled queries in EF?
### 🧠 Compiled Queries in Entity Framework

Compiled queries are a powerful feature in Entity Framework (EF) that allows you to optimize query performance, particularly when you repeatedly execute the same query. EF generates SQL queries dynamically during runtime, which can introduce overhead. Compiling a query once and reusing it eliminates the need for repeated compilation, improving efficiency.

---

### 📝 1. **What Are Compiled Queries?**

A compiled query is an EF query that is pre-compiled, which means the query is parsed, analyzed, and converted to SQL once, instead of every time the query is executed. This can significantly reduce the performance cost of executing repeated queries, especially in scenarios with frequent database access.

---

### 🚀 2. **When to Use Compiled Queries**

Use compiled queries in scenarios where:

* A specific query is executed repeatedly.
* The query structure doesn’t change.
* The performance gain from avoiding recompilation is significant.

---

### ⚙️ 3. **How to Create a Compiled Query**

Entity Framework provides a method called `EF.CompileQuery` to compile a LINQ query. You can compile a query by defining a function that represents the query and then calling `EF.CompileQuery`.

---

### 🖥️ Example of Using Compiled Queries in EF:

#### **Step-by-Step:**

1. **Define the query**:

   * First, define the LINQ query as a lambda expression.
2. **Compile the query**:

   * Use `EF.CompileQuery()` to compile the query.

```csharp
// Define the compiled query
var compiledQuery = EF.CompileQuery((MyDbContext context) => 
    context.Products.Where(p => p.Price > 100));

// Execute the compiled query
var results = compiledQuery(dbContext);
```

#### Explanation:

* The `EF.CompileQuery` method takes a lambda expression representing the query, in this case, filtering `Products` where `Price > 100`.
* The `compiledQuery` can now be executed multiple times without recompiling it each time, improving performance.

---

### ⚡ 4. **Optimizing with Compiled Queries**

For high-performance applications, especially those where a query is executed often (e.g., in loops or recurring tasks), compiling queries can reduce the overhead of repeatedly generating SQL.

#### **Example: Using Compiled Query in a Loop**

```csharp
var compiledQuery = EF.CompileQuery((MyDbContext context) =>
    context.Products.Where(p => p.Price > 100).OrderBy(p => p.Name));

for (int i = 0; i < 100; i++)
{
    var products = compiledQuery(dbContext);  // Reuses the compiled query
}
```

By compiling the query once and reusing it, you avoid the overhead of repeatedly parsing and generating SQL for the same query in every iteration.

---

### ⚠️ 5. **Limitations**

* **Query Changes**: If the structure of the query changes (e.g., adding or removing conditions), the compiled query may become invalid, and you will need to recompile it.
* **Memory Consumption**: Compiled queries are cached in memory, so keep in mind the memory impact when compiling a large number of queries.

---

### ✅ Answer Summary

* **Compiled queries** help improve performance by pre-compiling a query, avoiding the overhead of recompiling it every time.
* Use `EF.CompileQuery` to define and compile a query for reuse.
* Best suited for **repeated queries** that don’t change frequently.
* **Example**: Use compiled queries inside loops to avoid recompilation for the same query.
* **Limitations**: If the query structure changes or if too many queries are compiled, it can affect memory usage.

<br>

### 45. How do you manage the connection lifecycle for better performance in EF?
Entity Framework (EF) manages the connection to the database through the `DbContext`. Ensuring efficient management of the connection lifecycle is critical for optimizing performance, especially in applications that involve frequent database access.

---

### 📝 1. **Understanding the DbContext Lifecycle**

The `DbContext` in EF is responsible for managing the connection to the database and handling the database operations. It encapsulates the database connection and is designed to be used in a **unit-of-work** pattern, meaning it should ideally be created, used, and disposed of within a single unit of work (such as a web request or a service call).

---

### 🚀 2. **Best Practices for Managing Connection Lifecycle**

Proper connection lifecycle management helps minimize overhead, reduce connection leaks, and improve performance.

#### **a. Use DbContext in a Short-Lived Scope**

* **DbContext** should be scoped for the duration of a single operation (e.g., a web request or service call). Avoid keeping the `DbContext` open longer than necessary.
* **Use `DbContext` for a single operation** (such as querying or saving) and dispose of it afterward. This ensures that the connection is closed promptly and any resources are released.

Example:

```csharp
using (var context = new MyDbContext())
{
    var data = context.Products.Where(p => p.Price > 100).ToList();
}
```

* **Avoid long-lived `DbContext` instances** as it can lead to performance degradation due to connection pooling issues.

---

### ⚙️ 3. **Use Connection Pooling**

* EF automatically utilizes connection pooling, which can significantly improve performance when repeatedly connecting to the database. Connection pooling allows you to reuse existing database connections instead of opening a new one every time.
* Make sure to **close the connection** after use to return it to the pool. This can be done by disposing of the `DbContext`.

Example:

```csharp
using (var context = new MyDbContext())
{
    // Perform database operations
}
```

The `DbContext` will automatically close the connection after the `using` block is complete, returning it to the pool.

---

### 🚫 4. **Avoid Opening and Closing Connections Repeatedly**

* Opening and closing the connection manually can be inefficient, especially when it is done repeatedly.
* **EF automatically handles connection opening and closing**, so it’s better to let EF manage the connection lifecycle. Only open and close the connection manually if you have a specific reason.

---

### ⚡ 5. **Managing Transactions Efficiently**

* When performing multiple database operations that should be treated as a single unit, use **transactions** to ensure atomicity. This reduces the number of connections opened and closed.
* **EF manages transactions automatically** when calling `SaveChanges()`. However, for more control over the transaction, use `DbContext.Database.BeginTransaction()`.

Example:

```csharp
using (var context = new MyDbContext())
{
    using (var transaction = context.Database.BeginTransaction())
    {
        try
        {
            context.Products.Add(new Product { Name = "Product A" });
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

---

### 💡 6. **Lazy Loading and Explicit Loading with DbContext**

* **Lazy loading** can cause unnecessary connections if you aren't careful. Avoid lazy loading in scenarios where you only need specific data.
* **Explicit loading** allows you to load related entities on-demand, reducing unnecessary database queries and connection usage.

Example:

```csharp
var product = context.Products.Include(p => p.Category).FirstOrDefault();
```

---

### ✅ Answer Summary

* **DbContext Lifecycle**: Use `DbContext` in a short-lived scope, creating and disposing of it after each operation.
* **Connection Pooling**: EF automatically uses connection pooling. Always ensure you properly dispose of `DbContext` to return connections to the pool.
* **Transactions**: Use transactions to group operations into a single unit, minimizing connection overhead.
* **Avoid Repeated Connections**: Let EF manage connections automatically instead of manually opening and closing them repeatedly.
* **Lazy and Explicit Loading**: Be cautious of lazy loading, and prefer explicit loading when you only need specific data to avoid unnecessary queries and connections.

<br>

## 🔐 Entity Framework Concurrency

### 46. How does EF handle concurrency?
Concurrency control is a mechanism to handle situations where multiple users or processes try to modify the same data at the same time. EF provides several ways to manage concurrency and ensure data integrity, including optimistic concurrency and pessimistic concurrency.

---

### 📝 1. **Types of Concurrency in EF**

EF mainly supports **optimistic concurrency**. However, it also integrates with **pessimistic concurrency** when combined with database locks. The key difference lies in how conflicts are detected and resolved.

* **Optimistic Concurrency**: Assumes that conflicts are rare and allows multiple users to read and even modify the data simultaneously. Only when saving changes does EF check if any other user has modified the same data.
* **Pessimistic Concurrency**: Assumes conflicts are more frequent and locks the data when it is read, preventing other users from making changes at the same time.

---

### 🚀 2. **Optimistic Concurrency Control (OCC) in EF**

In **Optimistic Concurrency**, EF uses versioning to detect and handle concurrency conflicts. It works by keeping track of the version of the data in the database (via a timestamp or a version field).

#### **How EF Handles Optimistic Concurrency**

1. **Concurrency Tokens**: A column, often named `RowVersion` or `Timestamp`, is added to the table. This column is automatically updated whenever a row is modified.
2. **EF Tracks Changes**: When you load an entity, EF tracks the value of the concurrency token. During the `SaveChanges()` operation, EF compares the current value of the token with the one in the database.
3. **Conflict Detection**: If the token in the database has changed (i.e., another user has modified the data since it was loaded), EF throws a `DbUpdateConcurrencyException`.

---

### ⚙️ 3. **Enabling Concurrency in EF**

To implement optimistic concurrency in EF, follow these steps:

1. **Add a Concurrency Token**: Mark a column as the concurrency token in your model.

   * Use the `[Timestamp]` attribute or configure it using Fluent API.

   Example:

   ```csharp
   public class Product
   {
       public int ProductId { get; set; }
       public string Name { get; set; }
       public decimal Price { get; set; }
       
       [Timestamp]  // Concurrency token
       public byte[] RowVersion { get; set; }
   }
   ```

2. **Configure Concurrency in Fluent API** (if needed):

   ```csharp
   modelBuilder.Entity<Product>()
       .Property(p => p.RowVersion)
       .IsRowVersion();
   ```

3. **Handling Concurrency Conflicts**:

   * When `SaveChanges()` is called, EF checks for conflicts by comparing the original value of the concurrency token in the database with the current value.
   * If a conflict occurs, EF throws a `DbUpdateConcurrencyException`, which you can catch and handle.

Example:

```csharp
try
{
    dbContext.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    // Handle concurrency conflict
}
```

#### **Conflict Resolution**

* **Client-side Resolution**: You can reload the conflicting entity and apply changes.
* **Database-side Resolution**: You can overwrite the conflicting changes or merge them programmatically based on application rules.

---

### 🔐 4. **Pessimistic Concurrency Control**

Pessimistic concurrency is usually implemented at the database level with **locking**. This ensures that a row is locked for modification by a single user, preventing others from modifying it until the first user commits their changes. EF doesn’t directly manage pessimistic locking, but you can use raw SQL commands to achieve this.

Example:

```csharp
var product = dbContext.Products
    .FromSqlRaw("SELECT * FROM Products WITH (UPDLOCK) WHERE ProductId = {0}", productId)
    .FirstOrDefault();
```

* `WITH (UPDLOCK)` locks the row for updating until the transaction is complete.

---

### 🚀 5. **Handling Concurrency Exceptions**

EF throws a `DbUpdateConcurrencyException` when a conflict occurs. This exception contains a list of entries that had concurrency conflicts. You can inspect the `DbEntityEntry` objects and decide how to resolve the conflict.

#### **Steps to Handle Conflicts**:

1. **Catch the Exception**: Use a `try-catch` block around `SaveChanges()`.
2. **Resolve Conflicts**: You can:

   * Reload the data.
   * Merge changes from both the client and the database.
   * Discard changes and reload from the database.

Example:

```csharp
try
{
    dbContext.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    foreach (var entry in ex.Entries)
    {
        var currentValues = entry.CurrentValues;
        var databaseValues = entry.GetDatabaseValues();

        // Resolve conflict here (e.g., overwrite or merge)
    }
}
```

---

### ✅ Answer Summary

* **Optimistic Concurrency**: EF uses versioning (via `RowVersion` or `Timestamp`) to detect changes when saving data. If the data has been modified by another user, EF throws a `DbUpdateConcurrencyException`.
* **Pessimistic Concurrency**: Managed by database locks (e.g., `UPDLOCK`) to prevent multiple users from modifying data at the same time. EF doesn't handle this directly, but raw SQL can be used.
* **Conflict Handling**: Use `DbUpdateConcurrencyException` to catch and resolve conflicts, either by reloading data, merging changes, or discarding changes.
* **Concurrency Tokens**: Mark a column with the `[Timestamp]` attribute or Fluent API to track the version of a row for concurrency.

<br>

### 47. What is optimistic concurrency control in EF?
### 🔄 Optimistic Concurrency Control (OCC) in Entity Framework

Optimistic Concurrency Control is a strategy used by Entity Framework (EF) to handle scenarios where multiple users or processes may attempt to modify the same data at the same time, **without locking** the database rows. Instead of preventing conflicts upfront, it detects and handles them during the save operation.

---

### ✅ When and Why It’s Used

* **When**: Applied by default in EF for entities that include a concurrency token.
* **Why**: It allows high performance and better scalability since it avoids locking rows for read or update operations.

---

### 🔐 How EF Implements Optimistic Concurrency

EF checks whether the data in the database has changed since it was last read. This is achieved using **concurrency tokens**, such as a `RowVersion` or `Timestamp` field.

#### Steps:

1. **Entity is Read**: EF fetches the entity from the database and stores original values, including the concurrency token.
2. **Entity is Modified**: The application modifies the entity.
3. **SaveChanges is Called**: EF generates an `UPDATE` statement that includes a `WHERE` clause checking the original value of the concurrency token.
4. **Conflict Detection**:

   * If **no rows** are affected (because the token doesn’t match), EF knows someone else has modified the data.
   * EF then throws a `DbUpdateConcurrencyException`.

---

### 🛠️ Configuring OCC in EF

#### Using Attributes:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }

    [Timestamp] // Concurrency token
    public byte[] RowVersion { get; set; }
}
```

#### Using Fluent API:

```csharp
modelBuilder.Entity<Product>()
    .Property(p => p.RowVersion)
    .IsRowVersion();
```

---

### 🔁 Handling Concurrency Exceptions

EF throws `DbUpdateConcurrencyException` when it detects a conflict.

#### Example:

```csharp
try
{
    dbContext.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    foreach (var entry in ex.Entries)
    {
        var currentValues = entry.CurrentValues;
        var dbValues = entry.GetDatabaseValues();

        // You can choose to overwrite or merge
        entry.OriginalValues.SetValues(dbValues);
    }
}
```

Options:

* **Overwrite with current user’s data**
* **Keep database values**
* **Merge both values manually**

---

### 🧪 Use Case Example

Imagine two users load the same product record:

* User A changes the price to 100.
* User B changes the price to 200.
* User A saves first – update succeeds.
* User B tries to save – EF checks `RowVersion`, sees it's outdated, and throws an exception.

---

### 📌 Answer Summary

* **Optimistic Concurrency** allows multiple users to edit the same data without locks and checks for conflicts only when saving.
* **EF tracks original values** using a concurrency token (e.g., `RowVersion` or `Timestamp`).
* If data has changed since it was fetched, EF throws a `DbUpdateConcurrencyException`.
* You handle conflicts manually by reloading, overwriting, or merging data.
* Optimistic concurrency is best suited for high-read, low-write scenarios.

<br>

### 48. Explain how EF handles concurrency conflicts.
### 🔄 How Entity Framework Handles Concurrency Conflicts

Entity Framework (EF) handles concurrency conflicts using **optimistic concurrency control**. This means EF assumes that concurrency conflicts are rare and checks for them only when trying to **persist changes** to the database.

---

### 🧠 What Is a Concurrency Conflict?

A concurrency conflict occurs when **two or more users retrieve the same data**, make changes to it independently, and then try to **save their changes** back to the database. EF must determine whether the data was changed by someone else in the meantime.

---

### 🛡️ Mechanism of Conflict Detection

EF uses **concurrency tokens**, such as a `Timestamp` or `RowVersion` column in the table, to track changes.

#### Steps:

1. **Entity is Read**: EF fetches data and tracks original values.
2. **Entity is Modified**: Application modifies entity in memory.
3. **SaveChanges is Called**: EF sends an `UPDATE` or `DELETE` SQL query with a `WHERE` clause that includes the original value of the concurrency token.
4. **Conflict Check**:

   * If **zero rows** are affected, it means the data has been changed or deleted by someone else.
   * EF throws a `DbUpdateConcurrencyException`.

---

### 🧯 Handling Concurrency Conflicts in Code

You can catch and resolve conflicts by retrying or merging changes.

#### Example:

```csharp
try
{
    dbContext.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    foreach (var entry in ex.Entries)
    {
        var dbValues = entry.GetDatabaseValues();
        var clientValues = entry.CurrentValues;

        // Strategy: Overwrite database with client values
        entry.OriginalValues.SetValues(dbValues);
    }

    // Retry save
    dbContext.SaveChanges();
}
```

---

### 🧩 Strategies to Resolve Conflicts

1. **Client Wins**: Overwrite the database with current user’s changes.
2. **Store Wins**: Discard changes and reload values from the database.
3. **Merge**: Manually combine values from both sources.

---

### 🔧 Configuring Concurrency Tokens

#### Using `[Timestamp]` Attribute:

```csharp
public class Order
{
    public int Id { get; set; }

    [Timestamp]
    public byte[] RowVersion { get; set; }
}
```

#### Using Fluent API:

```csharp
modelBuilder.Entity<Order>()
    .Property(o => o.RowVersion)
    .IsRowVersion();
```

---

### 🧪 Real-World Scenario

* Two admins update the same product price.
* Admin A changes it to \$50 and saves — success.
* Admin B changes it to \$60 and tries to save — EF detects mismatch in `RowVersion` and throws a conflict exception.

---

### 📌 Answer Summary

* **EF uses optimistic concurrency** with tokens like `RowVersion` to detect changes during `SaveChanges`.
* **If another change occurred**, EF throws `DbUpdateConcurrencyException`.
* **Conflicts can be resolved** by retrying, merging, or overwriting.
* **Concurrency tokens** must be configured to enable conflict detection.
* Ideal for high-read, low-write applications where locking is too restrictive.

<br>

### 49. How do you use timestamps/row versions for concurrency in EF’s code-first approach?
### 🕒 Using Timestamps/RowVersions for Concurrency in EF Code-First

In Entity Framework’s **Code-First** approach, you can use a special column (usually a `byte[]`) as a **concurrency token** to detect concurrency conflicts. This is commonly referred to as a **RowVersion** or **Timestamp** column.

This token is automatically checked during `SaveChanges()`, and EF ensures that no one else has modified the data since it was last read.

---

### 🛠️ Step 1: Define a RowVersion Property in Your Entity

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }

    [Timestamp]
    public byte[] RowVersion { get; set; }  // Acts as concurrency token
}
```

* The `[Timestamp]` attribute tells EF that this column should be used for concurrency checking.
* EF automatically sets the `RowVersion` column to be auto-generated and binary.
* The type **must be `byte[]`**, and it will be mapped to `rowversion` or `timestamp` in SQL Server.

---

### 🧰 Step 2: Migrations – Add RowVersion Column to the DB

If you're using EF Migrations:

```bash
Add-Migration AddRowVersionToProduct
Update-Database
```

EF will generate SQL similar to:

```sql
ALTER TABLE Products ADD RowVersion rowversion NOT NULL;
```

---

### 🔁 Step 3: Concurrency Handling During SaveChanges

When `SaveChanges()` is called, EF generates a `WHERE` clause that includes the original value of the `RowVersion`. If no rows are affected (meaning the row was modified since it was read), EF throws a `DbUpdateConcurrencyException`.

```csharp
try
{
    dbContext.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    var entry = ex.Entries.First();
    var dbValues = entry.GetDatabaseValues();
    var clientValues = entry.CurrentValues;

    // Example strategy: Refresh values and retry
    entry.OriginalValues.SetValues(dbValues);
    dbContext.SaveChanges();
}
```

---

### ✅ Key Benefits

* **Automatic conflict detection** without manual checks.
* **Non-blocking**: doesn’t require row locks.
* Works well with **scaling and high concurrency scenarios**.

---

### 📝 Notes

* Works only if the underlying database supports `rowversion` or `timestamp` (SQL Server does).
* EF will **automatically update** the value of `RowVersion` after every successful save.
* You **must not set this value manually** in the application code.

---

### 📌 Answer Summary

* Use a `byte[]` property with the `[Timestamp]` attribute to enable row versioning in EF Code-First.
* EF uses this property for **optimistic concurrency control**.
* During `SaveChanges()`, EF compares original vs. database `RowVersion` values.
* If there’s a mismatch, EF throws `DbUpdateConcurrencyException`.
* Handle conflicts by refreshing, retrying, or merging data.
* Automatically supported in SQL Server via `rowversion` or `timestamp` column.

<br>

### 50. What are the types of concurrency patterns that can be implemented with EF?
### 🧵 Types of Concurrency Patterns in Entity Framework

Entity Framework supports various concurrency patterns to manage **conflicts that occur when multiple users access and modify the same data simultaneously**. These patterns help maintain data consistency without locking resources unnecessarily.

---

### 1️⃣ Optimistic Concurrency (Default in EF)

EF assumes that **conflicts are rare**, and allows multiple users to read the same data. It only checks for conflicts **when changes are saved**.

#### 🔧 How it works:

* Uses **concurrency tokens** like `RowVersion` or `Timestamp`.
* On `SaveChanges()`, EF checks if the original value in the token matches the database value.
* If mismatch: throws `DbUpdateConcurrencyException`.

#### ✅ Benefits:

* High performance, no locks.
* Ideal for web applications or systems with mostly read operations.

---

### 2️⃣ Pessimistic Concurrency (Manual Implementation)

This approach **locks the data at the database level** as soon as it’s read, preventing others from modifying it until the lock is released.

#### 🔧 How to use:

* Not natively supported in EF, but can be achieved using **raw SQL** or stored procedures with `SELECT ... FOR UPDATE` (if supported by DB).
* Usually involves **transaction and isolation level** settings.

#### ✅ Benefits:

* Prevents any modification conflict.
* Suitable for high-conflict, mission-critical systems.

#### ❌ Drawbacks:

* Reduced performance.
* Can cause deadlocks or long wait times.

---

### 3️⃣ Last Wins Strategy (Overwriting Conflicts)

This is a simplified approach where **no concurrency checking is done**, and the **last user to save wins**, overwriting any previous changes.

#### 🔧 How to use:

* Don’t configure concurrency tokens.
* Disable tracking or version checks.

#### ✅ Benefits:

* Simple implementation.
* No conflict resolution logic needed.

#### ❌ Drawbacks:

* Risk of silent data loss.
* Not recommended for critical data updates.

---

### 4️⃣ First Wins Strategy (Manual Conflict Resolution)

Also called **client-preferred strategy**, where EF checks for conflicts, and if one is found, the **first saved change is kept**, and others are discarded or manually resolved.

#### 🔧 How to use:

* Catch `DbUpdateConcurrencyException`.
* Show current values to the user.
* Let user choose whether to overwrite, merge, or discard.

#### ✅ Benefits:

* User-driven resolution.
* Keeps data intentional and accurate.

---

### 🧠 Choosing the Right Strategy

| Pattern             | Best For                                      |
| ------------------- | --------------------------------------------- |
| Optimistic          | Most web apps and high-read systems           |
| Pessimistic         | High-conflict systems, banking, inventory     |
| Last Wins           | Simple logs or non-critical data              |
| First Wins (manual) | Collaborative editing, business-critical data |

---

### 📌 Answer Summary

* **Optimistic Concurrency**: Default in EF, uses tokens to detect conflicts at save time.
* **Pessimistic Concurrency**: Manually implemented using locks, prevents others from editing data.
* **Last Wins**: Simplest form, last saved data wins, risk of data loss.
* **First Wins / Manual Merge**: Conflicts resolved by showing the user and choosing what to save.
* EF is optimized for **optimistic concurrency**, but other patterns can be applied based on use case.

<br>

## 🧠 Entity Framework Advanced Features

### 51. Can you explain T4 templates in the context of EF?
### 🧰 T4 Templates in Entity Framework

T4 (Text Template Transformation Toolkit) templates are code generation tools used by **Entity Framework Designer** to automatically generate classes based on the EDMX (Entity Data Model) file. They generate boilerplate code for entities and the `DbContext` based on your database schema or model.

EF uses T4 templates primarily in the **Database-First** and **Model-First** workflows.

---

### 📄 What is a T4 Template?

* A `.tt` file (Text Template).
* Contains a mix of **plain text, C# code, and control logic**.
* Generates `.cs` files at design time based on the template logic.

---

### 🏗️ How EF Uses T4 Templates

When you create an EF model from the database (`.edmx` file), Visual Studio generates:

* `Model.tt` → generates entity classes for each table.
* `Model.Context.tt` → generates the `DbContext` class.

These templates read the `.edmx` metadata and emit C# classes accordingly.

---

### 🛠️ Customizing T4 Templates

You can **customize the generated code** by editing the `.tt` files. Common customizations:

* Add base class inheritance.
* Implement interfaces like `INotifyPropertyChanged`.
* Add data annotations or logging.
* Rename class or property naming logic.

#### Example:

Modify `Model.tt` to make all entities implement `IAuditable` or append a suffix like `Entity` to each class name.

---

### 🔄 Template Re-generation

T4 templates are run automatically whenever the EDMX model is saved. This regenerates the output `.cs` files.

You can also **manually trigger** transformation by right-clicking on the `.tt` file in Solution Explorer and selecting **Run Custom Tool**.

---

### 🚫 T4 in Code-First

T4 templates are **not used in Code-First** approach. In Code-First, you manually write your model classes and configurations. Code generation is not handled by T4.

---

### 🔍 File Structure Example

```
Model.edmx
|__ Model.tt               -> Generates POCO entity classes
|__ Model.Context.tt       -> Generates DbContext class
|__ Model.Designer.cs      -> Legacy EF class file (older EF versions)
```

---

### 📌 Answer Summary

* **T4 Templates** are code generators used in EF’s Database-First and Model-First approaches.
* They generate **entity classes** and **DbContext** based on the `.edmx` model.
* Two main templates: `Model.tt` (entities) and `Model.Context.tt` (context).
* Fully **customizable** to add logic like base classes, annotations, or interfaces.
* **Automatically regenerate** when the EDMX is saved or manually run.
* **Not used in Code-First** workflow—there, you write classes manually.

<br>

### 52. What are interceptors in EF and when would you use them?
### 🕵️ Interceptors in Entity Framework

Interceptors in Entity Framework are powerful tools that allow you to **intercept, monitor, and modify EF operations** such as command execution, connection handling, and context lifecycle events. They act like **middleware** between EF and the database, giving you a way to inject custom logic.

They are available from **Entity Framework 6 onward** (in EF Core, similar functionality exists using diagnostics and logging).

---

### 🛠️ What Can Interceptors Do?

Interceptors implement specific interfaces that hook into EF's internal processes. Common interceptors include:

#### 1️⃣ **IDbCommandInterceptor**

Intercepts **raw SQL commands** before they’re sent to the database.

Use cases:

* Logging SQL queries.
* Modifying queries on the fly.
* Measuring performance.

#### 2️⃣ **IDbConnectionInterceptor**

Intercepts **connection open/close events**.

Use cases:

* Connection profiling.
* Custom retry logic or monitoring.

#### 3️⃣ **IDbCommandTreeInterceptor** (EF6 only)

Intercepts the **query command tree** before SQL is generated.

Use cases:

* Query rewriting at a high level.
* Filtering or multitenancy logic.

#### 4️⃣ **IDbTransactionInterceptor**

Intercepts **transactions** started or committed by EF.

Use cases:

* Audit logging for transactions.
* Custom handling or validation.

---

### 🔧 How to Implement an Interceptor

1. Create a class that implements one of the interceptor interfaces:

```csharp
public class LoggingInterceptor : IDbCommandInterceptor
{
    public void ReaderExecuting(DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        Console.WriteLine("SQL Executed: " + command.CommandText);
    }

    // Other methods can be left empty or implemented
}
```

2. Register it at application startup:

```csharp
DbInterception.Add(new LoggingInterceptor());
```

---

### 📦 When Would You Use Interceptors?

* **SQL Logging**: Capture queries and parameters for diagnostics or performance tuning.
* **Audit Trails**: Log who accessed what and when.
* **Query Modification**: Inject filters like `WHERE IsDeleted = 0` automatically.
* **Multitenancy**: Modify queries based on tenant context.
* **Performance Monitoring**: Measure execution times and detect bottlenecks.
* **Security Enforcement**: Block unwanted queries or log sensitive access.

---

### ❗ Caution

* Overusing interceptors can affect performance and maintainability.
* They run **for all queries**, so make them lightweight and efficient.
* Prefer **DbContext-level configuration** or services like logging when possible before resorting to interceptors.

---

### 📌 Answer Summary

* **Interceptors** let you hook into EF’s internal operations like SQL command execution, connection management, and transactions.
* Implement interfaces like `IDbCommandInterceptor`, `IDbConnectionInterceptor`, etc.
* Use them for **logging, auditing, filtering, multitenancy**, and **performance analysis**.
* Registered using `DbInterception.Add(...)`.
* Powerful but should be used judiciously to avoid performance or complexity issues.

<br>

### 53. How do you implement inheritance in EF models?
### 🧬 Implementing Inheritance in Entity Framework Models

Entity Framework supports inheritance in your domain models using three main strategies. These allow your C# class hierarchies to be mapped to relational database tables.

---

### 🧱 Inheritance Strategies in EF

#### 1️⃣ Table per Hierarchy (TPH)

* **Default strategy in EF**.
* All classes in the inheritance tree are mapped to a **single table**.
* A **discriminator column** identifies the entity type.

Example:

```csharp
public class Person { public int Id; public string Name; }
public class Student : Person { public string Grade; }
public class Teacher : Person { public string Subject; }
```

All data is stored in one `People` table:

```
Id | Name | Grade | Subject | Discriminator
--------------------------------------------
1  | Alex | A     | NULL    | Student
2  | John | NULL  | Math    | Teacher
```

**Pros**: Fast queries, simple schema
**Cons**: Lots of nulls in irrelevant columns

---

#### 2️⃣ Table per Type (TPT)

* Each class in the hierarchy maps to a **separate table**.
* The child table has a **foreign key to the base table**.

Tables:

* `Persons (Id, Name)`
* `Students (Id, Grade)` → FK to `Persons`
* `Teachers (Id, Subject)` → FK to `Persons`

EF Core 5+ supports TPT natively.

**Pros**: Cleaner tables, normalized
**Cons**: Slower performance due to joins

---

#### 3️⃣ Table per Concrete Class (TPC)

* Each non-abstract class gets its **own table**.
* No base table is created; shared columns are duplicated.

Tables:

* `Students (Id, Name, Grade)`
* `Teachers (Id, Name, Subject)`

**Pros**: Simple queries, no joins
**Cons**: Data duplication, hard to enforce consistency

EF Core 7+ supports TPC with `[Keyless]` entity configuration.

---

### 🧭 How to Configure Inheritance in Code-First

#### Table per Hierarchy (TPH – default)

No special configuration required. EF will use TPH by default.

#### Table per Type (TPT)

Use `ToTable` in `OnModelCreating`:

```csharp
modelBuilder.Entity<Person>().ToTable("Persons");
modelBuilder.Entity<Student>().ToTable("Students");
modelBuilder.Entity<Teacher>().ToTable("Teachers");
```

#### Table per Concrete Class (TPC – EF Core 7+)

Configure in `OnModelCreating`:

```csharp
modelBuilder.Entity<Student>().ToTable("Students").UseTpcMappingStrategy();
modelBuilder.Entity<Teacher>().ToTable("Teachers").UseTpcMappingStrategy();
```

---

### 📌 Answer Summary

* EF supports **inheritance** via TPH, TPT, and TPC strategies.
* **TPH (default)**: Single table + discriminator column; simple but may store nulls.
* **TPT**: Separate table per class; normalized but slower due to joins.
* **TPC**: Each class has its own table; no joins but duplicates shared fields.
* Use `OnModelCreating` with `ToTable` or `UseTpcMappingStrategy()` to configure.
* Choose the strategy based on **performance, schema clarity**, and **normalization needs**.

<br>

### 54. What is Code-First Data Annotations and how do they work?
### 🏷️ Code-First Data Annotations in Entity Framework

**Code-First Data Annotations** are attributes in .NET that you apply to your model classes to configure the database schema. They help Entity Framework understand how to map your classes and properties to database tables and columns — **without writing fluent API code**.

These annotations live in the `System.ComponentModel.DataAnnotations` and `System.ComponentModel.DataAnnotations.Schema` namespaces.

---

### 🧩 Commonly Used Data Annotations

#### 🔑 \[Key]

* Marks a property as the **primary key**.

```csharp
[Key]
public int EmployeeId { get; set; }
```

#### 🔁 \[DatabaseGenerated]

* Controls how values are generated:

  * `None`: Manually assigned
  * `Identity`: Auto-increment
  * `Computed`: Computed by the DB

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public int Id { get; set; }
```

#### 📏 \[MaxLength] / \[MinLength] / \[StringLength]

* Sets length limits on strings.

```csharp
[StringLength(50)]
public string Name { get; set; }
```

#### 📇 \[Column]

* Maps a property to a specific **column name and type**.

```csharp
[Column("emp_name", TypeName = "nvarchar(100)")]
public string Name { get; set; }
```

#### 🧩 \[ForeignKey]

* Defines a **foreign key** relationship.

```csharp
public int DepartmentId { get; set; }

[ForeignKey("DepartmentId")]
public Department Department { get; set; }
```

#### 🚫 \[NotMapped]

* Excludes a property from the database.

```csharp
[NotMapped]
public string TempValue { get; set; }
```

#### ⚠️ \[Required]

* Makes a column **NOT NULL**.

```csharp
[Required]
public string Email { get; set; }
```

---

### ⚙️ How They Work Behind the Scenes

* When EF builds the **model during runtime**, it uses reflection to read these attributes.
* The attributes **override EF conventions**, ensuring your model aligns with schema rules.
* You can still **combine data annotations with Fluent API** if you need more advanced configuration.

---

### ✅ When to Use

* Simple schema configurations.
* When you want to keep model and mapping in one place.
* For quick development and small-to-medium projects.

---

### ❌ When Not to Use

* Complex mappings (like composite keys, many-to-many with extra fields).
* When separation of concerns is needed (prefer Fluent API for better flexibility).

---

### 📌 Answer Summary

* **Data Annotations** are attributes that let you configure EF mappings directly in your model classes.
* Common ones: `[Key]`, `[Required]`, `[Column]`, `[StringLength]`, `[ForeignKey]`, `[NotMapped]`.
* Help customize primary keys, constraints, column names, relationships, and more.
* Useful for **simple and quick configurations**, but limited for complex scenarios.
* Internally read by EF during model creation to shape the database schema.

<br>

### 55. Can EF interact with view-models or must it always use database models?
### 👁️ EF and View Models: Separation of Concerns

Entity Framework does **not require** you to use only your database entity models for every operation. You can and **should** use **view models** (or DTOs – Data Transfer Objects) when interacting with UI or APIs.

EF operates on entity models **for database operations**, but the **result of queries** can be **projected into view models** using **LINQ**.

---

### 🔄 Why Use View Models with EF?

* Keeps database schema **decoupled** from presentation logic.
* Reduces **over-fetching** of unnecessary data.
* Prevents exposing **sensitive fields** (like passwords, IDs, etc.) to the UI.
* Supports **custom shapes** or **aggregated data** not directly present in entities.

---

### 🧪 How to Use View Models with EF

#### 1️⃣ Projecting with LINQ

Use `Select()` to map entities to a view model.

```csharp
var users = context.Users
    .Select(u => new UserViewModel {
        FullName = u.FirstName + " " + u.LastName,
        Email = u.Email
    }).ToList();
```

#### 2️⃣ Using AutoMapper

AutoMapper can help map between entities and view models automatically:

```csharp
var users = context.Users.ProjectTo<UserViewModel>(_mapper.ConfigurationProvider).ToList();
```

#### 3️⃣ Mapping Manually After Fetch

Sometimes you fetch entities and then convert them:

```csharp
var userEntity = context.Users.FirstOrDefault();
var userVM = new UserViewModel {
    Name = userEntity.Name,
    Email = userEntity.Email
};
```

---

### 🚫 What You Should Avoid

* **Binding EF entities directly to views** or APIs – leads to tight coupling and security risks.
* **Updating view models and trying to save them directly** – EF doesn't track view models, so you must **map back** to entity models.

---

### ✅ Best Practices

* Use **entities** for data access.
* Use **view models** for display, forms, or APIs.
* Use **AutoMapper or manual mapping** to convert between them.
* Do **not expose EF entities** directly to UI or external systems.

---

### 📌 Answer Summary

* EF can **work with view models** via **projection** using LINQ or AutoMapper.
* It only tracks **entity models** for database operations.
* View models improve **security, performance, and separation of concerns**.
* Avoid binding EF entities directly to UI or API responses.
* Always **map from entities to view models** and vice versa as needed.

<br>

## 🔗 Entity Framework and .NET Integration

### 56. How do you handle transactions across different data contexts in EF?
### 🔄 Transactions Across Multiple DbContexts in EF

Entity Framework supports transaction handling, but managing **transactions across multiple `DbContext` instances** requires **manual coordination**. EF Core doesn’t manage cross-context transactions automatically — you need to use **shared connections** or the **`TransactionScope`** class.

---

### ✅ Option 1: Using `TransactionScope` (for EF6 and EF Core with limitations)

* Use `System.Transactions.TransactionScope` to wrap multiple contexts.
* Ensures that all operations are committed or rolled back **atomically**.

```csharp
using (var scope = new TransactionScope())
{
    using (var context1 = new FirstDbContext())
    {
        context1.Add(new EntityA { Name = "Test A" });
        context1.SaveChanges();
    }

    using (var context2 = new SecondDbContext())
    {
        context2.Add(new EntityB { Name = "Test B" });
        context2.SaveChanges();
    }

    scope.Complete(); // Commit
}
```

> ⚠️ `TransactionScope` may trigger **Distributed Transactions (MSDTC)** if connections span multiple databases or servers.

---

### 🔁 Option 2: Sharing a Database Connection

* Share the same `DbConnection` between different contexts to use the **same transaction**.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    connection.Open();
    using (var transaction = connection.BeginTransaction())
    {
        var context1 = new FirstDbContext(new DbContextOptionsBuilder<FirstDbContext>()
            .UseSqlServer(connection)
            .Options);

        var context2 = new SecondDbContext(new DbContextOptionsBuilder<SecondDbContext>()
            .UseSqlServer(connection)
            .Options);

        context1.Database.UseTransaction(transaction);
        context2.Database.UseTransaction(transaction);

        context1.SaveChanges();
        context2.SaveChanges();

        transaction.Commit();
    }
}
```

> ✅ Works best when **both contexts use the same database**.

---

### 💡 Best Practices

* Use a **unit of work pattern** to abstract multiple contexts behind a service layer.
* Prefer a **single `DbContext`** where possible to avoid complexity.
* Use **shared `DbConnection`** for better control if within the same database.
* Use `TransactionScope` cautiously to avoid unintended **distributed transaction escalation**.

---

### 📌 Answer Summary

* EF **doesn't manage cross-context transactions** automatically.
* Use `TransactionScope` for **simple atomic operations** across multiple contexts.
* Share `DbConnection` and `DbTransaction` manually if using **same database**.
* Be aware of **distributed transaction** escalation in `TransactionScope`.
* Always commit or rollback explicitly to avoid partial data writes.

<br>

### 57. Can you unit test EF code?
### ✅ Yes, You Can Unit Test EF Code

Entity Framework code can be unit tested effectively, **but with careful design**. Since EF interacts with a database, you should **mock dependencies** like `DbContext` and `DbSet` or **abstract EF behind interfaces** to keep your tests fast and independent of the actual database.

---

### 🧪 Common Testing Strategies

#### 1️⃣ Use In-Memory Database (EF Core)

EF Core supports an **in-memory provider** that mimics database behavior without real I/O.

```csharp
var options = new DbContextOptionsBuilder<MyDbContext>()
    .UseInMemoryDatabase(databaseName: "TestDB")
    .Options;

using (var context = new MyDbContext(options))
{
    // Seed and test logic here
}
```

* ✅ Fast and real EF behavior
* ❌ Doesn't cover all SQL-specific behaviors (e.g., constraints, transactions)

---

#### 2️⃣ Mock `DbSet<T>` with Moq (for EF6)

You can mock `DbSet<T>` using libraries like **Moq** to test logic without EF internals.

```csharp
var mockSet = new Mock<DbSet<User>>();
var mockContext = new Mock<MyDbContext>();

mockContext.Setup(c => c.Users).Returns(mockSet.Object);

// Inject mockContext into your service
```

* ✅ Great for **logic-only** testing
* ❌ Tedious setup and doesn't behave like real database queries

---

#### 3️⃣ Repository Pattern with Interface Abstraction

Encapsulate EF logic behind interfaces so that the database layer can be mocked or replaced in tests.

```csharp
public interface IUserRepository {
    Task<User> GetByIdAsync(int id);
}

public class UserRepository : IUserRepository {
    private readonly MyDbContext _context;
    public UserRepository(MyDbContext context) => _context = context;

    public Task<User> GetByIdAsync(int id) => _context.Users.FindAsync(id).AsTask();
}
```

* ✅ Makes testing easier and more maintainable
* ✅ Supports mocking repositories without EF at all

---

### 🔁 Integration Testing (Optional but Useful)

When real database interaction is required, use **test-specific databases** or **SQLite in-memory** for integration testing.

```csharp
.UseSqlite("Filename=:memory:")
```

* ✅ Useful for full coverage (migrations, transactions, etc.)
* ❌ Slower than pure unit tests

---

### 🧠 Best Practices

* Abstract EF behind services or repositories.
* Use **EF Core In-Memory provider** for most tests.
* Avoid logic in `DbContext` — keep it in services for testability.
* Write **unit tests for logic** and **integration tests for EF behavior**.

---

### 📌 Answer Summary

* EF code **can be unit tested** using mocking or in-memory databases.
* Use **EF Core’s In-Memory provider** for quick and realistic tests.
* **Mock `DbSet` or use interfaces** to isolate EF dependencies.
* Apply **repository pattern** for easier testing and better architecture.
* Combine unit and integration tests for full confidence.

<br>

### 58. How does EF fit into the .NET Core ecosystem?
### 📦 EF in the .NET Core Ecosystem

Entity Framework fits naturally into the **.NET Core ecosystem** as the official **ORM (Object-Relational Mapper)**. The modern version is called **Entity Framework Core (EF Core)**, which was **rebuilt from scratch** to align with the **modular, cross-platform, and lightweight** goals of .NET Core.

---

### ⚙️ Dependency Injection (DI) Integration

* EF Core integrates seamlessly with .NET Core's **built-in dependency injection system**.
* You typically register the `DbContext` in `Startup.cs` or `Program.cs`:

```csharp
services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```

* You can then inject `AppDbContext` into controllers, services, or repositories.

---

### 🔌 Cross-Platform and Lightweight

* EF Core is **cross-platform** (Windows, Linux, macOS) just like .NET Core.
* You can use EF Core in **console apps, web APIs, Blazor apps, Azure Functions**, and more.
* Designed for **performance**, **low memory footprint**, and **flexibility**.

---

### 📂 Supports .NET Core Project Structure

* EF Core works smoothly with **ASP.NET Core MVC** or **Minimal APIs**.
* Uses .NET Core’s **configuration**, **logging**, and **hosting** model.
* EF tools like `dotnet ef` integrate into the .NET CLI.

---

### 🧪 Testing and Environment Management

* Supports **in-memory providers** and **SQLite** for lightweight testing.
* Environment-specific configurations (`appsettings.Development.json`, etc.) support dynamic connection strings and behavior.

---

### 🌐 Migration and Tooling Support

* Use CLI tools like:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* These commands integrate easily into .NET Core's command-line tooling.

---

### 🔄 Async and LINQ Support

* Fully supports **async/await** patterns, which is aligned with .NET Core’s async-first design.
* Compatible with LINQ queries and modern C# features.

---

### 🧱 Modular and Extensible

* Modular NuGet packages:

  * `Microsoft.EntityFrameworkCore`
  * `Microsoft.EntityFrameworkCore.SqlServer`
  * `Microsoft.EntityFrameworkCore.Tools`
* Swap providers easily (SQL Server, SQLite, PostgreSQL, In-Memory, etc.)

---

### 📌 Answer Summary

* EF Core is the **official ORM for .NET Core**, built for modern, cross-platform development.
* Integrates with **dependency injection**, **configuration**, and **logging** systems.
* Supports **async programming**, **LINQ**, and **modular packages**.
* Works with **dotnet CLI** tools and supports **code-first migrations**.
* Used in **web APIs**, **Blazor**, **desktop apps**, and **microservices**.
* Enables **clean architecture**, **testability**, and **high performance** in .NET Core apps.

<br>

### 59. What is dependency injection and how do you use it with EF?
### 🔄 What Is Dependency Injection?

**Dependency Injection (DI)** is a design pattern used to **provide dependencies** (like services, repositories, or contexts) to a class instead of creating them manually inside the class.

It promotes:

* **Loose coupling**
* **Testability**
* **Flexibility**
* **Code reusability**

In .NET Core, DI is built-in and configured via the `IServiceCollection` in `Program.cs` or `Startup.cs`.

---

### 🧩 EF + Dependency Injection in .NET Core

Entity Framework Core integrates seamlessly with DI. You register your `DbContext` once, and then inject it wherever you need it.

#### ✅ Registering `DbContext`

```csharp
services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```

* This tells .NET Core to **create and manage** `AppDbContext` as a **scoped service**.

#### ✅ Injecting `DbContext` into a Service or Controller

```csharp
public class ProductService
{
    private readonly AppDbContext _context;

    public ProductService(AppDbContext context)
    {
        _context = context;
    }

    public List<Product> GetProducts()
    {
        return _context.Products.ToList();
    }
}
```

* You don’t `new` up the context manually.
* DI **automatically provides the instance** based on the lifetime configuration.

---

### 📦 Lifetime Scopes in DI (Important for EF)

* `Scoped` (recommended for EF): A single instance per request (default for `DbContext`).
* `Transient`: New instance every time it's requested (not suitable for `DbContext`).
* `Singleton`: One instance for the entire application (never use for `DbContext`).

```csharp
services.AddScoped<AppDbContext>();
```

---

### 🧪 Testing with DI

* You can inject **mocked or in-memory DbContext** into services when testing.
* This makes unit testing easier and avoids hard dependencies on databases.

---

### 🧠 Common Mistakes to Avoid

* Don’t register `DbContext` as **singleton**.
* Don’t manually instantiate `DbContext` with `new` keyword — breaks DI benefits.
* Keep your data access code **in services or repositories**, not in controllers directly.

---

### 📌 Answer Summary

* **Dependency Injection (DI)** provides objects (like `DbContext`) instead of hard-coding them.
* In .NET Core, you register EF’s `DbContext` in `IServiceCollection`.
* EF’s `DbContext` is injected into services, controllers, or repositories automatically.
* EF `DbContext` should be registered as **scoped**.
* DI helps make EF code **loosely coupled**, **testable**, and **easier to maintain**.

<br>

### 60. How does EF work with asynchronous programming in .NET?
### ⚙️ Asynchronous Programming in EF

Entity Framework Core fully supports **asynchronous programming** using the `async` and `await` keywords. This is crucial for improving **scalability** and **responsiveness**, especially in **web applications** where thread management is vital.

EF Core provides **async counterparts** for most query and save operations to avoid blocking threads during database I/O.

---

### 🔄 Why Use Async with EF?

* **Non-blocking I/O**: Database operations are I/O-bound and can take time. Async calls free up threads to handle other requests.
* **Improved scalability**: In ASP.NET Core apps, async methods help handle more concurrent requests.
* **Responsive UI**: In desktop/mobile apps, async prevents the UI from freezing during long DB operations.

---

### 🧪 Common Async Methods in EF Core

| Sync Method         | Async Equivalent         |
| ------------------- | ------------------------ |
| `ToList()`          | `ToListAsync()`          |
| `FirstOrDefault()`  | `FirstOrDefaultAsync()`  |
| `SingleOrDefault()` | `SingleOrDefaultAsync()` |
| `Any()`             | `AnyAsync()`             |
| `Count()`           | `CountAsync()`           |
| `SaveChanges()`     | `SaveChangesAsync()`     |
| `Find()`            | `FindAsync()`            |

---

### 📘 Example: Async Query and Save

```csharp
public async Task<List<Product>> GetProductsAsync()
{
    return await _context.Products.ToListAsync();
}

public async Task AddProductAsync(Product product)
{
    _context.Products.Add(product);
    await _context.SaveChangesAsync();
}
```

* `await` pauses execution until the DB call finishes, without blocking the current thread.
* `async Task<T>` methods return a task that completes when the async operation finishes.

---

### 🧠 Best Practices

* **Use async all the way**: From data access to controller. Avoid mixing sync/async.
* Avoid `.Result` or `.Wait()` on async methods — causes deadlocks.
* Use `ConfigureAwait(false)` in libraries (not typically in ASP.NET Core).
* Don't overuse async — use only for I/O-bound DB calls, not for simple in-memory operations.

---

### 📌 Answer Summary

* EF Core supports **async methods** like `ToListAsync()`, `FirstOrDefaultAsync()`, and `SaveChangesAsync()`.
* Async prevents **thread blocking**, improves **scalability**, and enhances **performance**.
* Use `async/await` for all DB operations in web and desktop apps.
* Always prefer `async` methods in EF for long-running or high-load applications.

<br>

## 🧰 Entity Framework Troubleshooting

### 61. How do you troubleshoot performance issues in EF?
### 📊 Identifying Performance Issues in Entity Framework

Troubleshooting performance in EF involves detecting slow queries, inefficient data access patterns, and unnecessary operations that can cause lags or bottlenecks in your application. EF Core provides several tools and techniques to profile and optimize query performance.

---

### 🔍 Use Logging to See Generated SQL

EF Core allows you to **log the SQL queries** being generated and sent to the database.

```csharp
optionsBuilder
    .UseSqlServer(connectionString)
    .LogTo(Console.WriteLine, LogLevel.Information)
    .EnableSensitiveDataLogging();
```

* Helps identify N+1 queries, missing WHERE clauses, or unexpected joins.
* Use `EnableSensitiveDataLogging()` **only in development**.

---

### 📉 Analyze with SQL Profiler or Application Insights

* Use **SQL Server Profiler**, **EF Profiler**, or **Azure Application Insights** to analyze SQL execution times.
* Identify **slow queries**, **missing indexes**, or **expensive joins**.

---

### 🧠 Check for Common Performance Pitfalls

#### 1. **N+1 Query Problem**

Occurs when multiple queries are executed for related data.
**Fix**: Use **Eager Loading** (`Include`) or **Explicit Loading**.

```csharp
// Avoid this
var orders = context.Orders.ToList();
foreach (var o in orders)
    Console.WriteLine(o.Customer.Name);

// Prefer this
var orders = context.Orders.Include(o => o.Customer).ToList();
```

#### 2. **Unnecessary Tracking**

By default, EF tracks changes for all retrieved entities.
**Fix**: Use `AsNoTracking()` for read-only operations.

```csharp
var users = context.Users.AsNoTracking().ToList();
```

#### 3. **Inefficient Queries**

Generated SQL may not be optimal.
**Fix**: Use **projections** (`Select`), **pagination**, and **filters** to reduce data load.

```csharp
var products = context.Products
    .Where(p => p.IsActive)
    .Select(p => new { p.Name, p.Price })
    .ToList();
```

---

### 🧪 Enable Query Splitting (EF Core 5+)

To avoid Cartesian explosion in Include-heavy queries:

```csharp
context.Orders
    .Include(o => o.Items)
    .AsSplitQuery()
    .ToList();
```

---

### 🚀 Use Indexes and Database Tuning

* Ensure frequently queried columns are **indexed**.
* Use **database views** or **stored procedures** for complex or performance-critical operations.

---

### 🧼 Clean Navigation and Lazy Loading

* Disable or avoid **lazy loading** in high-traffic scenarios.
* Review and minimize deep or recursive navigation chains.

---

### 📌 Answer Summary

* **Enable EF logging** to inspect generated SQL.
* Use tools like **SQL Profiler** or **Application Insights** to track slow DB queries.
* Avoid N+1 problems using **Include** or **Explicit Loading**.
* Use `**AsNoTracking()**` for read-only operations.
* Optimize queries with **projections, filters, and pagination**.
* Apply **indexes** and use **query splitting** for complex includes.
* Regularly **profile and refactor** your queries and context usage for better performance.

<br>

### 62. What are the common exceptions in EF and how do you resolve them?
### ❗ Common Exceptions in Entity Framework and How to Fix Them

EF can throw various runtime exceptions during database operations. Understanding these helps quickly debug and resolve issues without breaking the application flow.

---

### 🔐 `DbUpdateConcurrencyException`

**Cause**: Occurs when EF detects that the data in the database has changed since it was loaded into memory.

**Fix**:

* Use **Concurrency Tokens** like `RowVersion`.
* Catch the exception and decide whether to **retry**, **overwrite**, or **cancel**.

```csharp
try
{
    context.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    // Resolve the conflict: refresh data or merge manually
}
```

---

### 📛 `DbEntityValidationException` (EF6)

**Cause**: Validation errors occurred while saving changes.

**Fix**:

* Check `ModelState.IsValid` in ASP.NET.
* Ensure entity properties satisfy **data annotations** or **fluent API rules**.

```csharp
foreach (var error in ex.EntityValidationErrors)
{
    foreach (var validationError in error.ValidationErrors)
    {
        Console.WriteLine($"Property: {validationError.PropertyName}, Error: {validationError.ErrorMessage}");
    }
}
```

---

### 📉 `InvalidOperationException`

**Common Causes**:

* Multiple active result sets (MARS) not enabled.
* Query includes filtered navigation properties incorrectly.
* Using disposed `DbContext`.

**Fix**:

* Enable MARS in the connection string.
* Avoid using navigation properties in disconnected scenarios.
* Use `using` statement properly for context lifecycle.

```csharp
using (var context = new AppDbContext())
{
    // safe context usage
}
```

---

### 🔄 `DbUpdateException`

**Cause**: General database update issue — typically due to **foreign key violations**, **null constraints**, or **unique key conflicts**.

**Fix**:

* Check **inner exception** for SQL error details.
* Ensure entity relationships and required fields are properly set.

---

### 💥 `SqlException`

**Cause**: A lower-level database error (e.g., timeout, syntax error, connection failure).

**Fix**:

* Inspect **inner exception** message.
* Use **retry logic** for transient failures (like network errors).

---

### 🧩 `ObjectDisposedException`

**Cause**: Using a `DbContext` that has already been disposed.

**Fix**:

* Do not store context in long-lived variables.
* Use **short-lived DbContext** with proper scoping.

---

### 💬 `InvalidCastException` or `FormatException`

**Cause**: Mapping mismatch between entity property and database column types.

**Fix**:

* Ensure the entity property type matches the corresponding DB column type.
* Check for model mismatches in Code-First approach.

---

### 📌 Answer Summary

* `DbUpdateConcurrencyException`: Handle version/token conflicts gracefully.
* `DbEntityValidationException`: Validate entities against rules before saving.
* `InvalidOperationException`: Avoid misuse of `DbContext`, use MARS if needed.
* `DbUpdateException`: Resolve data integrity issues and check foreign key constraints.
* `SqlException`: Handle database-level errors with retries or error checks.
* `ObjectDisposedException`: Avoid reusing a disposed `DbContext`.
* `InvalidCastException`: Ensure property types match database schema.

<br>

### 63. How can you debug EF’s generated SQL queries?
### 🔎 Viewing and Debugging EF's Generated SQL Queries

When working with Entity Framework, it's important to see the actual SQL being executed—especially when performance is critical or results seem incorrect. EF provides several ways to inspect or log the SQL it generates.

---

### 📜 Use `LogTo()` in EF Core

In EF Core, the most direct way to log SQL is using the `LogTo` method in `DbContextOptionsBuilder`.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer("YourConnectionString")
        .LogTo(Console.WriteLine, LogLevel.Information)
        .EnableSensitiveDataLogging(); // Optional
}
```

* Shows generated SQL, connection details, and execution times.
* `EnableSensitiveDataLogging()` displays parameters — use **only in development**.

---

### 🧪 Use Debug Tools like SQL Profiler

* **SQL Server Profiler** can capture all SQL queries sent to SQL Server.
* Ideal for debugging performance, indexes, or seeing what's sent from EF at runtime.
* Filters can be set by app name or login to narrow the scope.

---

### 📦 Use EF Interceptors (EF Core 3+)

EF Core allows you to create custom **interceptors** to log or manipulate SQL commands.

```csharp
public class SqlCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult<DbDataReader> ReaderExecuting(
        DbCommand command, CommandEventData eventData, InterceptionResult<DbDataReader> result)
    {
        Console.WriteLine(command.CommandText);
        return base.ReaderExecuting(command, eventData, result);
    }
}
```

Register the interceptor during context configuration.

---

### 🐞 For EF6: Use `Database.Log`

In EF6, use the `Database.Log` property to write generated SQL to console or file.

```csharp
public MyDbContext()
{
    this.Database.Log = Console.Write;
}
```

---

### 📤 Use `ToQueryString()` in EF Core (for debugging specific queries)

```csharp
var query = context.Users.Where(u => u.IsActive);
string sql = query.ToQueryString(); // View the raw SQL
Console.WriteLine(sql);
```

* Great for unit tests or debugging a single LINQ query.
* Works only with IQueryable — not after `.ToList()` or async execution.

---

### ⚠️ Avoid Logging in Production

* Logging SQL adds overhead.
* `EnableSensitiveDataLogging` can expose user data — **disable it in production**.

---

### 📌 Answer Summary

* Use `**LogTo()**` in EF Core to log SQL to console or file.
* Enable `**EnableSensitiveDataLogging()**` to view query parameters (development only).
* Use `**SQL Server Profiler**` for external monitoring.
* `**ToQueryString()**` helps inspect a query before execution.
* Use `**Database.Log**` in EF6 to print SQL.
* Use `**interceptors**` for advanced logging and auditing scenarios.

<br>

### 64. What are the steps to resolve issues with entity states not updating as expected?
### 🔄 Resolving Issues with Entity States Not Updating in EF

Sometimes, changes to entities in EF aren’t tracked or saved properly. This is usually due to incorrect handling of entity states or `DbContext`. Knowing how EF tracks state is key to resolving such issues.

---

### 🧠 Understand EF Entity States

EF tracks entities in one of the following states:

* `Added` – New entity to be inserted.
* `Unchanged` – No changes detected.
* `Modified` – Updated values exist.
* `Deleted` – Entity should be removed.
* `Detached` – Not tracked by `DbContext`.

Use this to inspect:

```csharp
var state = context.Entry(entity).State;
```

---

### ✅ Ensure Entity Is Tracked

If an entity is `Detached`, EF won’t track or save it.

**Fix**:

* Attach it using `context.Attach(entity)`.
* Or mark it as modified: `context.Entry(entity).State = EntityState.Modified;`

```csharp
context.Attach(entity);
context.Entry(entity).State = EntityState.Modified;
```

---

### 🗂️ Use Correct `DbContext` Instance

Using multiple or incorrect `DbContext` instances may cause EF to lose tracking.

**Fix**:

* Perform all related operations using the same `DbContext`.
* Avoid creating new `DbContext` unless required for a new unit of work.

---

### 🔎 Explicitly Set Modified Properties (Optional)

When updating partial data, EF may not detect changes unless values are truly different.

**Fix**:

```csharp
context.Entry(entity).Property(p => p.Name).IsModified = true;
```

Useful when updating disconnected models (e.g., from APIs or web forms).

---

### ❌ Avoid Mixing Tracked and Untracked Entities

If you attach a new object while another instance of the same entity is tracked, EF will throw an exception.

**Fix**:

* Use `.AsNoTracking()` when reading, if you don’t want tracking.
* Or check the local cache:

```csharp
var existing = context.Set<EntityType>().Local.FirstOrDefault(e => e.Id == entity.Id);
```

---

### 💾 Save Changes Correctly

Ensure you're calling `context.SaveChanges()` or `SaveChangesAsync()` to persist changes.

---

### 🔁 Refresh Entity from Database

If entity state isn’t consistent with the database, reload it.

```csharp
context.Entry(entity).Reload();
```

---

### 🧪 Use Change Tracker for Debugging

Inspect tracked entities to find out what EF is monitoring.

```csharp
var entries = context.ChangeTracker.Entries();
```

---

### 📌 Answer Summary

* Check if the entity is in `Detached` state — attach or mark as modified.
* Use the **same `DbContext` instance** for all related operations.
* Set **individual properties** as modified when needed.
* Avoid **mixing tracked and untracked** entity instances.
* Always call `**SaveChanges()**` or `**SaveChangesAsync()**`.
* Use `**ChangeTracker**` or inspect `**Entry(entity).State**` to debug.
* Use `**Reload()**` to refresh mismatched entities from the DB.

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
