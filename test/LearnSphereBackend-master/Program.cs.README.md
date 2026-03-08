# Documentation: Program.cs

## Overview
`Program.cs` is the entry point of the LearnSphere backend application. It configures the web host, registers services for Dependency Injection (DI), and defines the middleware pipeline.

## Detailed Explanation
The file uses the modern .NET 6/7/8 "Minimal API" style without a `Startup` class. It is divided into two sections: **Service Registration** (before `builder.Build()`) and **Middleware Pipeline** (after `builder.Build()`).

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `using System.Text;` | Imports text encoding utilities (used for JWT keys). |
| **2** | `using Microsoft.AspNetCore.Authentication.JwtBearer;` | Imports JWT middleware for the security pipeline. |
| **3** | `using Microsoft.EntityFrameworkCore;` | Imports the ORM used for database communication. |
| **4** | `using Microsoft.IdentityModel.Tokens;` | Imports security token validation logic. |
| **5** | `using Microsoft.OpenApi.Models;` | Imports Swagger configuration models. |
| **6-10** | `using MyProject.Api...` | Imports project-specific data, repo, and service layers. |
| **12** | `var builder = WebApplication.CreateBuilder(args);` | Initializes the app builder and configuration logic. |
| **14** | `builder.Services.AddControllers();` | Registers the controller discovery service. |
| **15** | `builder.Services.AddEndpointsApiExplorer();` | Enables endpoint metadata collection for Swagger. |
| **16-17** | `builder.Services.AddSwaggerGen(c => {` | Starts the Swagger generator block for API docs. |
| **18** | `c.SwaggerDoc("v1", ...)` | Defines the API version and basic title. |
| **20-28** | `c.AddSecurityDefinition("Bearer", ...)` | Adds the "Authorize" button to Swagger UI for JWT testing. |
| **30-43** | `c.AddSecurityRequirement(...)` | Tells Swagger that all routes require the Bearer token by default. |
| **47-49** | `builder.Services.AddDbContext<AppDbContext>(...)` | Connects the app to SQL Server using a connection string. |
| **52-58** | `builder.Services.AddScoped<I...Repository, ...Repository>();` | Registers Data Repositories for "Shared but Unique" per-request use. |
| **61-64** | `builder.Services.AddScoped<I...Service, ...Service>();` | Registers Business Services (JWT, Auth, Analytics, Uploads). |
| **67-68** | `builder.Services.AddAuthentication(...)` | Starts the authentication setup using the JWT scheme. |
| **70** | `var jwt = builder.Configuration.GetSection("Jwt");` | Pulls the JWT settings (Key/Issuer) from `appsettings.json`. |
| **72-84** | `options.TokenValidationParameters = ...` | Defines the strict rules for verifying incoming student tokens. |
| **87** | `builder.Services.AddAuthorization();` | Registers logic for role-based access (e.g., `[Authorize]`). |
| **88-95** | `builder.Services.AddCors(...)` | Defines the "ViteCors" policy to allow the React frontend access. |
| **97** | `var app = builder.Build();` | Consolidates all services and builds the final application object. |
| **100-105** | `if (!string.IsNullOrEmpty(...))` | **Self-Healing**: Checks and creates the `uploads` folder on disk. |
| **108** | `await SeedDataService.SeedAdminUserAsync(...)` | **Bootstrap**: Creates the default admin account if not present. |
| **110** | `app.UseCors("ViteCors");` | Activates the CORS policy middleware. |
| **113** | `app.UseStaticFiles();` | Enables the browser to access images/videos in `wwwroot`. |
| **115-116** | `app.UseSwagger(...)` | Mounts the Swagger documentation page for development. |
| **119** | `app.UseAuthentication();` | Checks WHO the user is (Verifies the JWT token). |
| **120** | `app.UseAuthorization();` | Checks WHAT the user can do (Verifies Roles). |
| **122** | `app.MapControllers();` | Links the URL routes to the actual Controller methods. |
| **123** | `app.Run();` | Starts the application loop and begins listening for requests. |

## Program Flow
1. **Startup**: The OS runs the binary.
2. **Setup**: Services are registered into a "Bucket" (DI container).
3. **Pipeline**: The app defines the order of checks (CORS -> Static Files -> Auth).
4. **Active**: The app waits for HTTP requests and routes them to the correct Controller.

## Why it is used
It is the "Brain" of the application initialization. Without this file, the server wouldn't know which database to use, which services are available, or how to secure the API.

## Interview Questions
1. **What is the difference between `AddScoped`, `AddTransient`, and `AddSingleton`?**
   *Answer: `AddScoped` (used here) creates a new instance per HTTP request. `AddTransient` creates a new instance every time it's requested. `AddSingleton` creates one instance for the entire lifetime of the app.*
2. **Why is the order of `app.UseAuthentication()` and `app.UseAuthorization()` important?**
   *Answer: You must know WHO the user is (Authentication) before you can decide what they are ALLOWED to do (Authorization).*
3. **What does `builder.Configuration` do?**
   *Answer: It reads settings from `appsettings.json`, environment variables, and secrets as a unified configuration source.*
