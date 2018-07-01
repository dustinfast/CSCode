# C# and .Net Development in Visual Studio Code
C# and .Net Core are a robust combo, but developing and debugging in Visual Studio can be awkward if you prefer the keyboard and terminal approach embodied by VSCode. Thankfully, it's easy to get VSCode up and running for use with these technologies.  
  
**This article demonstrates how to:**

* Setup VSCode for C# and .Net Core development.
* Create two independant projects - A Console Application (VSCodeCSDemo.Console) and a Class Library (VSCodeCSDemo.Lib) that's accessible to it.
* Create a top-level solution to simultaneously build, manage, and debug both projects.
* Build and Debug the project from within VSCode.

**See ./VSCodeCSDemo for the resulting solution**

## Setup

From your shell, install VSCode and the .Net Core SDK with your favorite package manager. On Windows, I like [choco](https://chocolatey.org):

```bash
choco install visualstudiocode -Y
choco install dotnetcore-sdk -Y
```

## Solution and Project files

Open VSCode in a your folder of choice, toggle open an integrated terminal with `CTRL + '`, and enter run following:

```bash
# Install requisite VSCode extensions
# (skip this step if you've previously installed them)
code --install-extension ms-vscode.csharp
code --install-extension jmrog.vscode-nuget-package-manager
code --install-extension pkief.material-icon-theme

# Create project and solution directory structure
mkdir VSCodeCSDemo
cd VSCodeCSDemo
mkdir src
mkdir ./src/VSCodeCSDemo.Console
mkdir ./src/VSCodeCSDemo.Lib

# Create ConsoleApp and ClassLIb project files
dotnet new console -o ./src/VSCodeCSDemo.Console
dotnet new classlib -o ./src/VSCodeCSDemo.Lib

# Create solution file
dotnet new sln

# Add project references to the solution
dotnet sln add ./src/VSCodeCSDemo.Console/VSCodeCSDemo.Console.csproj
dotnet sln add ./src/VSCodeCSDemo.Lib/VSCodeCSDemo.Lib.csproj

# Reference the ClassLib project from the ConsoleApp,
dotnet add ./src/VSCodeCSDemo.Console/VSCodeCSDemo.Console.csproj reference ./src/VSCodeCSDemo.Lib/VSCodeCSDemo.Lib.csproj

# Open VSCodeCSDemo.Console's Program.cs - The workspace will be automatically updated by VSCode. Click 'Yes' to update project assets when prompted.
code ./src/VSCodeCSDemo.Console/Program.cs
```

## Build/Debug
Hit `F5` to build and debug the solution.

**Error CS0234:**  
In this demo our project and solution names are different. Because this creates ambiguity in the compiler, you will likely receive the following build error:  
  
`Program.cs(9,13): error CS0234: The type or namespace name 'WriteLine' does not exist in the namespace 'VSCodeCSDemo.Console' (are you missing an assembly reference?)`
  
The fix is to use the solution's namespace in your project. In our case, we make the following change in `.src/VSCodeCSDemo.Console/Program.cs`.

```CSharp
using System;

namespace VSCodeCSDemo.Console   // <--- Change this
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

To this:

```CSharp
using System;

namespace VSCodeCSDemo   // <--- To this
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

## A note on solution files

You can get away with not using a solution file at all for your C# and .Net projects in VSCode! However, they are necessary if you want to link projects together.
  
To build a standalalone project with no solution file, simply use `dotnet new PROJECT_TYPE`.  
  
  For more information on the dotnet command, including a list of project types, checkout the [.Net Core SDK docs](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new?tabs=netcore21).
