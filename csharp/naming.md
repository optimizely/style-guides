# Naming Conventions

In the following examples, any of the guidance pertaining to elements marked `public` is also applicable when working with `protected` and `protected internal` elements, all of which are  intended to be visible to external callers.

## Pascal Case

### Code

Use pascal casing ("PascalCasing") when naming a `class`, `record`, `public` field, `public` property, `namespace`, `enum`, or `struct`.

When naming an `interface`, use pascal casing in addition to prefixing the name with an `I`. This clearly indicates to consumers that it's an `interface`.

When naming `public` members of types, such as fields, properties, events, methods, and local functions, use pascal casing.

```csharp
public interface IWorkerQueue { }

public class ExampleEvents
{
    // A public field
    public bool IsValid;

    // An init-only property
    public IWorkerQueue WorkerQueue { get; init; }

    // An event
    public event Action EventProcessing;

    // Struct    
    public struct ValueCoordinate { }

    // A public record with parameters that become public properties
    public record PhysicalAddress(
        string Street,
        string City,
        string StateOrProvince,
        string ZipCode);

    // TODO: mike document this better in text 
    private string _FooBar;
    public string FooBar
    {
        get 
        {
            return _FooBar;
        }
        private set
        {
            _FooBar = value;
        }
    }

    // Method
    public void StartEventProcessing()
    {
        // Local function
        static int CountQueueItems() => WorkerQueue.Count;
        // ...
    }
}
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
```

Naming convention is unaffected by modifiers such as `readonly`.


```csharp
public class DataService
{
    private readonly int _maxWorkers;
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

## CAPS_SNAKE Case

```csharp
    private const string MY_VAR = "Hi";

    public static int UNIVERSAL_ANSWER = 42;
```