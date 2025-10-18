> 🇹🇷 [Click here for the Turkish version.](README.tr.md)

# BlogApp

`BlogApp` is the backend service for a modern blog platform where users can create posts, categories, and comments. This project is designed using **Clean Architecture** on **.NET 9**, keeping in mind the principles of sustainable, testable, and scalable software development.

*Note: This project currently includes only the backend services. A user interface will be developed soon with a modern frontend technology (React, Vue, Blazor, etc.).*

## Core Features

- **User Management:** Registration, login, and role-based authorization (Admin, User).
- **Categories:** Admins can create categories to classify posts.
- **Posts:** Users can create, update, and delete rich-content posts under specific categories.
- **Comments:** Users can comment on posts.
- **Security:** Secure endpoints protected with JWT (JSON Web Token).

## Architecture and Design Patterns

The foundation of the project is **Clean Architecture**, which provides a clear separation of concerns. The business logic is completely isolated from external dependencies (database, UI, external services).

- **Clean Architecture:** The project is divided into four main layers: `Domain`, `Application`, `Infrastructure`, and `Presentation`. All dependencies point towards the center (Domain).
- **CQRS (Command Query Responsibility Segregation):** The application logic is separated into operations that change data (Commands) and operations that read data (Queries). This makes the system more performant and manageable.
- **MediatR Design Pattern:** The MediatR library is used to decouple `Commands` and `Queries` from their handlers. This ensures a flexible and low-coupling structure.

## Technologies and Libraries Used

- **Framework:** .NET 9
- **API:** ASP.NET Core Web API
- **Database:** Entity Framework Core 9
- **Database Provider:** SQLite (for local development)
- **Authentication & Authorization:** ASP.NET Core Identity, JWT Bearer Tokens
- **CQRS Implementation:** MediatR
- **Validation:** FluentValidation
- **API Documentation:** Microsoft.AspNetCore.OpenApi (.NET 9) and Scalar UI

## API Endpoints

Endpoints marked as `(Authorization Required)` need a valid JWT Bearer token.

### Auth
- `POST /api/login` - Logs in a user and returns a JWT.
- `POST /api/register` - Creates a new user account.

### Categories
- `GET /api/categories` - Lists all categories.
- `POST /api/categories` - Creates a new category. `(Admin Role Required)`
- `GET /api/categories/{id}` - Retrieves a single category by its ID.
- `PUT /api/categories/{id}` - Updates a specific category. `(Admin Role Required)`
- `DELETE /api/categories/{id}` - Deletes a specific category. `(Admin Role Required)`
- `GET /api/categories/{category_id}/posts` - Lists all posts belonging to a specific category.

### Posts
- `GET /api/posts` - Lists all posts (pagination can be added).
- `POST /api/posts` - Creates a new post. `(Authorization Required)`
- `PUT /api/posts/{id}` - Updates a specific post. `(Authorization & Ownership Required)`
- `DELETE /api/posts/{id}` - Deletes a specific post. `(Authorization & Ownership/Admin Role Required)`
- `GET /api/posts/{post_id}/comments` - Lists all comments for a specific post.

### Comments
- `POST /api/comments` - Adds a new comment to a post. `(Authorization Required)`
- `PUT /api/comments/{id}` - Updates a specific comment. `(Authorization & Ownership Required)`
- `DELETE /api/comments/{id}` - Deletes a specific comment. `(Authorization & Ownership/Admin Role Required)`

### Users
- `GET /api/users/{user_id}/posts` - Lists all posts by a specific user.

## Getting Started

### Prerequisites

- .NET 9 SDK
- Visual Studio 2022 Preview or Visual Studio Code

### Installation Steps

1.  **Clone the Project:**
    ```sh
    git clone [https://github.com/inferna15/BlogApp.git](https://github.com/inferna15/BlogApp.git)
    cd BlogApp
    ```

2.  **Configure the `appsettings.json` File:**
    Create or edit the `appsettings.Development.json` file inside the `BlogApp.Presentation` project. **You must change** the JWT `Secret` key.
    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Data Source=../BlogApp.db"
      },
      "JwtSettings": {
        "Secret": "THIS_PART_MUST_BE_A_VERY_SECRET_AND_UNPREDICTABLE_LONG_KEY_AT_LEAST_32_CHARACTERS",
        "Issuer": "[https://api.blogapp.com](https://api.blogapp.com)",
        "Audience": "[https://webapp.blogapp.com](https://webapp.blogapp.com)",
        "ExpiryMinutes": 60
      }
    }
    ```

3.  **Create the Database (Migration):**
    To create the project's database schema, open a terminal in the solution's root directory and run the following commands:
    ```sh
    # Make sure dotnet-ef tool is installed: dotnet tool install --global dotnet-ef
    dotnet ef database update --startup-project BlogApp.Presentation
    ```
    This command will create an SQLite database file named `BlogApp.db` and seed it with initial data (Admin user, categories, etc.).

4.  **Run the Application:**
    ```sh
    dotnet run --project BlogApp.Presentation
    ```
    The application will start on ports like `https://localhost:7122` and `http://localhost:5012` by default.

## Testing the API

1.  After the application is running, navigate to the configured address for the API documentation in your browser (e.g., `https://localhost:7122/docs`).
2.  The Scalar interface will appear.
3.  Register a new user using the `POST /api/register` endpoint.
4.  Log in using the `POST /api/login` endpoint and copy the returned `token`.
5.  Click on the **"Authentication"** section at the top of the Scalar interface and paste the copied token into the "Bearer Token" field.
6.  You can now successfully make requests to endpoints protected with `[Authorize]` (those with a lock icon next to them).
