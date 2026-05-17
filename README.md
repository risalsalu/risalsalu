<h1 align="center">Hi, I'm Muhammed Rizal 👋</h1>

<p align="center">
  <strong>Aspiring Full-Stack Developer</strong> · .NET Core & React · Building secure, clean, and testable APIs
</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-8.0-512BD4?style=flat&logo=dotnet&logoColor=white" alt=".NET 8" />
  <img src="https://img.shields.io/badge/ASP.NET_Core-Web_API-512BD4?style=flat&logo=dotnet&logoColor=white" alt="ASP.NET Core" />
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react&logoColor=black" alt="React" />
  <img src="https://img.shields.io/badge/EF_Core-ORM-512BD4?style=flat&logo=dotnet&logoColor=white" alt="EF Core" />
  <img src="https://img.shields.io/badge/Swagger-OpenAPI-85EA2D?style=flat&logo=swagger&logoColor=black" alt="Swagger" />
  <img src="https://img.shields.io/badge/JWT-Auth-000000?style=flat&logo=jsonwebtokens&logoColor=white" alt="JWT" />
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat" alt="MIT License" />
</p>

---

## 📋 Table of Contents

- [About Me](#-about-me)
- [Tech Stack](#-tech-stack)
- [Current Focus](#-current-focus)
- [Key Concepts I Work With](#-key-concepts-i-work-with)
- [Sample Code Snippets](#-sample-code-snippets)
- [Project Structure (Typical API)](#-project-structure-typical-api)
- [Getting Started with My Projects](#-getting-started-with-my-projects)
- [Running Tests](#-running-tests)
- [Roadmap](#-roadmap)
- [Get in Touch](#-get-in-touch)

---

## 👨‍💻 About Me

I'm an aspiring full-stack developer specializing in **.NET Core** backend APIs and **React** frontends. I focus on writing secure, maintainable, and well-structured code — from JWT-based authentication to clean architecture patterns.

- 🔭 **Current focus:** ASP.NET Core Web APIs + React frontends
- 🛡️ **Interests:** AuthN/AuthZ (JWT, role-based access), middleware pipelines, global error handling
- ⚙️ **Practices:** Clean Architecture, async/await, DTO mapping, dependency injection
- 🧪 **Tools:** Swagger/OpenAPI, Postman, Git/GitHub, xUnit
- 🎯 **Goal:** Become a confident, job-ready full-stack developer

---

## 🛠 Tech Stack

| Layer | Technologies |
|---|---|
| **Backend** | .NET 8, ASP.NET Core Web API, Entity Framework Core |
| **Frontend** | React 18, Axios / Fetch API, REST |
| **Auth** | JWT Bearer Tokens, Role-Based Authorization |
| **Database** | SQL Server, SQLite (dev) |
| **API Docs** | Swagger / OpenAPI |
| **Testing** | xUnit, Postman |
| **DevOps** | Git, GitHub, GitHub Actions (learning) |

---

## 🎯 Current Focus

- 🔐 Building production-ready APIs with advanced security and role-based policies
- ⚡ Implementing caching and pagination for scalable performance
- 🧪 Writing automated tests with **xUnit** and setting up **CI/CD pipelines**
- 🔗 Integrating .NET Core APIs with React frontends into full-stack projects

---

## 🔑 Key Concepts I Work With

### JWT Authentication

```csharp
// Program.cs — Registering JWT Bearer Authentication
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer           = true,
            ValidateAudience         = true,
            ValidateLifetime         = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer              = builder.Configuration["Jwt:Issuer"],
            ValidAudience            = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey         = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]!))
        };
    });
```

### Role-Based Authorization

```csharp
// Protecting an endpoint by role
[Authorize(Roles = "Admin")]
[HttpDelete("{id}")]
public async Task<IActionResult> DeleteUser(int id)
{
    await _userService.DeleteAsync(id);
    return NoContent();
}
```

### Global Error Handling Middleware

```csharp
// Middleware/ExceptionMiddleware.cs
public class ExceptionMiddleware(RequestDelegate next, ILogger<ExceptionMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await next(context);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Unhandled exception");
            context.Response.StatusCode  = StatusCodes.Status500InternalServerError;
            context.Response.ContentType = "application/json";
            await context.Response.WriteAsJsonAsync(new { error = "An unexpected error occurred." });
        }
    }
}
```

### DTO Mapping Pattern

```csharp
// DTOs/UserDto.cs
public record UserDto(int Id, string Name, string Email);

// Mapping inside a service
public async Task<UserDto?> GetByIdAsync(int id)
{
    var user = await _context.Users.FindAsync(id);
    return user is null ? null : new UserDto(user.Id, user.Name, user.Email);
}
```

---

## 📁 Project Structure (Typical API)

```
MyApi/
├── Controllers/          # API endpoints
├── DTOs/                 # Data Transfer Objects
├── Middleware/           # Custom middleware (error handling, logging)
├── Models/               # Domain/entity models
├── Services/             # Business logic (interfaces + implementations)
├── Data/                 # EF Core DbContext and migrations
├── appsettings.json      # App configuration
└── Program.cs            # Entry point & service registration
```

---

## 🚀 Getting Started with My Projects

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [Node.js 18+](https://nodejs.org/) *(for React frontend)*
- [SQL Server](https://www.microsoft.com/en-us/sql-server) or SQLite *(for local dev)*
- [Git](https://git-scm.com/)

### Clone & Run (Backend)

```bash
# 1. Clone the repository
git clone https://github.com/muhammed-rizal/<repo-name>.git
cd <repo-name>

# 2. Restore dependencies
dotnet restore

# 3. Apply database migrations
dotnet ef database update

# 4. Run the API
dotnet run --project src/MyApi
```

The API will be available at `https://localhost:5001`. Swagger UI: `https://localhost:5001/swagger`

### Clone & Run (Frontend)

```bash
cd client
npm install
npm run dev
```

---

## ⚙️ Configuration & Environment Variables

Update `appsettings.json` or use `dotnet user-secrets` for local development:

```json
{
  "Jwt": {
    "Key":      "your-256-bit-secret-key-here",
    "Issuer":   "https://localhost:5001",
    "Audience": "https://localhost:3000"
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyDb;Trusted_Connection=True;"
  }
}
```

```bash
# Using dotnet user-secrets (recommended for local dev)
dotnet user-secrets set "Jwt:Key" "your-super-secret-key"
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "your-connection-string"
```

---

## 🧪 Running Tests

```bash
# Run all tests
dotnet test

# Run with detailed output
dotnet test --verbosity normal

# Run a specific test project
dotnet test tests/MyApi.Tests
```

Example xUnit test:

```csharp
public class UserServiceTests
{
    [Fact]
    public async Task GetByIdAsync_ReturnsNull_WhenUserNotFound()
    {
        // Arrange
        var service = new UserService(/* mock dependencies */);

        // Act
        var result = await service.GetByIdAsync(999);

        // Assert
        Assert.Null(result);
    }
}
```

---

## 📍 Roadmap

- [x] JWT authentication & role-based authorization
- [x] Global error handling middleware
- [x] DTO mapping & clean service layer
- [ ] Pagination & filtering
- [ ] Response caching
- [ ] xUnit test coverage
- [ ] GitHub Actions CI/CD pipeline
- [ ] Docker containerization

---

## 📬 Get in Touch

<p>
  <a href="https://www.linkedin.com/in/muhammed-rizal/">
    <img src="https://img.shields.io/badge/LinkedIn-Muhammed_Rizal-0A66C2?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn" />
  </a>
  &nbsp;
  <a href="mailto:mdrizalnp@gmail.com">
    <img src="https://img.shields.io/badge/Email-mdrizalnp@gmail.com-D14836?style=flat&logo=gmail&logoColor=white" alt="Email" />
  </a>
</p>

---

<p align="center">
  <em>Open to feedback, collaboration, and junior .NET / full-stack opportunities! 🚀</em>
</p>
