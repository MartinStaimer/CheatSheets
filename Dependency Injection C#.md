# Beispiele Dependency Injection

Einbinden des Dependency Injection Packet:
```c#
using Microsoft.Extensions.DependencyInjection;

ServiceProvider services = new ServiceCollection()
                               .AddSingleton<IName, Name>()
                               .AddTransient<>()
                               .BuildServiceProvider();
```

Service hohlen:
```C#
var test = services.GetService<IName>();
```

Serilog einbinden:

```C#
using Serilog;

Log.Logger = new LoggerConfiguration()
                 .WriteTo.Debug()
                 .CreateLogger();

ServiceProvider services = new ServiceCollection()
                               .AddSingleton<ILogger>(Log.Logger)
                               .BuildServiceProvider();
```

oder:

```C#
using Serilog;

Log.Logger = new LoggerConfiguration()
                 .WriteTo.Debug()
                 .CreateLogger();

ServiceProvider services = new ServiceCollection()
                               .AddSingleton<ILogger>(x =>
                               {
                                  return new LoggerConfiguration().
                                  WriteTo.Debug().
                                  CreateLogger();
                               })
                               .BuildServiceProvider();
```