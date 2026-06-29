
#  JWT (JSON Web Tokens) in .NET

#  What Is JWT?

- **JWT** = **JSON Web Token**.
    
- It's a **compact**, **self-contained** way to **transmit information** between parties securely as a **JSON object**.
    
- Information can be **verified and trusted** because it is **digitally signed**.
    
- Typically used for:
    
    - Authentication
        
    - Authorization
        
    - Data Integrity
        

---

#  JWT Structure

A JWT has **three parts**, separated by **dots** (`.`):

```plaintext
HEADER.PAYLOAD.SIGNATURE
```

### 1. Header

- Type of token (JWT) and signing algorithm (e.g., HMAC SHA256).
    

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 2. Payload

- Claims (data) about the user.
    

```json
{
  "sub": "1234567890",
  "name": "Mahmoud",
  "role": "Admin",
  "exp": 1716239022
}
```

### 3. Signature

- Protects the token against tampering.
    
- Created by encoding the Header and Payload, and signing with a **secret key**.
    

---

#  JWT in .NET

1. User logs in → Server authenticates credentials.
    
2. Server **generates JWT** and **returns** it to client.
    
3. Client stores JWT (typically in local storage, session storage, or cookies).
    
4. Client sends JWT on every request (Authorization header).
    
5. Server **validates** JWT before granting access.
    

---

# Basic JWT Example in .NET

### Install Needed Packages

```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

---

#  Setting Up JWT Authentication in `Program.cs`

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true,
        ValidateIssuerSigningKey = true,
        ValidIssuer = "yourdomain.com",
        ValidAudience = "yourdomain.com",
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("SuperSecretKey12345"))
    };
});

builder.Services.AddAuthorization();
var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();
app.Run();
```

---

#  How to Generate JWT Token

```csharp
public string GenerateJwtToken(User user)
{
    var securityKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("SuperSecretKey12345"));
    var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);

    var claims = new[]
    {
        new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
        new Claim(ClaimTypes.Name, user.Username),
        new Claim(ClaimTypes.Role, user.Role)
    };

    var token = new JwtSecurityToken(
        issuer: "yourdomain.com",
        audience: "yourdomain.com",
        claims: claims,
        expires: DateTime.Now.AddHours(1),
        signingCredentials: credentials
    );

    return new JwtSecurityTokenHandler().WriteToken(token);
}
```

---

#  Sending JWT from Client

Every HTTP request should include the JWT in **Authorization** header:

```http
Authorization: Bearer {token}
```

Example using HttpClient:

```csharp
client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

---

#  How .NET Validates JWT

When a protected API is called:

- Middleware reads Authorization header.
    
- Middleware **validates token signature** and **claims** based on TokenValidationParameters.
    
- If valid → User is authenticated.
    
- If invalid → Returns 401 Unauthorized.
    

---

#  Protecting Endpoints with JWT

Add `[Authorize]` attribute to protect a controller or an action:

```csharp
[Authorize]
[HttpGet("secret")]
public IActionResult SecretData()
{
    return Ok("This is a secret only for authorized users.");
}
```

**Role-Based Authorization:**

```csharp
[Authorize(Roles = "Admin")]
[HttpGet("admin-data")]
public IActionResult AdminOnly()
{
    return Ok("Admin secret data.");
}
```

---

#  Real-World Best Practices

|Practice|Why?|
|---|---|
|Short-lived tokens (15-60 min)|Limit damage if token is stolen.|
|Use HTTPS|Prevent token theft via sniffing.|
|Refresh Tokens|Allow users to re-authenticate smoothly without login.|
|Store token securely|Avoid XSS attacks (e.g., not in localStorage).|
|Rotate signing keys|Improve security regularly.|

---

#  Advanced Topics

## Refresh Tokens

- **Access Token** expires quickly.
    
- **Refresh Token** has longer lifespan.
    
- Client uses Refresh Token to get a new Access Token without re-login.
    

.NET Example:

- Store refresh tokens securely (in DB).
    
- Validate refresh tokens separately.
    

---

#  Manual JWT Validation Example

If you want to manually validate:

```csharp
var tokenHandler = new JwtSecurityTokenHandler();
var principal = tokenHandler.ValidateToken(token, new TokenValidationParameters
{
    ValidateIssuerSigningKey = true,
    IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("SuperSecretKey12345")),
    ValidateIssuer = true,
    ValidIssuer = "yourdomain.com",
    ValidateAudience = true,
    ValidAudience = "yourdomain.com",
    ValidateLifetime = true
}, out SecurityToken validatedToken);

// principal.Identity.Name will now have the user's name
```

---
