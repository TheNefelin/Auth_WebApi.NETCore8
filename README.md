# Auth_WebApi.NETCore8

### Dependencias Nuget
```
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Tools
Microsoft.AspNetCore.Identity.EntityFrameworkCore
```

### Conexion
* Crear conexion a la BD en el archivo appsettings.json
```
  "ConnectionStrings": {
    "RutaSQL": "Server=nom_server; Database=nom_db; User ID=nom_usuario; Password=contraseña; Trusted_Connection=True; TrustServerCertificate=True;"
  }
```

* Crear carpeta Conexion con la clase AppDbContext.cs
```
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace Auth_WebApi.NETCore8.Conexion
{
    public class AppDbContext : IdentityDbContext
    {
        public AppDbContext(DbContextOptions options) : base(options)
        {
        }
    }
}
```

### Archivo Program.cs
* Agregar la conexion
```
// Conexion a Base de Datos
builder.Services.AddDbContext<AppDbContext>(opcion =>
    opcion.UseSqlServer(builder.Configuration.GetConnectionString("RutaSQL"))
);
```

* Agregar Identity a los EndPonts para autenticar
```
builder.Services.AddIdentityApiEndpoints<IdentityUser>()
    .AddEntityFrameworkStores<AppDbContext>();
```

* Agrega los EndPoint de Identity
```
app.MapIdentityApi<IdentityUser>();
```

### Migración
* Crear la migracion a la base de datos con la Package Manager Console
```
// crea las tablas de indetity
Add-Migration Inicial

// inicia la migracion creando las tablas de Identity en la base de datos
update-database
```

### Agregar autenticacion a los endpoints con bearer
```
[HttpGet(Name = "GetWeatherForecast"), Authorize]
```
