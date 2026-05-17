# Payment Project

A robust and secure REST API for managing payment information, built with ASP.NET Core 5.0 and SQL Server.

## Overview

Payment Project is a web application that provides API endpoints to create, read, update, and delete payment detail information. The application uses Entity Framework Core for data access and offers interactive documentation via Swagger.

## Main Features

- ✅ **RESTful API** - Complete CRUD endpoints for payment details
- ✅ **Swagger Documentation** - Interactive interface to test the API
- ✅ **Entity Framework Core** - Modern ORM for data access
- ✅ **SQL Server** - Robust database with LocalDB for development
- ✅ **CORS Enabled** - Integration with frontend applications (e.g., Angular on port 4200)
- ✅ **Data Validation** - Validations at the model level

## Technologies Used

- **Framework**: ASP.NET Core 5.0
- **ORM**: Entity Framework Core 5.0.6
- **Database**: SQL Server (LocalDB)
- **API Documentation**: Swashbuckle.AspNetCore 5.6.3
- **Code Generation**: Microsoft.VisualStudio.Web.CodeGeneration.Design 5.0.2

## Project Structure

```
PaymentAPI/
├── PaymentAPI/
│   ├── Controllers/          # API Controllers
│   │   ├── PaymentDetailsController.cs
│   │   └── WeatherForecastController.cs
│   ├── Models/               # Data Models
│   │   ├── PaymentDetail.cs
│   │   └── PaymentDetailContext.cs
│   ├── Migrations/           # Database Migrations
│   ├── Views/                # Views (if applicable)
│   ├── Properties/           # Project Settings
│   ├── appsettings.json      # Application Configuration
│   ├── appsettings.Development.json
│   ├── Program.cs            # Application Entry Point
│   └── Startup.cs            # Services and Middleware Configuration
└── PaymentAPI.sln            # Solution File
```

## Data Model

### PaymentDetail

Represents the details of a payment card:

| Field | Type | Description |
|-------|------|-----------|
| PaymentDetailId | int (PK) | Unique identifier for the record |
| CardOwnerName | nvarchar(100) | Name of the card holder |
| CardNumber | nvarchar(16) | Card number (max 16 digits) |
| ExpirationDate | nvarchar(5) | Expiration date (format mm/yy) |
| SecurityCode | nvarchar(3) | Security code (CVV) |

## API Endpoints

### GET /api/paymentdetails
Returns a list of all payment details.

**Response:**
```json
[
  {
    "paymentDetailId": 1,
    "cardOwnerName": "John Smith",
    "cardNumber": "1234567890123456",
    "expirationDate": "12/25",
    "securityCode": "123"
  }
]
```

### GET /api/paymentdetails/{id}
Returns a specific payment detail by ID.

**Response:**
```json
{
  "paymentDetailId": 1,
  "cardOwnerName": "John Smith",
  "cardNumber": "1234567890123456",
  "expirationDate": "12/25",
  "securityCode": "123"
}
```

### POST /api/paymentdetails
Creates a new payment detail.

**Request Body:**
```json
{
  "cardOwnerName": "John Smith",
  "cardNumber": "1234567890123456",
  "expirationDate": "12/25",
  "securityCode": "123"
}
```

### PUT /api/paymentdetails/{id}
Updates an existing payment detail.

**Request Body:**
```json
{
  "paymentDetailId": 1,
  "cardOwnerName": "John Smith",
  "cardNumber": "1234567890123456",
  "expirationDate": "12/25",
  "securityCode": "123"
}
```

### DELETE /api/paymentdetails/{id}
Deletes a payment detail by ID.

## Development Setup

### Prerequisites

- [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)
- [Visual Studio 2019+](https://visualstudio.microsoft.com/downloads/) or [Visual Studio Code](https://code.visualstudio.com/)
- [SQL Server LocalDB](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/sql-server-express-localdb) (included with Visual Studio)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/brunolssantos/PaymentProject.git
   cd PaymentProject
   ```

2. **Open the solution:**
   ```bash
   cd PaymentAPI
   ```

3. **Restore dependencies:**
   ```bash
   dotnet restore
   ```

4. **Apply database migrations:**
   ```bash
   dotnet ef database update
   ```

   Or via Package Manager Console in Visual Studio:
   ```powershell
   Update-Database
   ```

5. **Run the application:**
   ```bash
   dotnet run
   ```

The API will be available at `https://localhost:5001` (HTTPS) or `http://localhost:5000` (HTTP).

## Using the API

### Via Swagger UI

After starting the application, access the interactive documentation at:
```
http://localhost:5000/swagger
```

The Swagger interface allows you to test all endpoints directly from the browser.

### Via cURL

**GET Example:**
```bash
curl -X GET "http://localhost:5000/api/paymentdetails" -H "accept: application/json"
```

**POST Example:**
```bash
curl -X POST "http://localhost:5000/api/paymentdetails" \
  -H "Content-Type: application/json" \
  -d "{\"cardOwnerName\":\"John\",\"cardNumber\":\"1234567890123456\",\"expirationDate\":\"12/25\",\"securityCode\":\"123\"}"
```

### Via Postman

1. Import the API URL into Postman
2. Use the endpoints listed above
3. Set the `Content-Type: application/json` header for POST/PUT requests

## Database Configuration

The project uses LocalDB by default. The connection string is configured in `appsettings.json`:

```json
"ConnectionStrings": {
  "DevConnection": "Server=(LocalDb)\\MSSQLLocalDB;Database=PaymentDetailDB;Trusted_Connection=True;MultipleActiveResultSets=True;"
}
```

### Create/Update Database

To create the initial migrations:
```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

## CORS

The application is configured to accept requests from `http://localhost:4200` (typical Angular application). To modify, edit `Startup.cs`:

```csharp
app.UseCors(options =>
  options.WithOrigins("http://localhost:4200")
    .AllowAnyMethod()
    .AllowAnyHeader());
```

## Security

⚠️ **Important**: This project is a development example. For production:

- Do not store passwords in code
- Use HTTPS in production
- Implement authentication and authorization
- Validate and sanitize all inputs
- Use environment variables for sensitive data
- Consider implementing rate limiting
- Protect sensitive card data (PCI DSS compliance)

## Troubleshooting

### Database connection error
- Verify that LocalDB is installed
- Run: `sqllocaldb info` to check instances
- Confirm the connection string in `appsettings.json`

### Port already in use
- Change the port in `launchSettings.json` (Properties folder)
- Or terminate the process using the port

### Migration issues
- Delete the `Migrations` folder and database
- Run `dotnet ef migrations add InitialCreate`
- Run `dotnet ef database update`

## Useful Resources

- [ASP.NET Core 5.0 Documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/)
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- [Swagger/OpenAPI](https://swagger.io/)

## Contributing

Contributions are welcome! Please:

1. Fork the project
2. Create a branch for your feature (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is under the MIT license. See the LICENSE file for more details.

## Contact

**Developer**: Bruno L. S. Santos  
**Repository**: [github.com/brunolssantos/PaymentProject](https://github.com/brunolssantos/PaymentProject)

---

**Last Update**: 2026
