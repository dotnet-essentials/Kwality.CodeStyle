# C# / .NET - IDE Configuration, Roslyn analyzers & PowerShell scripts

[![Publish on NuGet](https://github.com/dotnet-essentials/Kwality.CodeStyle/actions/workflows/Publish.yml/badge.svg)](https://github.com/dotnet-essentials/Kwality.CodeStyle/actions/workflows/publish.yml)
[![latest version](https://img.shields.io/nuget/v/Kwality.CodeStyle)](https://www.nuget.org/packages/Kwality.CodeStyle)

![](assets/readme-header.jpg)

## What's this package about?

In large teams, it's very often very difficult to maintain a consistent code style over different projects / repositories.

The goal of this package is to remove the burden that comes with the sharing code style in an organization.

## How to install this package?

Since it's packaged as a NuGet package, it can be installed using the .NET CLI.

```bash
dotnet add package Kwality.CodeStyle
```

This will install the latest version (at the time of writing) in your project.  
When the project that contains this NuGet package is being built, the Kwality.CodeStyle files & folders will be placed in the *ROOT* of your C# project.

What has been achieved now is that your project is tied to a **specific** version of the Kwality.CodeStyle NuGet package. The version will match the latest version that was available at the time when you ran the `dotnet restore` command.

You could also choose to always use the latest version that's available at the time when you run the `dotnet build` command. In order to do so, you need to use `*` as the version of the package.  
It can be achieved by using the following command:

```bash
dotnet add package Kwality.CodeStyle --version *
```

## What's inside this package?

When you build a project which contains this NuGet package, the following files will be created in the *ROOT* of your C# project:

- An `.editorconfig` file.  
  This file contains IDE (Visual Studio, Rider & ReSharper) settings.

- A `.globalconfig` file.  
  This file contains configuration for Roslyn analyzers.

- A folder `.Kwality.CodeStyle/`.  
  This contains various PowerShell scripts.

- A `.gitignore` file.
  This files prevents the `.editorconfig` file, the `.globalconfig` file and the `.Kwality.CodeStyle/` folder from being committed to the GIT repository.

Besides these files, the following settings are applied to the project in which it's installed:

- Code style is **NOT** enforced while building.
- Analysis level is set to `latest-all`.
- Warnings are treated as errors which running a CI build.  
  *See [PowerShell scripts](#powershell-scripts) for more information.*
- C#'s `nullable` feature is enabled.
- C#'s `nullable` warnings are treated as errors.
- Use the latest C# version.

You can still deviate from any of these settings by modifying the C# project file.  
For example, if you DO not want to treat C#'s `nullable` warnings as errors, you can add the following:

```xml
<WarningsAsErrors />
```

## PowerShell scripts

The following PowerShell scripts can be found in the `.Kwality.CodeStyle` folder.

- Build - *.Kwality.CodeStyle/scripts/build.ps1*  
  Builds a .NET Project / .NET Solution.  
  Arguments:
  - Path: The path to the .NET Project / .NET Solution to build.
  - CIBuild: A switch parameter. If it's set, *ALL* warnings will be treated as errors.

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

- Cover - *.Kwality.CodeStyle/scripts/cover.ps1*  
  **NOTE:** *This is only installed in test projects.*  
  Calculate the code coverage of a .NET Project.  
  Arguments:
  - Path: The path to the .NET Project / .NET Solution to build.


## Frequently asked questions.

#### *I see a lot of squiggly lines inside my IDE when I open the .NET Solution.*

The configuration files which controls the squiggly lines in your IDE (`.editorconfig` and `.globalconfig`) are NOT committed to the GIT repository. You need to run a `dotnet build` command first.

#### *I suddenly see new errors in my IDE without changing code.*

This can happen in the case when you're using the `*` version of the NuGet package.  
If you're working on a feature you're using the latest available version of the NuGet package (thanks to the `*` version). When you later decide to continue your work you might see warnings in your IDE (after a build) because in the meantime, it might be possible that the NuGet package has been updated with new rules. Since your project is configured to use the latest available version, your code will be checked against these new rules.

You can solve this case by using a fixed version of the NuGet package.