<h1 align="center">Hi, I'm Muhammed Rizal 👋</h1>

<p align="center">
  <strong>Backend-Focused Full-Stack Developer</strong><br/>
  Building secure, scalable, and testable web applications with <strong>.NET Core</strong> & <strong>React</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-8.0-512BD4?style=flat&logo=dotnet&logoColor=white" alt=".NET 8" />
  <img src="https://img.shields.io/badge/ASP.NET_Core-Web_API-512BD4?style=flat&logo=dotnet&logoColor=white" alt="ASP.NET Core" />
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react&logoColor=black" alt="React 18" />
  <img src="https://img.shields.io/badge/EF_Core-ORM-512BD4?style=flat&logo=dotnet&logoColor=white" alt="EF Core" />
  <img src="https://img.shields.io/badge/SQL_Server-Database-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white" alt="SQL Server" />
  <img src="https://img.shields.io/badge/Swagger-OpenAPI-85EA2D?style=flat&logo=swagger&logoColor=black" alt="Swagger" />
  <img src="https://img.shields.io/badge/JWT-Auth-000000?style=flat&logo=jsonwebtokens&logoColor=white" alt="JWT" />
  <img src="https://img.shields.io/badge/xUnit-Testing-5C2D91?style=flat&logo=dotnet&logoColor=white" alt="xUnit" />
  <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=flat&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat" alt="MIT License" />
</p>

<p align="center">
  <a href="https://muhammedrizalnp.vercel.app/" target="_blank">
    <img src="https://img.shields.io/badge/Portfolio-rizal.dev-0A66C2?style=flat&logo=vercel&logoColor=white" alt="Portfolio" />
  </a>
  &nbsp;
  <a href="https://www.linkedin.com/in/muhammed-rizal/">
    <img src="https://img.shields.io/badge/LinkedIn-Muhammed_Rizal-0A66C2?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn" />
  </a>
  &nbsp;
  <a href="mailto:mdrizalnp@gmail.com">
    <img src="https://img.shields.io/badge/Email-mdrizalnp@gmail.com-D14836?style=flat&logo=gmail&logoColor=white" alt="Email" />
  </a>
</p>

---

## 📋 Table of Contents

