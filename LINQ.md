# Reflection vs Expression Trees in .NET (User Data Access)

## Reflection

- **What**: Runtime inspection of types, properties, fields, methods.
    
- **How it gets data**:
    
    - You ask an object: _"What are your properties?"_
        
    - You get or set values dynamically at runtime.
        
- **Example**:
    

```csharp
var property = user.GetType().GetProperty("Name");
var nameValue = property.GetValue(user);
```

- **Pros**:
    
    - Very **flexible** (works on any object without compile-time knowledge).
        
    - Quick to set up.
        
- **Cons**:
    
    - **Slower** (uses metadata lookup at runtime).
        
    - **No compile-time safety** (spelling errors won't be caught at compile time).
        
    - Harder to optimize by the JIT compiler.
        

## Expression Trees

- **What**: Compile expressions into executable code at runtime or build dynamic code.
    
- **How it gets data**:
    
    - Build a lightweight function that _compiles_ to access the property fast.
        
    - Example: create a delegate to get a property value efficiently.
        
- **Example**:
    

```csharp
var param = Expression.Parameter(typeof(User), "user");
var body = Expression.Property(param, "Name");
var lambda = Expression.Lambda<Func<User, object>>(
    Expression.Convert(body, typeof(object)),
    param
);
var getName = lambda.Compile();
var nameValue = getName(user);
```

- **Pros**:
    
    - **Much faster** after compilation (almost like hand-written code).
        
    - **Compile-time like safety** if built properly.
        
    - JIT compiler can optimize expressions better.
        
- **Cons**:
    
    - **More complex** to write.
        
    - Initial build cost (creating the Expression Tree).
        

# Quick Comparison Table

|Aspect|Reflection|Expression Trees|
|---|---|---|
|Speed|Slow (runtime lookup)|Fast (compiled delegate)|
|Ease of use|Very easy|More complex|
|Compile-time safety|No|Partial (depends on usage)|
|Best for|Simple inspection, tools, admin|High-performance data access|
|Example use-case|Admin panels, metadata readers|High-speed ORM like Entity Framework|

# IEnumerable vs IQueryable, Select(), and Update() in EF Core

## Select() Behavior

- The `Select()` function on **IQueryable** expects an **Expression Tree** (`Expression<Func<TSource, TResult>>`) and is translated and executed on the **database side**.
    
- The `Select()` **extension method** on **IQueryable** **translates into SQL** and runs on the **server (database)**.
    
- The `Select()` **extension method** on **IEnumerable** runs **in memory** (after data is loaded).
    

## IEnumerable vs IQueryable

|Feature|IEnumerable|IQueryable|
|---|---|---|
|Target|In-memory collections (RAM)|External data sources (DB, API)|
|Execution|In-memory (client-side)|Server-side (translated to SQL)|
|Performance|Loads entire collection before querying|Queries only needed data|
|Use-case|When data is already in memory|When querying large external data|

## Update Behavior in EF Core

- `context.Update(entity);`
    
    - **Attaches** the entity to the context.
        
    - **Marks** the entity as **Modified** in the **Change Tracker**.
        
    - Generates an **UPDATE query that updates all columns** in the database for that entity.
        
    - **Requires** the user to **send all entity attributes**, because **it will update all fields** in the database (even if only one was changed).
        

## In Short

- **IQueryable** `Select()` = Expression Tree → SQL → Database Execution
    
- **IEnumerable** `Select()` = In-Memory Execution
    
- **Update()** = All fields updated; user must provide a full entity.

---

