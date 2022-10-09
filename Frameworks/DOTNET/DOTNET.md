# What Is .NET
<hr>
.NET is a free, [[cross-platform]], [[open source]] developer platform for building many different types of applications.

.NET apps and libraries are build from [[Source Code|source code]] and a project file, using the .NET [[Command-Line Interface|CLI]] or an [[Integrated Development Environment]] (IDE).

Project file:
```XML
<Project Sdk="Microsoft.NET.Sdk">
	<PropertyGroup>
		<OutputType>Exe</OutputType>
		<TargetFramework>net6.0</TargetFramework>
	</PropertyGroup>
</Project>
```

Source code:
```csharp
Console.WriteLine("Hello World!");
```

.NET CLI:
```Bash
% dotnet run
Hello, World!
```

## Binary distributions
* .NET SDK -- Set of tools, libraries, and runtimes for development, building, and testing apps.
* .NET Runtimes -- Set of [[Runtime|runtimes]] and libraries, for running apps.