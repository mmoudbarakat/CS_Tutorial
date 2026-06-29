# Onion Architecture in .NET

---

## 1. What is Onion Architecture?

- **Onion Architecture** is a software architectural pattern that emphasizes **separation of concerns**, maintaining the independence of the core application logic from external systems like databases, UI, and third-party services.
    
- It organizes code into concentric layers, with the **core domain logic** at the center and dependencies on outer layers directed **inwards**.
    

---

## 2. Structure of Onion Architecture

The architecture is composed of **multiple layers**, with the innermost being the **core** and the outermost being the **UI** or **infrastructure**. Layers should only **depend on the inner layers**, never the other way around.

### 2.1 Layers Breakdown:

1. **Core Domain (Innermost Layer)**
    
    - **Entities/Models**: Represents business logic and domain models.
        
    - **Interfaces**: Defines abstractions for repositories and services.
        
    - **No dependencies on external systems**: Purely focuses on business rules.
        
2. **Application Layer**
    
    - **Use cases and services**: Implements application logic that orchestrates operations.
        
    - **Interfaces**: Can interact with the domain layer interfaces.
        
3. **Infrastructure Layer**
    
    - **External services**: Deals with databases, file systems, APIs, and other external dependencies.
        
    - **Implementation of repositories, services** defined in the application layer.
        
4. **Presentation Layer (Outermost Layer)**
    
    - **UI/Controllers/Views**: User-facing part of the application (e.g., Web API controllers, MVC Views).
        
    - **Does not directly interact with core domain**: It communicates with the application layer.
        

---

## 3. How Onion Architecture Works

### 3.1 Dependency Flow

- **Inner Layers** (Core, Application) should have no dependencies on the **outer layers** (Infrastructure, UI).
    
- The **outer layers** depend on the **inner layers**. This ensures that the core business logic remains **decoupled** from external concerns.
    

---

## 4. Example of Onion Architecture in .NET

### 4.1 Core Domain Layer (Entities and Interfaces)

In this layer, define business models and interfaces.

**Example:**

```csharp
// Core layer (Domain)
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Core layer (Interfaces)
public interface IProductRepository
{
    Task<IEnumerable<Product>> GetAllProductsAsync();
    Task<Product> GetProductByIdAsync(int id);
}
```

### 4.2 Application Layer (Use Cases)

This layer implements application-specific logic and interacts with the domain layer. It should depend on the domain layer interfaces but not on any external systems.

**Example:**

```csharp
// Application layer (Service)
public class ProductService
{
    private readonly IProductRepository _productRepository;

    public ProductService(IProductRepository productRepository)
    {
        _productRepository = productRepository;
    }

    public async Task<IEnumerable<Product>> GetAllProductsAsync()
    {
        return await _productRepository.GetAllProductsAsync();
    }

    public async Task<Product> GetProductByIdAsync(int id)
    {
        return await _productRepository.GetProductByIdAsync(id);
    }
}
```

### 4.3 Infrastructure Layer (Repositories, External Services)

This layer implements actual external services like database connections and API calls. It should implement the interfaces defined in the core domain layer.

**Example:**

```csharp
// Infrastructure layer (Repository Implementation)
public class ProductRepository : IProductRepository
{
    private readonly DbContext _context;

    public ProductRepository(DbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<Product>> GetAllProductsAsync()
    {
        return await _context.Products.ToListAsync();
    }

    public async Task<Product> GetProductByIdAsync(int id)
    {
        return await _context.Products.FindAsync(id);
    }
}
```

### 4.4 Presentation Layer (API or UI Controllers)

This layer contains the controller or UI logic that presents data to the user and interacts with the application layer.

**Example:**

```csharp
// Presentation layer (API Controller)
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly ProductService _productService;

    public ProductsController(ProductService productService)
    {
        _productService = productService;
    }

    [HttpGet]
    public async Task<IActionResult> GetAllProducts()
    {
        var products = await _productService.GetAllProductsAsync();
        return Ok(products);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetProductById(int id)
    {
        var product = await _productService.GetProductByIdAsync(id);
        if (product == null) return NotFound();
        return Ok(product);
    }
}
```

---

## 5. Key Benefits of Onion Architecture

1. **Separation of Concerns**:
    
    - The business logic is kept separate from infrastructure concerns (like databases and web APIs), which results in **easier maintenance** and **testability**.
        
2. **Testability**:
    
    - Since the core business logic is independent of external dependencies, it can be unit tested without the need to interact with the database or UI.
        
3. **Maintainability**:
    
    - Changes in external systems (e.g., swapping out a database provider) only affect the infrastructure layer, leaving the core logic intact.
        
4. **Scalability**:
    
    - The architecture is highly flexible, allowing you to **scale independently**. New UI or infrastructure components can be added without affecting the core logic.
        
5. **Decoupling**:
    
    - Onion Architecture promotes loose coupling between layers, meaning changes to the infrastructure layer or UI layer won’t affect the business logic or application layer.
        

---

## 6. Summary Table: Onion Architecture Layers

|Layer|Purpose|Dependencies|
|---|---|---|
|**Core Domain**|Contains business logic, models, and interfaces|No external dependencies.|
|**Application**|Implements use cases and application logic|Depends on Core domain interfaces.|
|**Infrastructure**|Implements data access, external services|Depends on Application interfaces.|
|**Presentation**|UI/Controllers for user interaction|Depends on Application layer.|

---

## 7. When to Use Onion Architecture?

- Use **Onion Architecture** when you need a **clean separation** between business logic and infrastructure, especially in **complex applications** with heavy business rules.
    
- Ideal for **enterprise-level applications** that require flexibility, maintainability, and testability.
    
- Perfect for applications that involve **multiple data sources**, external APIs, or multiple user interfaces.
    
---
