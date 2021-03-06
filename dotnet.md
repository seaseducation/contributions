# .NET Contribution Guide
This guide describes the types of projects and code style for submissions using the .NET platform.

## Workflow
Please use the [Suggested Workflow for .NET Core Contributions](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/contributing-workflow.md#suggested-workflow).

Additionally, make sure your contributions have passing test coverage before submitting.

## API Changes/Additions
*What is an API change?*
- Change to the name or scope of a public or protected type (interface, class, struct, record) or member (property, field, method, indexer, or constructor).
- Change to the signature of a public or protected member.
- Adding a constructor to an existing type.
- Any other change that would require a code change for consumers to use the new functionality.
API Changes are considered _breaking_. Breaking changes are accepted only as a last-resort bug fix or as part of a major planned refactor.
Some exceptions may be made for constructor signatures where dependencies are expected to be resolved by a container.

*What is an API addition?*
- A new public or protected type and its members.
- A new public or protected member (except constructors) in an existing type that does not affect the accessibility of existing members. Examples:
  - Adding a new method overload is an _addition_. Adding a new parameter to a method/constructor signature is a _change_, even if the parameter is optional.
  - Adding a parameterless constructor to an existing type that previously had no constructor is an _addition_. Constructor overloads or signature changes are _changes_.

## Target Frameworks
We currently prefer the following target frameworks for new submissions:
- `netstandard2.0` for projects intended to be consumed in .NET Framework.
- `netcoreapp3.1` until the release of .NET 6.
- `net5.0` for prototypes only until officially released.

Because this list will need to change over time, the criteria for consideration is as follows:

### .NET Standard
We will consider submissions using any official release of .NET Standard, but prefer 2.0.

### .NET Core and .NET 5+
We will consider submissions targeting:
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
2. We run Code Cleanup (`Ctrl+K, Ctrl+E` in Visual Studio or the broom icon at the bottom of the code window) on modified files. You can apply many of our code style rules by adding the following fixers to your Code Cleanup profile:
   - Apply file header preferences
   - Format document
   - Sort usings
   - Remove unnecessary usings
   - Remove unused variables
   - Remove unnecessary casts
   - Add accessibility modifiers
   - Sort accessibility modifiers
   - Make private fields readonly when possible
   - Apply language/framework type preferences   
3. We use [Allman style](http://en.wikipedia.org/wiki/Indent_style#Allman_style) braces, where each brace begins on a new line. A single line statement block can go without braces only in the following conditions:
   - Guard clauses (for example: `if (source == null) throw new ArgumentNullException(nameof(source));`). See point 20 for more information.
   - Nested `using` blocks where the nested block begins on the next line.
   - Inline expression-bodied functions (for example: `int GetFoo() => foo;`.
4. We use four spaces of indentation (no tabs). This is specified in `.editorconfig`.
5. We use `camelCase` for internal and private fields and use `readonly` where possible. 
   - Do not prefix field names (e.g., `m_foo` or `str_foo`). 
   - When used on static fields, `readonly` should come after `static` (e.g. `static readonly` not `readonly static`).  
   - Public fields should be used sparingly and should use PascalCasing with no prefix when used.
6. We avoid superfluous use of `this.`.
7. We always specify the visibility, even if it's the default (e.g. private string _foo not string _foo). Visibility should be the first modifier (e.g. `public abstract` not `abstract public`). Code Cleanup can apply this for you.
8. Namespace imports should be specified at the top of the file, *outside* of `namespace` declarations.
9. Namespaces should be sorted alphabetically. Code Cleanup can apply this for you.
10. Avoid more than one empty line at any time. For example, do not have two blank lines between members of a type.
11. Avoid spurious free spaces. For example avoid `if (someVar == 0)...`, where the dots mark the spurious free spaces.
    Code Cleanup can apply this for you.
12. If a file happens to differ in style from these guidelines (e.g. private members are named `m_member` rather than `member`), the existing style in that file takes precedence.
13. We prefer the use of `var` except when it is difficult to infer the variable type from usage (e.g., `var validators = GetValidators();` vs `IEnumerable<IValidator<ReallyLongTypeName>> validators = GetValidators();`).
14. We use language keywords instead of BCL types (e.g. `int, string, float` instead of `Int32, String, Single`, etc) for both type references as well as method calls (e.g. `int.Parse` instead of `Int32.Parse`).
15. We use PascalCasing to name all our constant local variables and fields. The only exception is for interop code where the constant value should exactly match the name and value of the code you are calling via interop.
16. We use ```nameof(...)``` instead of ```"..."``` whenever possible and relevant.
17. Fields should be specified at the top within type declarations.
18. When including non-ASCII characters in the source code use Unicode escape sequences (\uXXXX) instead of literal characters. Literal non-ASCII characters occasionally get garbled by a tool or editor.
19. When using labels (for goto), indent the label one less than the current indentation.
20. When using a single-statement `if`, we follow these conventions:
	- Only use single-line form for guard clauses (for example: `if (source == null) throw new ArgumentNullException(nameof(source));`).
    - Braces and proper indentation are required for all other `if` statements.
21. String literals that will be user-facing should reference a [resource file](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-3.1#resource-files) entry. 
    Resource files should be named using [this convention](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-3.1#resource-file-naming).
	This applies to all submissions, and not just ASP.NET.
	
## Semantic Versioning
We strictly use [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

Code owners will assign and adjust package version numbers as needed using the following criteria:
- For breaking API changes (see above definition), increment the `major` version.
- For backward-compatible API additions, increment the `minor` version.
- For all other changes, increment the `patch` version.

> Should relatively trivial API changes force a `major` version change? Should a large number of new features only increment the `minor` version?

Yes on both counts. We do not engage in _romantic_ versioning. Package versions exist to prevent seemingly trivial changes from breaking builds; they are to be read by machines rather than humans.
