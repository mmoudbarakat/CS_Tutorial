#  AutoMapper in .NET (Full Guide)

## 🛠 What is AutoMapper?

- **AutoMapper** is an object-object mapper.
    
- It automatically **maps** properties from one object to another **based on matching property names**.
    
- Helps you **avoid manually copying** data from a source object to a destination object.
    

---

# ⚡ Basic Setup

## 1. Install AutoMapper

```bash
dotnet add package AutoMapper
```

or for ASP.NET Core:

```bash
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```

---

# 🚀 Basic Example

## Mapping Between Two Classes

```csharp
public class User {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

public class UserDto {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

## Create Mapping Profile

```csharp
using AutoMapper;

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<User, UserDto>();
    }
}
```

## Use AutoMapper

```csharp
var config = new MapperConfiguration(cfg => cfg.AddProfile<MappingProfile>());
var mapper = config.CreateMapper();

var user = new User { FirstName = "Mahmoud", LastName = "Barakat" };
var userDto = mapper.Map<UserDto>(user);
```

---

# 🧠 Concepts

|Concept|Meaning|
|---|---|
|Profile|Class where you define mappings.|
|CreateMap<T, U>|Define how to map from type T to type U.|
|Mapper|Engine that applies the mappings.|

---

# All Common Cases

##  1. Simple Property Mapping

If properties **have the same name**, AutoMapper maps them **automatically**.

---

##  2. Different Property Names (Custom Mapping)

Use `.ForMember()` if names differ:

```csharp
public class User {
    public string Name { get; set; }
}

public class UserDto {
    public string FullName { get; set; }
}
```

Mapping:

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.Name));
```

---

##  3. Nested Objects Mapping

```csharp
public class Address {
    public string City { get; set; }
}

public class User {
    public Address Address { get; set; }
}

public class UserDto {
    public string City { get; set; }
}
```

Mapping:

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.City, opt => opt.MapFrom(src => src.Address.City));
```

---

## 4. Flattening

AutoMapper automatically **flattens** nested properties if names match.

```csharp
// Source
user.Address.City

// Destination
userDto.City
```

AutoMapper can map **nested.City** → **Dto.City** without needing `.ForMember()` if names match.

---

## 5. Reverse Mapping

You can **map both ways**:

```csharp
CreateMap<User, UserDto>().ReverseMap();
```

Now you can map:

- `User → UserDto`
    
- `UserDto → User`
    

---

## 6. Lists / Collections Mapping

AutoMapper automatically **maps collections**.

```csharp
List<User> users = ...;
List<UserDto> userDtos = mapper.Map<List<UserDto>>(users);
```

Also works for `IEnumerable`, `IQueryable`, etc.

---

##  7. Conditional Mapping

Map only if a condition is true:

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.FirstName, 
        opt => opt.Condition(src => !string.IsNullOrEmpty(src.FirstName)));
```

---

##  8. Value Converters / Custom Transformations

Sometimes you need **custom logic**:

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.FirstName, 
        opt => opt.MapFrom(src => src.FirstName.ToUpper()));
```

Or create a **value converter class** if complex.

---

#  Advanced Scenarios

##  1. ProjectTo (Entity Framework)

Instead of `.Map()`, you can use **`.ProjectTo<T>()`** to **translate mapping into SQL queries**:

```csharp
var userDtos = _context.Users.ProjectTo<UserDto>(_mapper.ConfigurationProvider);
```

- **Advantage**: Only needed fields are selected from DB.
    
- **Works** only with IQueryable.
    

---

## 2. Mapping into Existing Objects

Instead of creating a new object:

```csharp
var existingDto = new UserDto();
mapper.Map(user, existingDto);
```

It **fills** the existing object instead of creating a new one.

---

##  3. Ignore Certain Properties

```csharp
CreateMap<User, UserDto>()
    .ForMember(dest => dest.LastName, opt => opt.Ignore());
```

AutoMapper **won't touch** LastName.

---

# Important Tips:

- Always **validate configuration** at startup:
    

```csharp
config.AssertConfigurationIsValid();
```

- If mapping **complex entities**, prefer **ProjectTo** + **IQueryable**.
    
- Keep mappings **inside Profiles**.
    
- **Debugging**: If something is not mapped, check spelling / casing carefully.
    

---

# Summary Table

|Case|Code Example|
|---|---|
|Simple Mapping|`CreateMap<T, U>();`|
|Different Names|`.ForMember(dest => dest.X, opt => opt.MapFrom(src => src.Y))`|
|Reverse Mapping|`.ReverseMap();`|
|Nested Mapping|`.ForMember(dest => dest.City, opt => opt.MapFrom(src => src.Address.City))`|
|List Mapping|`mapper.Map<List<T>>(listSource)`|
|Conditional Mapping|`.Condition(src => src.Prop != null)`|
|Ignore Fields|`.ForMember(dest => dest.X, opt => opt.Ignore())`|
|SQL Query Mapping|`ProjectTo<T>()`|

---

# Full MappingProfile

```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<User, UserDto>()
            .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.Name))
            .ForMember(dest => dest.City, opt => opt.MapFrom(src => src.Address.City))
            .ReverseMap();
    }
}
```

---

# Common Problems & Solutions

|Problem|Reason|Solution|
|---|---|---|
|Property not mapping|Different names or missing config|Use `.ForMember()`|
|Null reference in nested object|Not handling nulls|Add null-checks or use conditional mapping|
|AutoMapper mapping wrong fields|Overlapping names|Explicitly define mapping rules|
|EF Core error on ProjectTo|Missing `ConfigurationProvider`|Inject and pass `_mapper.ConfigurationProvider`|
