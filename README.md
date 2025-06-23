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

## .binlog
- See [/binlog/msbuild.redacted.binlog](./binlog/msbuild.redacted.binlog)
- See also https://msbuildlog.com/
1. `dotnet clean`
1. `dotnet build -bl`
1. `dotnet tool install binlogtool --global`
1. `binlogtool redact`

## Repro 3
- reproducible after installing _Visual Studio 17.14.7_

## Build
- `dotnet build --framework net9.0-android`
  - _Build succeeded_
- `dotnet build --framework net9.0-ios`
  - _Build succeeded_
- `dotnet build --framework net9.0-windows10.0.19041.0`
  - _Build succeeded_
- `dotnet build --framework net9.0-maccatalyst`
  - **Build failed**
    ```Text
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/PrivateHeaders' is a symlink.
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/Resources' is a symlink.
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/Versions/Current' is a symlink.
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/Headers' is a symlink.
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/Sentry' is a symlink.
    C:\Program Files\dotnet\packs\Microsoft.MacCatalyst.Sdk.net9.0_18.5\18.5.9199\tools\msbuild\Xamarin.Shared.targets(153,3): error : Can't process the zip file 'C:\Users\REDACTED__Username\.nuget\packages\sentry.bindings.cocoa\5.11.0\lib\net8.0-maccatalyst17.0\Sentry.Bindings.Cocoa.resources.zip' on this platform: the file 'Sentry-Dynamic.xcframework/ios-arm64_arm64e_x86_64-maccatalyst/Sentry.framework/Modules' is a symlink.
    ```
