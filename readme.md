# C# and .Net in VSCode
C# and .Net are a robust combo, but developing in Visual Studio can be awkward if you prefer the keyboard and terminal approach embodied by VSCode. Thankfully, it's easy to get VSCode up and running for use with these technologies.
  
**By following the steps outlined below, you will:

* Setup VSCode for C# and .Net development.
* Create two independant projects - a Console Application (VSCodeCSDemo.Console) and a Class Library (VSCodeCSDemo.Lib).
* Create a top-level solution to build and manage both projects.
* Reference the Class Library from the Console App, so it can use it's members.
* Build and Debug the project from your VSCode integrated terminal.

**See ./VSCodeCSDemo for the resulting output

## Setup

From your shell or command prompt, install VSCode and the .Net SDK with your favorite package manager. I like [choco](https://chocolatey.org):

```bash
choco install visualstudiocode -Y
choco install dotnetcore-sdk -Y
```

# Solution and Project files

Open VSCode in a your folder of choice, toggle open an integrated terminal with `CTRL + '`, and enter the following:

```bash
# Install requisite VSCode extensions
# (skip this step if they've been previously installed)
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

# Build/Debug
Hit `F5` to build and debug the solution.

**Error CS0234:**  
In this demo our project and solution names are different. Because this creates ambiguity in the compiler, you will likely receive the following build error:
  
<span style="color:red">
Program.cs(9,13): error CS0234: The type or namespace name 'WriteLine' does not exist in the namespace 'VSCodeCSDemo.Console' (are you missing an assembly reference?)
</span>
  
The fix is to change the namespace in the offending file. In the case of this demo, change `.src/VSCodeCSDemo.Console/Program.cs` from this:

```CSharp
using System;

namespace VSCodeCSDemo.Console   // <---
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

namespace VSCodeCSDemo   // <---
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

You can get away with not using a solution file at all for your C# and .Net projects in VSCode! However, they are required in the case of this demonstration for the linking together of the two projects.
  
To build a project with no solution file, simply use `dotnet new PROJECT TYPE`.