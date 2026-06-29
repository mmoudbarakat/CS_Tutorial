#  CORS (Cross-Origin Resource Sharing) in .NET — Full Guide

---

#  What is CORS?

- **CORS** stands for **Cross-Origin Resource Sharing**.
    
- It is a **browser security feature**.
    
- It **controls** whether a web page (JavaScript) from **one origin** (domain, port, protocol) can **make requests** to a **different origin**.
    

> **Simple:**  
> Without CORS → Browser **blocks** cross-origin requests for security reasons.

---

#  When CORS Happens

Imagine:

|Client Origin|API Origin|Result|
|---|---|---|
|`https://yourfrontend.com`|`https://api.yourbackend.com`|CORS is triggered|
|`https://yourfrontend.com`|`https://yourfrontend.com`|No CORS needed (same origin)|

---

#  Why Is CORS Needed?

- **APIs** are often **hosted separately** from frontends.
    
- **Browsers** need **explicit permission** to allow frontend JavaScript to talk to backend APIs on different origins.
    
- **Without proper CORS config**, the browser **blocks** the request.
    

---

#  Enabling CORS in .NET

### 1. Add CORS Service

In `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowMyFrontend",
        policy =>
        {
            policy.WithOrigins("https://yourfrontend.com")
                  .AllowAnyHeader()
                  .AllowAnyMethod();
        });
});
```

---

### 2. Use CORS Middleware

Still in `Program.cs`:

```csharp
var app = builder.Build();

app.UseCors("AllowMyFrontend");

app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

---

#  CORS Common Scenarios

|Scenario|How|
|---|---|
|Allow Specific Origin|`.WithOrigins("https://yourfrontend.com")`|
|Allow All Origins (Not Recommended)|`.AllowAnyOrigin()`|
|Allow Only Specific Methods|`.WithMethods("GET", "POST")`|
|Allow All Methods|`.AllowAnyMethod()`|
|Allow Specific Headers|`.WithHeaders("Content-Type", "Authorization")`|
|Allow All Headers|`.AllowAnyHeader()`|
|Allow Credentials (Cookies, Auth Headers)|`.AllowCredentials()` (must not use `.AllowAnyOrigin()`!)|

---

#  Important Rules about CORS

- **AllowCredentials** + **AllowAnyOrigin()** **cannot** be used together (security risk).
    
- If you allow credentials (cookies, tokens), you **must** specify allowed origins explicitly.
    
- **Preflight requests** (`OPTIONS` method) happen automatically for complex requests.
    
- **CORS is enforced by the browser**, not by the server.
    

---

#  CORS Preflight Requests

- Browser sends a **preflight OPTIONS request** to the server.
    
- If server responds with the correct headers, browser continues with actual request.
    

Example:

```http
OPTIONS /api/data HTTP/1.1
Origin: https://yourfrontend.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Authorization
```

Server must respond:

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://yourfrontend.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: Authorization
```

---

#  Protecting vs Over-Permissiveness

✅ Good Practice:

```csharp
.WithOrigins("https://yourfrontend.com")
.AllowAnyHeader()
.AllowAnyMethod()
```

 Bad Practice (Security Risk):

```csharp
.AllowAnyOrigin()
.AllowAnyMethod()
.AllowAnyHeader()
.AllowCredentials()
```

---

#  Real-World Full Example

```csharp
var builder = WebApplication.CreateBuilder(args);

// 1. Add CORS Policy
builder.Services.AddCors(options =>
{
    options.AddPolicy("ProductionCorsPolicy", policy =>
    {
        policy.WithOrigins("https://frontend.example.com")
              .WithMethods("GET", "POST", "PUT")
              .WithHeaders("Content-Type", "Authorization")
              .AllowCredentials();
    });
});

// 2. Configure Middleware
var app = builder.Build();

app.UseCors("ProductionCorsPolicy");

app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();
app.Run();
```

---

#  Common Errors and Fixes

|Error|Cause|Fix|
|---|---|---|
|No 'Access-Control-Allow-Origin' header present|Server didn’t allow origin|Configure CORS properly|
|CORS policy error: credentialed requests|AllowCredentials needs specific origin|Use `.WithOrigins()`, not `.AllowAnyOrigin()`|
|Preflight request fails (OPTIONS 403 Forbidden)|Server does not respond correctly to preflight|Handle OPTIONS request or fix CORS headers|

---

#  CORS in Development vs Production

|Environment|Strategy|
|---|---|
|Development|Allow localhost origins freely|
|Production|Allow only specific trusted domains|

Example for Development:

```csharp
.WithOrigins("http://localhost:3000", "http://localhost:4200")
.AllowAnyHeader()
.AllowAnyMethod();
```

---