- [About Me](#-about-me)
- [Tech Stack](#-tech-stack)
- [Current Focus](#-current-focus)
- [Key Concepts I Work With](#-key-concepts-i-work-with)
- [Project Structure (Typical API)](#-project-structure-typical-api)
- [Getting Started](#-getting-started)
- [Configuration & Environment Variables](#-configuration--environment-variables)
- [API Documentation & Usage](#-api-documentation--usage)
- [Running Tests](#-running-tests)
- [Docker Support](#-docker-support)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)
- [Get in Touch](#-get-in-touch)

---

## 👨‍💻 About Me

I'm a backend-focused full-stack developer specializing in **.NET Core** Web APIs and **React** frontends. I care deeply about writing secure, maintainable, and well-architected code — from JWT-based authentication flows to clean, layered service architectures.

- 🔭 **Current focus:** Production-grade ASP.NET Core APIs paired with React 18 frontends
- 🛡️ **Specialties:** AuthN/AuthZ (JWT, role-based policies), middleware pipelines, global error handling
- ⚙️ **Practices:** Clean Architecture, async/await patterns, DTO mapping, dependency injection
- 🧪 **Testing & Tooling:** xUnit, Swagger/OpenAPI, Postman, Git/GitHub, GitHub Actions
- 🎯 **Goal:** Become a job-ready, production-grade full-stack .NET developer
- 🌐 **Portfolio:** [muhammedrizalnp.vercel.app](https://muhammedrizalnp.vercel.app/)

---

## 🛠 Tech Stack

| Layer | Technologies |
|---|---|
| **Backend** | .NET 8, ASP.NET Core Web API, Entity Framework Core 8 |
| **Frontend** | React 18, Axios / Fetch API, REST integration |
| **Authentication** | JWT Bearer Tokens, ASP.NET Core Identity, Role-Based Authorization |
| **Database** | SQL Server (production), SQLite (local dev) |
| **API Docs** | Swagger / OpenAPI 3.0 |
| **Testing** | xUnit, Moq, Postman |
| **DevOps** | Git, GitHub, GitHub Actions (CI/CD), Docker |

---

## 🎯 Current Focus

- 🔐 Building production-ready APIs with advanced security, role-based policies, and refresh token flows
- ⚡ Implementing response caching and cursor/offset pagination for scalable query performance
- 🧪 Writing automated unit and integration tests with **xUnit** + **Moq**, and wiring up **GitHub Actions** CI pipelines
- 🔗 Integrating .NET Core backends with React 18 frontends into cohesive full-stack applications
- 🐳 Containerising APIs with **Docker** and Docker Compose for reproducible dev environments

---

## 🔑 Key Concepts I Work With

### JWT Authentication

Configuring JWT Bearer authentication in `Program.cs` with full token validation:

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

Generating a signed JWT token inside a service:

```csharp
// Services/TokenService.cs
public string GenerateToken(AppUser user, IList<string> roles)
{
    var claims = new List<Claim>
    {
        new(ClaimTypes.NameIdentifier, user.Id),
        new(ClaimTypes.Email, user.Email!),
    };
    claims.AddRange(roles.Select(r => new Claim(ClaimTypes.Role, r)));

    var key   = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_config["Jwt:Key"]!));
    var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var token = new JwtSecurityToken(
        issuer:             _config["Jwt:Issuer"],
        audience:           _config["Jwt:Audience"],
        claims:             claims,
        expires:            DateTime.UtcNow.AddHours(1),
        signingCredentials: creds
    );

    return new JwtSecurityTokenHandler().WriteToken(token);
}
```

---

### Role-Based Authorization

Protecting endpoints using `[Authorize]` with role constraints:

```csharp
// Controllers/UsersController.cs

[Authorize(Roles = "Admin")]
[HttpDelete("{id}")]
public async Task<IActionResult> DeleteUser(int id)
{
    var success = await _userService.DeleteAsync(id);
    if (!success) return NotFound();
    return NoContent();  // 204
}

[Authorize(Roles = "Admin,Manager")]
[HttpPut("{id}")]
public async Task<IActionResult> UpdateUser(int id, UpdateUserDto dto) { /* ... */ }

[Authorize]  // Any authenticated user
[HttpGet("me")]
public async Task<ActionResult<UserDto>> GetCurrentUser() { /* ... */ }
```

---

### Global Error Handling Middleware

Catching unhandled exceptions across the entire pipeline and returning a consistent JSON error envelope:

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
        catch (NotFoundException ex)
        {
            logger.LogWarning(ex, "Resource not found");
            context.Response.StatusCode  = StatusCodes.Status404NotFound;
            context.Response.ContentType = "application/json";
            await context.Response.WriteAsJsonAsync(new { error = ex.Message });
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

Register it early in the middleware pipeline:

```csharp
// Program.cs
app.UseMiddleware<ExceptionMiddleware>();
app.UseAuthentication();
app.UseAuthorization();
```

---

### DTO Mapping Pattern

Keeping domain models internal and exposing only clean, shaped DTOs:

```csharp
// DTOs/UserDto.cs
public record UserDto(int Id, string Name, string Email);
public record CreateUserDto(string Name, string Email, string Password);
public record UpdateUserDto(string? Name, string? Email);

// Services/UserService.cs
public async Task<UserDto?> GetByIdAsync(int id)
{
    var user = await _context.Users.FindAsync(id);
    return user is null ? null : new UserDto(user.Id, user.Name, user.Email);
}

public async Task<UserDto> CreateAsync(CreateUserDto dto)
{
    var user = new User { Name = dto.Name, Email = dto.Email };
    _context.Users.Add(user);
    await _context.SaveChangesAsync();
    return new UserDto(user.Id, user.Name, user.Email);
}
```

---

### Async Repository Pattern

Abstracting data access behind interfaces for testability:

```csharp
// Interfaces/IUserRepository.cs
public interface IUserRepository
{
    Task<User?> GetByIdAsync(int id);
    Task<IEnumerable<User>> GetAllAsync();
    Task<User> AddAsync(User user);
    Task DeleteAsync(int id);
}

// Data/Repositories/UserRepository.cs
public class UserRepository(AppDbContext context) : IUserRepository
{
    public async Task<User?> GetByIdAsync(int id) =>
        await context.Users.FindAsync(id);

    public async Task<IEnumerable<User>> GetAllAsync() =>
        await context.Users.AsNoTracking().ToListAsync();

    public async Task<User> AddAsync(User user)
    {
        context.Users.Add(user);
        await context.SaveChangesAsync();
        return user;
    }

    public async Task DeleteAsync(int id)
    {
        var user = await GetByIdAsync(id);
        if (user is not null)
        {
            context.Users.Remove(user);
            await context.SaveChangesAsync();
        }
    }
}
```

Register with DI:

```csharp
// Program.cs
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IUserService, UserService>();
```

---

## 📁 Project Structure (Typical API)

```
MyApi/
├── Controllers/              # HTTP endpoints — thin, delegate to services
│   └── UsersController.cs
├── DTOs/                     # Request/response shapes (records or classes)
│   ├── UserDto.cs
│   ├── CreateUserDto.cs
│   └── UpdateUserDto.cs
├── Middleware/               # Custom pipeline middleware
│   └── ExceptionMiddleware.cs
├── Models/                   # EF Core entity models
│   └── User.cs
├── Services/                 # Business logic layer
│   ├── Interfaces/
│   │   └── IUserService.cs
│   └── UserService.cs
├── Data/                     # EF Core DbContext and migrations
│   ├── AppDbContext.cs
│   ├── Repositories/
│   │   └── UserRepository.cs
│   └── Migrations/
├── appsettings.json          # Base configuration
├── appsettings.Development.json
└── Program.cs                # App entry point, DI registration, middleware pipeline
```

---

## 🚀 Getting Started

### Prerequisites

Make sure the following are installed before cloning:

| Tool | Version | Link |
|---|---|---|
| .NET SDK | 8.0+ | [dotnet.microsoft.com](https://dotnet.microsoft.com/download) |
| Node.js | 18+ | [nodejs.org](https://nodejs.org/) |
| SQL Server | 2019+ or LocalDB | [microsoft.com/sql-server](https://www.microsoft.com/en-us/sql-server) |
| Git | Latest | [git-scm.com](https://git-scm.com/) |
| Docker (optional) | Latest | [docker.com](https://www.docker.com/) |

---

### Backend — Clone & Run

```bash
# 1. Clone the repository
git clone https://github.com/muhammed-rizal/<repo-name>.git
cd <repo-name>

# 2. Restore NuGet packages
dotnet restore

# 3. Set up user secrets (see Configuration section)
dotnet user-secrets set "Jwt:Key" "your-super-secret-key-min-32-chars"
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "your-connection-string"

# 4. Apply EF Core migrations and seed the database
dotnet ef database update

# 5. Run the API
dotnet run --project src/MyApi
```

The API will be available at:
- **HTTPS:** `https://localhost:5001`
- **Swagger UI:** `https://localhost:5001/swagger`

---

### Frontend — Clone & Run

```bash
# Navigate to the client directory
cd client

# Install npm dependencies
npm install

# Start the development server
npm run dev
```

The React app will be available at `http://localhost:5173` (Vite default).

---

## ⚙️ Configuration & Environment Variables

This project uses `appsettings.json` for base config and `dotnet user-secrets` for sensitive local values. **Never commit secrets to source control.**

### `appsettings.json` (non-sensitive defaults)

```json
{
  "Jwt": {
    "Key":      "",
    "Issuer":   "https://localhost:5001",
    "Audience": "https://localhost:5173"
  },
  "ConnectionStrings": {
    "DefaultConnection": ""
  },
  "Logging": {
    "LogLevel": {
      "Default":               "Information",
      "Microsoft.AspNetCore":  "Warning"
    }
  }
}
```

### Local Development — `dotnet user-secrets`

```bash
# Initialise user secrets for the project (one-time)
dotnet user-secrets init --project src/MyApi

# Set secrets
dotnet user-secrets set "Jwt:Key"      "your-256-bit-secret-min-32-chars"  --project src/MyApi
dotnet user-secrets set "Jwt:Issuer"   "https://localhost:5001"              --project src/MyApi
dotnet user-secrets set "Jwt:Audience" "https://localhost:5173"              --project src/MyApi
dotnet user-secrets set "ConnectionStrings:DefaultConnection" \
  "Server=localhost;Database=MyDb;Trusted_Connection=True;TrustServerCertificate=True;" \
  --project src/MyApi

# List all secrets
dotnet user-secrets list --project src/MyApi
```

### Production — Environment Variables

In CI/CD or production hosting (Azure App Service, Railway, etc.), supply values as environment variables:

```bash
Jwt__Key=your-production-secret
Jwt__Issuer=https://api.yourdomain.com
Jwt__Audience=https://yourdomain.com
ConnectionStrings__DefaultConnection=Server=prod-server;Database=MyDb;...
```

> **Note:** ASP.NET Core maps double-underscore `__` to the colon `:` separator in configuration keys.

---

## 📖 API Documentation & Usage

Swagger UI is enabled in development at `/swagger`. All endpoints are documented with request/response schemas.

### Authentication Flow

**1. Register**

```http
POST /api/auth/register
Content-Type: application/json

{
  "name":     "John Doe",
  "email":    "john@example.com",
  "password": "P@ssw0rd123!"
}
```

**2. Login — receive JWT**

```http
POST /api/auth/login
Content-Type: application/json

{
  "email":    "john@example.com",
  "password": "P@ssw0rd123!"
}
```

Response:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresAt": "2025-07-01T12:00:00Z"
}
```

**3. Call protected endpoints**

```http
GET /api/users/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Example — CRUD Endpoints

```http
GET    /api/users          # Get all users (Admin only)
GET    /api/users/{id}     # Get user by ID (Authenticated)
POST   /api/users          # Create user   (Admin only)
PUT    /api/users/{id}     # Update user   (Admin, Manager)
DELETE /api/users/{id}     # Delete user   (Admin only)
```

All error responses follow a consistent envelope:

```json
{
  "error": "Human-readable error message."
}
```

---

## 🧪 Running Tests

Tests are written with **xUnit** and **Moq**. All test projects live under the `tests/` directory.

```bash
# Run all tests
dotnet test

# Run with detailed output
dotnet test --verbosity normal

# Run a specific test project
dotnet test tests/MyApi.Tests

# Run with code coverage (requires coverlet)
dotnet test --collect:"XPlat Code Coverage"
```

### Example — Unit Test

```csharp
// tests/MyApi.Tests/Services/UserServiceTests.cs
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _repoMock = new();
    private readonly UserService _sut;

    public UserServiceTests()
    {
        _sut = new UserService(_repoMock.Object);
    }

    [Fact]
    public async Task GetByIdAsync_ReturnsNull_WhenUserNotFound()
    {
        // Arrange
        _repoMock.Setup(r => r.GetByIdAsync(999)).ReturnsAsync((User?)null);

        // Act
        var result = await _sut.GetByIdAsync(999);

        // Assert
        Assert.Null(result);
    }

    [Fact]
    public async Task GetByIdAsync_ReturnsUserDto_WhenUserExists()
    {
        // Arrange
        var user = new User { Id = 1, Name = "Alice", Email = "alice@example.com" };
        _repoMock.Setup(r => r.GetByIdAsync(1)).ReturnsAsync(user);

        // Act
        var result = await _sut.GetByIdAsync(1);

        // Assert
        Assert.NotNull(result);
        Assert.Equal("Alice", result.Name);
    }
}
```

---

## 🐳 Docker Support

A `Dockerfile` and `docker-compose.yml` are included for containerised local development.

### Build & Run with Docker

```bash
# Build the API image
docker build -t myapi:latest .

# Run the container
docker run -p 5001:8080 \
  -e Jwt__Key="your-secret" \
  -e ConnectionStrings__DefaultConnection="your-conn-string" \
  myapi:latest
```

### Docker Compose (API + SQL Server)

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f api

# Stop and remove containers
docker compose down
```

`docker-compose.yml` spins up:
- `api` — ASP.NET Core Web API on port `5001`
- `db`  — SQL Server 2022 on port `1433`

> Refer to `docker-compose.yml` in the repository root for the full configuration.

---

## 📍 Roadmap

- [x] JWT authentication & role-based authorization
- [x] Global error handling middleware
- [x] DTO mapping & clean service layer
- [x] Async repository pattern with dependency injection
- [ ] Pagination & filtering (offset + cursor)
- [ ] Response caching with `IMemoryCache`
- [ ] xUnit + Moq unit test coverage (>80%)
- [ ] GitHub Actions CI/CD pipeline
- [ ] Docker & Docker Compose support
- [ ] Refresh token flow
- [ ] Rate limiting middleware (.NET 8 built-in)

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m "feat: add your feature"`
4. **Push** to the branch: `git push origin feature/your-feature-name`
5. **Open** a Pull Request against `main`

Please follow these conventions:
- Use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages
- Add or update tests for any new behaviour
- Keep PRs focused — one feature or fix per PR

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](./LICENSE) file for details.

---

## 📬 Get in Touch

I'm actively looking for junior .NET / full-stack opportunities and open to collaboration and feedback.

<p>
  <a href="https://muhammedrizalnp.vercel.app/">
    <img src="https://img.shields.io/badge/Portfolio-rizal.dev-000000?style=flat&logo=vercel&logoColor=white" alt="Portfolio" />
  </a>
  &nbsp;
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
