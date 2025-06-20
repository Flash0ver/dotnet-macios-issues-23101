# dotnet-macios-issues-23101
dotnet-macios-issues-23101

## Issues
- Sentry: https://github.com/getsentry/sentry-dotnet/issues/4292
- .NET: https://github.com/dotnet/macios/issues/23101

## Repro 1
1. On Windows
1. `dotnet --version`
   - ```
     9.0.301
     ```
1. `dotnet new maui --name SentryExample`
1. `dotnet workload restore`
1. `dotnet build`
   - Passed
1. `dotnet add package Sentry.Maui --version 5.11.0`
   - ```diff
     <ItemGroup>
       <PackageReference Include="Microsoft.Maui.Controls" Version="$(MauiVersion)" />
       <PackageReference Include="Microsoft.Extensions.Logging.Debug" Version="9.0.0" />
     + <PackageReference Include="Sentry.Maui" Version="5.11.0" />
     </ItemGroup>
     ```
1. Optional: `UseSentry` received by `Microsoft.Maui.Hosting.MauiAppBuilder`
   - ```diff
     //..
     builder
       .UseMauiApp<App>()
       .ConfigureFonts(fonts =>
       {
          fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
          fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
     +  })
     + .UseSentry(options =>
     + {
     +    options.Dsn = "<Dsn>";
       });
     //..
     ```
1. Build: Failed
   - `dotnet build`
   - _Visual Studio 2022 17.14.6 (June 2025)_

## Repro 2
1. example [Repro 1](#repro-1)
1. with _Microsoft Visual Studio Enterprise 2022 (64-bit) - Current_, _Version 17.14.5_
1. `dotnet --version`
   - ```
     9.0.301
     ```
1. `dotnet build`
   - Passed
1. update to _Microsoft Visual Studio Enterprise 2022 (64-bit) - Current_, _Version 17.14.6 (June 2025)_
1. `dotnet --version`
   - ```
     9.0.301
     ```
1. `dotnet workload restore`
1. `dotnet build`
   - Failed
