# Blazor Single User Auth

A Blazor Web App template built with **.NET 11** that provides a ready-to-use authentication system for applications with a single administrator account.

The template uses **ASP.NET Core Identity** with a custom user store, so no database is required. User credentials are configured through application settings, while passwords are securely stored as hashes.

## Features

- 🔐 Login and logout functionality using ASP.NET Core Identity
- 👤 Single-user authentication (no registration or user management required)
- 🔒 Password hashing using Microsoft's `PasswordHasher<TUser>`
- 🍪 Cookie-based authentication
- 🛡️ Protected pages using `[Authorize]`
- 👑 Example Admin page
- 🚫 Account lockout after repeated failed login attempts
- 💾 No database required
- ⚙️ Credentials configurable through application settings
- 🎨 Bootstrap-based UI

## Requirements

- [.NET 11 SDK](https://dotnet.microsoft.com/download/dotnet/11.0)
- An IDE such as Visual Studio, JetBrains Rider or Visual Studio Code

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/hartmannronnie/Blazor.SingleUserAuth.git
```

### 2. Configure your administrator account
Open `appsettings.Development.json` and locate the Administrator configuration section.

Configure your username and password hash:

```json
{
  "Administrator": {
    "Username": "admin",
    "PasswordHash": "YOUR_PASSWORD_HASH"
  }
}
```

**Important:** Never store your actual password in the configuration file. Only store the generated password hash.

For production environments, configure these values using environment variables or another secure configuration provider instead of committing credentials to source control.

### 3. Generate a password hash
The template uses Microsoft's `PasswordHasher<IdentityUser>` to hash passwords.

Create a temporary console application and execute the following code:

```csharp
using Microsoft.AspNetCore.Identity;

public class Program
{
    public static void Main(string[] args)
    {
        var passwordHasher = new PasswordHasher<IdentityUser>();

        var user = new IdentityUser
        {
            UserName = "newUsername"
        };

        string hashedPassword = passwordHasher.HashPassword(
            user,
            "newPassword"
        );

        Console.WriteLine($"Hashed Password: {hashedPassword}");
    }
}
```

Replace `newUsername` and `newPassword` with your desired credentials.

Copy the generated hash into the `PasswordHash` property in `appsettings.Development.json`.

**Note:** The username used when generating the hash must match the configured administrator username because the Identity password hasher uses the user information when generating and verifying hashes.

### 4. Run the application
Start the application using your IDE or the .NET CLI:

```bash
dotnet run
```

Navigate to the application URL displayed in the console.

You can now log in using your configured administrator credentials.

## Authentication
The template uses ASP.NET Core Identity with a custom user store instead of the default Entity Framework-based implementation.

This means:
- No database migrations are necessary.
- No database connection is required.
- There is no registration functionality.
- Only the configured administrator account can authenticate.

### Protecting pages
To restrict access to a page, simply add the `[Authorize]` attribute:

```razor
@page "/admin"
@attribute [Authorize]

<h1>Admin</h1>

<p>Welcome to the admin area.</p>
```

Unauthenticated users are automatically redirected to the login page.

## Security Considerations
- Never commit production credentials or password hashes to a public repository.
- Use environment variables or a secure secrets provider for production configuration.
- Always use HTTPS in production.
- Keep the .NET runtime and dependencies up to date.
- The application includes account lockout to mitigate repeated password guessing attempts.
- Use a strong, unique administrator password.

## Customization
The template is intended as a starting point for projects that need simple authentication without the overhead of a database-backed user management system.

You can extend it with additional pages, roles, authorization policies and application-specific functionality.

## License
See the LICENSE file for license information.
