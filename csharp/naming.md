# Naming Conventions

There are several naming conventions to consider when writing C# code.

In the following examples, any of the guidance pertaining to elements marked `public` is also applicable when working with `protected` and `protected internal` elements, all of which are  intended to be visible to external callers.

## Pascal case

Use pascal casing ("PascalCasing") when naming a `class`, `record`, or `struct`.

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
    // A public field, these should be used sparingly
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

## Camel case

Use camel casing ("camelCasing") when naming `private` or `internal` fields, and prefix them with `_`.

```csharp
public class DataService
{
    private IWorkerQueue _workerQueue;
}
```

> [!TIP]
> When editing C# code that follows these naming conventions in an IDE that supports statement completion, typing `_` will show all of the object-scoped members.

When working with `static` fields that are `private` or `internal`, use the `s_` prefix and for thread static use `t_`.

```csharp
public class DataService
{
    private static IWorkerQueue s_workerQueue;

    [ThreadStatic]
    private static TimeSpan t_timeSpan;
}
```

When writing method parameters, use camel casing.

```csharp
public T SomeMethod<T>(int someNumber, bool isValid)
{
}
```


## Additional naming conventions

- Examples that don't include [using directives](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/using-directive.md), use namespace qualifications. If you know that a namespace is imported by default in a project, you don't have to fully qualify the names from that namespace. Qualified names can be broken after a dot (.) if they are too long for a single line, as shown in the following example.

  :::code language="csharp" source="./snippets/coding-conventions/program.cs" id="Snippet1":::

- You don't have to change the names of objects that were created by using the Visual Studio designer tools to make them fit other guidelines.
