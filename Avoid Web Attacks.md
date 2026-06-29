# How to Defend Your App Against Attacks Exploiting JWT and CORS

---

## 1. JWT Protection Best Practices

### 1.1 Secure Token Storage

- **Use HttpOnly Cookies** (preferred) to store JWTs, not localStorage or sessionStorage.
    
    - HttpOnly cookies **cannot be read by JavaScript**, preventing token theft via XSS.
        

### 1.2 Use Short Expiration Times

- Set JWTs to **expire quickly** (e.g., 15 minutes).
    
- Require **refresh tokens** (stored safely) for re-authentication.
    

### 1.3 Validate Token Properly

- Always **validate signature**, **issuer**, **audience**, and **expiration** server-side.
    
- Reject tokens that do not meet your expected validation settings.
    

```csharp
options.TokenValidationParameters = new TokenValidationParameters
{
    ValidateIssuer = true,
    ValidIssuer = "https://yourdomain.com",
    ValidateAudience = true,
    ValidAudience = "youraudience",
    ValidateLifetime = true,
    ValidateIssuerSigningKey = true,
    IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("YourSecretKey"))
};
```

### 1.4 Rotate and Invalidate Tokens

- **Rotate** secret signing keys periodically.
    
- Provide **token blacklisting** or **revocation** (especially for refresh tokens).
    

### 1.5 Avoid Token Leaks in URLs

- **Never pass JWTs in query strings** (URLs are logged everywhere).
    
- Always pass JWTs in headers or HttpOnly cookies.
    

---

## 2. CORS Protection Best Practices

### 2.1 Strict Origin Whitelisting

- **Allow only trusted, specific domains** in your CORS policy.
    
- **Never** use `.AllowAnyOrigin()` in production.
    

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("StrictCors", builder =>
    {
        builder
            .WithOrigins("https://yourtrustedapp.com")
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials();
    });
});
```

### 2.2 Disallow Credentials If Not Needed

- If you **do not need cookies or authorization headers**, set `.DisallowCredentials()`.
    
- If you **do use credentials**, CORS must be configured carefully, never `.AllowAnyOrigin()` together with `.AllowCredentials()` (this is illegal and dangerous).
    

### 2.3 Explicit Methods and Headers

- Only allow necessary HTTP methods (e.g., GET, POST) and headers.
    
- Avoid `.AllowAnyMethod()` and `.AllowAnyHeader()` unless absolutely necessary.
    

### 2.4 Use Preflight Checks Properly

- The browser sends an OPTIONS request ("preflight") before certain requests.
    
- Handle preflight requests correctly and securely.
    
- Monitor for abnormal preflight patterns (can detect probing).
    

---

# Summary Table

|Attackers Might Try|Defense Using JWT and CORS|
|---|---|
|Stealing tokens from storage|Use HttpOnly cookies|
|Using expired tokens|Set short expiration and validate lifetime|
|Sending fake requests from other sites|Whitelist trusted origins strictly with CORS|
|Passing tokens through unsafe channels|Never send JWTs in URLs, use headers or secure cookies|
|Replay attacks with old tokens|Rotate secrets, support token invalidation|
|Bypassing browser CORS enforcement (using scripts)|Server must still validate origin manually if needed|

---

# Additional Defenses

- **Implement XSS protection** (Content Security Policy, input sanitization) to prevent token theft.
    
- **Implement rate limiting** to prevent brute-force attacks.
    
- **Log and monitor** failed authentication attempts, strange origins, unusual preflight requests.
    
---
