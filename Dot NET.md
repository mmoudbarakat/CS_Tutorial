#  ViewModels vs DTOs in .NET 

---

#  What Are DTOs (Data Transfer Objects)?

- **DTO**: A **plain object** used to **transfer data** across application boundaries.
    
- Purpose: **Minimize data**, **secure** sensitive fields, and **shape data** for APIs or services.
    

##  Characteristics

- Only **carries data** (no business logic).
    
- Can be shaped differently than the source model (Entity/Domain Model).
    
- Typically used between:
    
    - API ↔ Frontend
        
    - Service ↔ API Layer
        
    - Microservice ↔ Microservice
        

---

#  Example of DTO

```csharp
public class UserDto
{
    public string FullName { get; set; }
    public string Email { get; set; }
}
```

Mapping:

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.FirstName + " " + src.LastName));
```

---

#  What Are ViewModels?

- **ViewModel**: A **model specifically prepared for the UI** (Frontend or Razor View).
    
- Purpose: Include **only** the data needed **to render a view/page**, often including:
    
    - UI-specific properties
        
    - Validation attributes
        
    - Dropdown lists, page states, extra fields for display
        

---

#  Example of ViewModel

```csharp
public class RegisterViewModel
{
    [Required]
    public string Email { get; set; }

    [Required]
    [MinLength(8)]
    public string Password { get; set; }

    [Compare("Password")]
    public string ConfirmPassword { get; set; }
}
```

- Includes validation rules.
    
- Meant for form submission, not backend storage.
    

---

#  Key Differences Table

|Aspect|DTO|ViewModel|
|---|---|---|
|Main Purpose|Transport data across layers or systems|Prepare data for UI (views/forms/pages)|
|Contains Validation?|No|Yes (uses DataAnnotations)|
|Usage Area|API, Microservices, External Contracts|MVC Views, Razor Pages, Frontend forms|
|Business Logic|None|None (may have small UI logic like SelectList)|
|Mapping Source|Domain/Entity models|DTOs, domain models, or direct user input|
|Example Use Case|API output/input|Form validation, Display logic|

---

#  How DTOs and ViewModels Fit Together (Pipeline Example)

### Typical Flow:

```plaintext
Database Entity 
    ⇅
Service Layer → DTOs 
    ⇅
API Controllers 
    ⇅
ViewModels (UI Forms)
    ⇅
User
```

Example:

- Backend returns **DTOs** to **API**.
    
- Frontend Razor Views use **ViewModels** for form binding and validation.
    

---

#  Mapping DTOs and ViewModels

Usually, you use **AutoMapper** between:

- Entity ↔ DTO
    
- DTO ↔ ViewModel
    

Example:

```csharp
CreateMap<User, UserDto>();
CreateMap<UserDto, UserViewModel>();
```

---

#  Example Full Real-World Scenario

## Entity

```csharp
public class User
{
    public Guid Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

## DTO

```csharp
public class UserDto
{
    public string FullName { get; set; }
    public string Email { get; set; }
}
```

## ViewModel

```csharp
public class UserProfileViewModel
{
    [Display(Name = "Full Name")]
    public string FullName { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}
```

## Mapping Profile

```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<User, UserDto>()
            .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.FirstName + " " + src.LastName));

        CreateMap<UserDto, UserProfileViewModel>();
    }
}
```

## Controller Usage

```csharp
public class UsersController : Controller
{
    private readonly IMapper _mapper;
    private readonly IUserService _userService;

    public UsersController(IMapper mapper, IUserService userService)
    {
        _mapper = mapper;
        _userService = userService;
    }

    public IActionResult Profile()
    {
        var user = _userService.GetCurrentUser();
        var userDto = _mapper.Map<UserDto>(user);
        var vm = _mapper.Map<UserProfileViewModel>(userDto);
        return View(vm);
    }
}
```

---

#  Why Separate DTOs and ViewModels?

|Reason|Explanation|
|---|---|
|Different concerns|DTOs for data transfer, ViewModels for display/forms|
|Decoupling|Changes in frontend forms don't affect APIs|
|Security|Hide internal domain logic or sensitive fields|
|Flexibility|Easy versioning and evolution of APIs and UIs separately|

---

#  Best Practices:

- **Do NOT** expose Entities directly to APIs or Views.
    
- **Separate DTOs from ViewModels**: Keep DTOs clean for transport and ViewModels for forms/UI.
    
- **Use AutoMapper**: To reduce manual mapping code.
    
- **Use Profiles**: Keep mappings organized.
    
---
