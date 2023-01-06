
EF Tools:
```powershell
dotnet tool install --global dotnet-ef

// Oder

dotnet tool update --global dotnet-ef

```


## Data Model

230106 Info:
Achtung ASP NET MVC Projektvorlage funktionierte nicht mit der Postgresql Docker version 15.1-alpine -> Collation Error -> Template1

Primary Keys
```C#
class FooBar
{
	// Wird automatisch zum Primary Key
	public int Id {get; set;}
	
	...
}
```

Doku: https://learn.microsoft.com/en-us/ef/core/modeling/keys?tabs=data-annotations

Nullable Strings im Data Model:
```C#
class FooBar
{
	// Wird automatisch zum Primary Key
	public int Id {get; set;}
	
	public string? BlaBlub {get; set;}

	// Funktioniert auch mit anderen Datantypen
	public int? Optional {get; set;}
}
```

Wird in der Datenbanktabelle auch zu Nullable -> Optional

Data Annotation:
```C#
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

class FooBar
{
	[MaxLength(100)]
	public string? BlaBlub {get; set;}
}
```

Doku: https://learn.microsoft.com/en-us/ef/core/modeling/entity-properties?tabs=data-annotations%2Cwithout-nrt

Data Anotation Decimal:
```C#
class FooBar
{
	[Column(TypeName = "decimal(5, 2)")]
	public decimal? BlaBlub {get; set;}
}
```

## Einbindung DB

Einfachste Datenbankanbindung:
```C#
class DataContext : DbContext
{
	// Datenmodel
	public DbSet<{Data_Model}> {NAME_OF_DBSET} {get; set;}

	// Konstruktor
	public DataContext(DbContextOptions<DataContext> options) 
		: base(options)
	{ 
	
	}
}
```

Nur Für Konsolenanwendung -> Factory implementieren!
```C#
class DataContextFactory : IDesignTimeDbContextFactory<DataContext>
{
    public DataContext CreateDbContext(string[]? args = null)
    {
        var configuration = new ConfigurationBuilder().AddJsonFile("appsettings.json").Build();

        var optionsBuilder = new DbContextOptionsBuilder<DataContext>();
        optionsBuilder
            // Uncomment the following line if you want to print generated
            // SQL statements on the console.
            // .UseLoggerFactory(LoggerFactory.Create(builder => builder.AddConsole()))
            .UseNpgsql(configuration["ConnectionString:DefaultConnection"]);

        return new DataContext(optionsBuilder.Options);
    }
}
```

Postgresql als DB Provider:
```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Andere: https://learn.microsoft.com/en-us/ef/core/providers/?tabs=dotnet-core-cli


Einfügen neue Datei -> appsettings.json
```json
{
	"ConnectionString": {
		"DefaultConnection" : "....Look SQL SERVER DOKU...."
	}
}
```

NugetPackage Installieren -> dotnet add package Microsoft.Extensions.Configuration.Json

Manage DB
```Powershell
// Migration
dotnet ef migrations add {MIGRATION_NAME}

// Update -> Erzeuge Datenbank
dotnet ef database update
```

Doku: https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli

## Arbeiten mit EF

```C#
var factory = new DataContextFactory();
using var context = factory.CreateDbContext(args);
```

Daten einfuegen:
```C#
// add Content
context.{ModelClass}.Add({DATA_OF_TYPE_MODEL_CLASS});
await context.SaveChangesAsync();

//after Save
var id = {ModelClass}.Id // hat jetzt den Wert
```

Daten hohlen:
```C#
// read Content
var fooBar = context.{ModelClass}.Where(d => d.Id == 1).FirstOrDefaultAsync();

```

Daten updaten:
```C#
// change Content
fooBar.{PROPERTY} = {NEW_VALUE};
await context.SaveChangesAsync();
```

Daten loeschen:
```C#
// add Content
context.{ModelClass}.Remove(fooBar);
await context.SaveChangesAsync();
```

