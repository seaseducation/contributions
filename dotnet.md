# .NET Contribution Guide
This guide desribues the types of projects and code style for submissions using the .NET platform.

## Workflow
Please use the [Suggested Workflow section of the .NET Contribution](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/contributing-workflow.md#suggested-workflow).

Additionally, make sure your contributions have passing test coverage before submitting.

## API Changes/Additions
*What is an API change?*
- Change to the name or scope of a public or protected interface, class, property, field, method, indexer, or constructor.
- Change to the signature of a public or protected method, indexer, or constructor.
- Any other change that would require a code change for consumers to use the new functionality.
API Changes are considered _breaking_. Breaking changes are accepted only as a last-resort bug fix or as part of a major planned refactor.
Some exceptions may be made for constructor signatures where dependencies are expected to be resolved by a container.

*What is an API addition?*
- A new public or protected interface, class, property, field, method, indexer, or constructor.

## Project Target Frameworks
We currently prefer the following target frameworks for new submissions:
- `netstandard2.0` for projects intended to be consumed in .NET Framework.
- `netcoreapp3.1` until the release of .NET 6.
- `net5.0` for prototypes only until officially released.

Because this list will need to change over time, the criteria for consideration is as follows:

### .NET Standard
We will consider submissions using any official release of .NET Standard, but prefer 2.0.

### .NET Core and .NET 5+
We will consider:
- The most recent official release.
- The most recent Long Term Support (LTS) release, if different from above.
- Language-stable previews and release candidates _for prototype submissions only_.

### .NET Framework (4.x and earlier)
We do not accept .NET Framework submissions except within existing repositories where the framework is already in use.

## Code Style
You will find our style guide cribs heavily from [the one used by the .NET team](https://github.com/dotnet/corefx/blob/master/Documentation/coding-guidelines/coding-style.md), but has some significant variations.

In most cases, a `.editorconfig` file has been included in the repository root that defines the guidelines for file types expected in the repository.

### Non-Code Files
For non code files (xml, etc), our current best guidance is consistency. When editing files, keep new code and changes consistent with the style in the files. For new files, it should conform to the style for that component. If there is a completely new component, anything that is reasonably broadly accepted is fine.

### Code Files
1. We use C# for .NET projects. 
   - Submissions using F# will be given consideration if the language use is appropriate. 
   - Submissions in Visual Basic will not be accepted.
2. We use [Allman style](http://en.wikipedia.org/wiki/Indent_style#Allman_style) braces, where each brace begins on a new line. A single line statement block can go without braces but the block must be properly indented on its own line and must not be nested in other statement blocks that use braces (See rule 17 for more details). One exception is that a `using` statement is permitted to be nested within another `using` statement by starting on the following line at the same indentation level, even if the nested `using` contains a controlled block.
3. We use four spaces of indentation (no tabs).
4. We use `_camelCase` for internal and private fields and use `readonly` where possible. 
   - Prefix internal and private instance fields with `_`, static fields with `s_` and thread static fields with `t_`. 
   - When used on static fields, `readonly` should come after `static` (e.g. `static readonly` not `readonly static`).  
   - Public fields should be used sparingly and should use PascalCasing with no prefix when used.
5. We avoid `this.` unless absolutely necessary. 
6. We always specify the visibility, even if it's the default (e.g. `private string _foo` not `string _foo`). Visibility should be the first modifier (e.g. `public abstract` not `abstract public`).
7. Namespace imports should be specified at the top of the file, *outside* of `namespace` declarations.
   - Namespaces should be sorted alphabetically, with the exception of `System.*` namespaces, which are to be placed on top of all others.
8. Avoid more than one empty line at any time. For example, do not have two blank lines between members of a type.
9. Avoid spurious free spaces. For example avoid `if (someVar == 0)...`, where the dots mark the spurious free spaces.
   Consider enabling "View White Space (Ctrl+R, Ctrl+W)" or "Edit -> Advanced -> View White Space" if using Visual Studio to aid detection.
10. If a file happens to differ in style from these guidelines (e.g. private members are named `m_member` rather than `_member`), the existing style in that file takes precedence.
11. We only use `var` when it's obvious what the variable type is (e.g. `var stream = new FileStream(...)` not `var stream = OpenStandardInput()`).
12. We use language keywords instead of BCL types (e.g. `int, string, float` instead of `Int32, String, Single`, etc) for both type references as well as method calls (e.g. `int.Parse` instead of `Int32.Parse`). See issue [391](https://github.com/dotnet/corefx/issues/391) for examples.
13. We use PascalCasing to name all our constant local variables and fields. The only exception is for interop code where the constant value should exactly match the name and value of the code you are calling via interop.
14. We use ```nameof(...)``` instead of ```"..."``` whenever possible and relevant.
15. Fields should be specified at the top within type declarations.
16. When including non-ASCII characters in the source code use Unicode escape sequences (\uXXXX) instead of literal characters. Literal non-ASCII characters occasionally get garbled by a tool or editor.
17. When using labels (for goto), indent the label one less than the current indentation.
18. When using a single-statement if, we follow these conventions:
    - Never use single-line form (for example: `if (source == null) throw new ArgumentNullException("source");`)
    - Using braces is always accepted, and required if any block of an `if`/`else if`/.../`else` compound statement uses braces or if a single statement body spans multiple lines.
    - Braces may be omitted only if the body of *every* block associated with an `if`/`else if`/.../`else` compound statement is placed on a single line.
19. String literals that will be user-facing should reference a [resource file](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-3.1#resource-files) entry. 
    Resource files should be named using [this convention](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-3.1#resource-file-naming).
	This applies to all submissions, and not just ASP.NET.
	
## Semantic Versioning
We strictly use [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

Code owners will assign and adjust package version numbers as needed using the following criteria:
- For breaking API changes (see above definition), increment the `major` version.
- For backward-compatible API additions, increment the `minor` version.
- For all other changes, increment the `patch` version.

> Should relatively trivial API changes force a `major` version change? Should a large number of new features only increment the `minor` version?.

Yes on both counts. We do not engage in _romantic_ versioning. Package versions exist to prevent seemingly trivial changes from breaking builds; they are to be read by machines rather than humans.
