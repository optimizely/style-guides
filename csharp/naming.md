# Naming Conventions

There are several naming conventions to consider when writing C# code.

In the following examples, any of the guidance pertaining to elements marked `public` is also applicable when working with `protected` and `protected internal` elements, all of which are  intended to be visible to external callers.

## Pascal Case

### Code

Use pascal casing ("PascalCasing") when naming a `class`, `record`, `public` field, `public` property, `namespace`, `enum`, or `struct`.

```csharp
public class DataService
{
}
```

```csharp
public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);
```

```csharp
public struct ValueCoordinate
{
}
```

When naming an `interface`, use pascal casing in addition to prefixing the name with an `I`. This clearly indicates to consumers that it's an `interface`.

```csharp
public interface IWorkerQueue
{
}
```

When naming `public` members of types, such as fields, properties, events, methods, and local functions, use pascal casing.

```csharp
public class ExampleEvents
{
    // A public field
    public bool IsValid;

    // An init-only property
    public IWorkerQueue WorkerQueue { get; init; }

    // An event
    public event Action EventProcessing;

    // Method
    public void StartEventProcessing()
    {
        // Local function
        static int CountQueueItems() => WorkerQueue.Count;
        // ...
    }
}
```

When writing positional records, use pascal casing for parameters as they're the public properties of the record.

```csharp
public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);
```

For more information on positional records, see [Positional syntax for property definition](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/builtin-types/record.md#positional-syntax-for-property-definition).


### Files

- Filenames and directory names are `PascalCase`, e.g. `MyFile.cs`.
- Where possible the file name should be the same as the name of the main class in the file, e.g. `MyClass.cs`.

## Camel Case

Use camel casing ("camelCasing") when naming `private` fields, and prefix them with `_`.

```csharp
public class DataService
{
    private IWorkerQueue _workerQueue;
}
```

> [!TIP]
> When editing C# code that follows these naming conventions in an IDE that supports statement completion, typing `_` will show all of the object-scoped members.

Naming convention is unaffected by modifiers such as `const`, `static`, `readonly`, etc.


```csharp
public class DataService
{
    private static IWorkerQueue _workerQueue;

    [ThreadStatic]
    private static TimeSpan _timeSpan;
}
```

When writing method parameters and local variables use camel casing.

```csharp
public T SomeMethod<T>(int someNumber, bool isValid)
{
    var someLocalVariable = 14;
}
```

For casing, a "word" is anything written without internal spaces, including acronyms. For example, `MyRpc` instead of ~~`MyRPC`~~.

## Additional naming conventions

- Examples that don't include [using directives](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/using-directive.md), use namespace qualifications. If you know that a namespace is imported by default in a project, you don't have to fully qualify the names from that namespace. Qualified names can be broken after a dot (.) if they are too long for a single line, as shown in the following example.

```csharp
var currentPerformanceCounterCategory = new System.Diagnostics.
    PerformanceCounterCategory();
```


