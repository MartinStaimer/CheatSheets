
## Interne Methoden aus Klasse für Unit Test verfügbar machen

```C#
using System.Runtime.CompilerServices;

[assembly: InternalsVisibleTo("MyTests")]
```

# Alle Dateien aus Ordner und Unterordner zurückgeben

```C#
foreach (var item in Directory.EnumerateFiles(path, "*.txt", SearchOption.AllDirectories))
{
  // Do Soemthing
}        
```

