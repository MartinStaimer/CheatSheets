
## Interne Methoden aus Klasse für Unit Test verfügbar machen

```C#
using System.Runtime.CompilerServices;

[assembly: InternalsVisibleTo("MyTests")]
```

## Alle Dateien aus Ordner und Unterordner zurückgeben

```C#
foreach (var item in Directory.EnumerateFiles(path, "*.txt", SearchOption.AllDirectories))
{
  // Do Soemthing
}        
```

## Number Provider für z.B. String -> Double

```c#
 NumberFormatInfo Provider = new NumberFormatInfo
{
    NumberDecimalSeparator = ",",
    NumberGroupSeparator = ".",
    NumberGroupSizes = new int[] { 3 }
};

double num = Convert.ToDouble("10,30", Provider);

// Inhalt von num ist jetzt immer 10.30;
```

## Kurzschreibweise if then else

```c#
var newValue = value == 5 ? 3 : 5;

// entspricht
var newValue = 5;

if (value == 5)
{
  newValue = 3;
}
else
{
  newValue = 5
}
```

