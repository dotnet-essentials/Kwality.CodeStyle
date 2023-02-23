# C# / .NET - IDE Configuration, Roslyn analyzers & PowerShell scripts

[![Publish on NuGet](https://github.com/dotnet-essentials/Kwality.CodeStyle/actions/workflows/Publish.yml/badge.svg)](https://github.com/dotnet-essentials/Kwality.CodeStyle/actions/workflows/publish.yml)
[![latest version](https://img.shields.io/nuget/v/Kwality.CodeStyle)](https://www.nuget.org/packages/Kwality.CodeStyle)

## How to install this package?

To ensure that you're always using the latest version, you can use `*` as the version when installing the package.

```bash
dotnet add package Kwality.CodeStyle --version *
```

This will add the following to your `.csproj` file.

```xml
<ItemGroup>
  <PackageReference Include="Kwality.CodeStyle" Version="*" />
</ItemGroup>
```

## What's inside this package?

Once the package is installed and your project is built, you'll the following in the folder containing your `*.csproj` file.

- An `.editorconfig` file that's used by JetBrains Rider / ReSharper.  
  **NOTE:** *This file will NOT be committed to the GIT repository.*

- A `.globalconfig` file. that's used by .NET to configure the Roslyn analyzers.  
  **NOTE:** *This file will NOT be committed to the GIT repository.*

- A `.Kwality.CodeStyle/` folder which contains PowerShell scripts.  
  **NOTE:** *This file will NOT be committed to the GIT repository.*

When you install this package, the following happens automatically to the project in which it's installed:

- Code style is NOT enforced while building.
- Analysis Level is set to `latest-all`.
- Warnings are treated as errors when running a CI build.
- Enable C#'s `nullable` feature.
- C#'s `nullable` warnings are treated as errors.
- Use the latest C# version.

All the above is done by setting these properties (automatically).  

```xml
<EnforceCodeStyleInBuild>false</EnforceCodeStyleInBuild>
<AnalysisLevel>latest-all</AnalysisLevel>
<TreatWarningsAsErrors Condition="'$(CIBuild)' == 'true'">true</TreatWarningsAsErrors>
<WarningsAsErrors>nullable</WarningsAsErrors>
<Nullable>enable</Nullable>
<LangVersion>latest</LangVersion>
```

- If it's a test project, the project is excluded from being analyzed by SonarQube.
- If it's **NOT** a test project, the `internal` members are visible for test projects and for Moq.
- The assembly is marked as NOT being `CLSCompliant`.
- The `SonarAnalyzers.CSharp` NuGet package (latest version) is installed.


## Which PowerShell scripts are included?

- Build - *.Kwality.CodeStyle/scripts/build.ps1*
  Builds a .NET Project / .NET Solution.  
  Arguments:
  - Path: The path to the .NET Project / .NET Solution to build.
  - CIBuild: A switch parameter. If it's set, warnings will be treated as errors.

- Cover - *.Kwality.CodeStyle/scripts/cover.ps1*  
  **NOTE:** *This is only installed in test projects.*  
  Calculate the code coverage of a .NET Project.  
  Arguments:
  - Path: The path to the .NET Project / .NET Solution to build.

- Format - *.Kwality.CodeStyle/scripts/format.ps1*  
  Format a .NET Solution.
  Arguments:
  - Path: The path to the .NET Solution to build.

- Mutate - *.Kwality.CodeStyle/scripts/mutate.ps1*  
  **NOTE:** *This is only installed in test projects.*  
  Performs mutation testing using [Stryker.NET](https://stryker-mutator.io/docs/stryker-net/introduction/)
  Arguments:
  - TestProjectFilePath: The path to the .NET Test project.
  - ProjectFilePath: The path to the .NET project to mutate.
  - OpenReport: A switch parameter. If it's set, the report will be opened upon completion.
