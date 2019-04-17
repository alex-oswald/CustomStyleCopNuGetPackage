# Custom StyleCop NuGet Package

Use a NuGet package to easily distribute custom rule sets for [StyleCopAnalyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers)
to use within your team or organization.

I had the need to do this and didn't really find any good places out there with all the information
I needed in one place. I've included all reference links I used to put this project together. You
can include and customize `stylecop.json` as much as you like, or exclude it. Multiple rule sets can
be added to customize what rules StyleCop and the other dot net analyzers are abiding by.

### Files in solution

#### `.nuspec`

 ([reference](https://docs.microsoft.com/en-us/nuget/reference/nuspec))

Build Action: None

- The `id` tag in the `.nuspec` file is your NuGet package `id`.
- Your `.nuspec` file must be named the same as the NuGet package `id`.
- `id`, `version`, `description`, and `authors` are required tags ([reference](https://docs.microsoft.com/en-us/nuget/reference/nuspec#general-form-and-schema)).
- `developmentDependency` should be set to `true`. ([reference](https://docs.microsoft.com/en-us/nuget/reference/nuspec#developmentdependency))
- Add the dependency for `StyleCop.Analyzers`.
- Add your files. Make sure any `props` or `targets` files have a target set to `build`. Also, your `props` and `targets` files need to be named the same as the NuGet package `id`. ([reference](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#including-msbuild-props-and-targets-in-a-package))

#### `.props` or `.targets` files

Build Action: None

- `<CodeAnalysisRuleSetLocation>` needs to be properly set.
- Use `<CodeAnalysisRuleSet>` to add your rule sets.
- Use `<AdditionalFiles>` to add `stylecop.json`.

#### Rule sets

([reference](https://docs.microsoft.com/en-us/visualstudio/code-quality/analyzer-rule-sets?view=vs-2019))

_(these files are optional)_

Build Action: C# analyzer additional file

#### `stylecop.json`

([reference](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/Configuration.md))

_(this file is optional)_

Build Action: C# analyzer additional file

#### `.csproj`

Make sure you are using the newer `PackageReference` style. ([reference](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files))

You can use the below code to pack your builds.

_Download [nuget.exe](https://www.nuget.org/downloads) and place in C:\Windows or have it in your PATH environmental variable._

```xml
<!--Pack your Debug build-->
<Target Name="PostBuildDebug" AfterTargets="PostBuildEvent" Condition="$(Configuration) == 'Debug'">
  <Exec Command="nuget pack $(ProjectFileName) -OutputDirectory $(OutDir)" />
</Target>
```
```xml
<!--Pack your Release build-->
<Target Name="PostBuildRelease" AfterTargets="PostBuildEvent" Condition="$(Configuration) == 'Release'">
  <Exec Command="nuget pack $(ProjectFileName) -OutputDirectory $(OutDir) -properties Configuration=Release" />
</Target>
```