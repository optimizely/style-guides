# Language Guidelines

## Constants

- Variables and fields that can be made `const` should always be made `const`.
- If `const` isn’t possible, `readonly` can be a suitable alternative.
- Prefer named constants to magic numbers.

```csharp
    // Avoid magic numbers
    var circleArea =  3.141592653589 * Math.Pow(radius, 2);

    // Set a constant instead 
    const float PI_ESTIMATION = 3.141592653589;
    var circleCircumference = PI_ESTIMATION * (radius * 2);
```
- Declare const variables at the top of the block using them. 

## Implicitly Typed Local Variables

- Use [implicit typing](https://github.com/dotnet/docs/blob/main/docs/csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables.md) for local variables when the type of the variable is obvious from the right side of the assignment, or when the precise type is not important.

```csharp
var var1 = "This is clearly a string.";
var var2 = 27;
```

- Don't use [var](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/var.md) when the type is not apparent from the right side of the assignment. Don't assume the type is clear from a method name. A variable type is considered clear if it's a `new` operator or an explicit cast.

```csharp
int var3 = Convert.ToInt32(Console.ReadLine()); 
int var4 = ExampleClass.ResultSoFar();
```

- Don't rely on the variable name to specify the type of the variable. It might not be correct. In the following example, the variable name `inputInt` is misleading. It's a string.

```csharp
var inputInt = Console.ReadLine();
Console.WriteLine(inputInt);
```

- Use of `var` is encouraged if it aids readability by avoiding type names
    that are noisy, obvious, or unimportant.
  - Encouraged:
    - When the type is obvious - e.g. `var apple = new Apple();`
    - For transient variables that are only passed directly to other methods - e.g. `var item = GetItem(); ProcessItem(item);`
  - Discouraged:
    - When working with basic types - e.g. `var success = true;`
    - When working with compiler-resolved built-in numeric types - e.g. `var
        number = 12 * ReturnsFloat();`
    - When users would clearly benefit from knowing the type - e.g. `var
        listOfItems = GetList();`

- Use implicit typing to determine the type of the loop variable in [`for`](../../language-reference/statements/iteration-statements.md#the-for-statement) loops.

```csharp
for (var i = 1; i < 42; i++)
{
    Console.WriteLine($"Universal answer is not {i}.");
}
```

- Do not use implicit typing to determine the type of the loop variable in [`foreach`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/statements/iteration-statements.md#the-foreach-statement) loops.

```csharp
foreach (char ch in laugh)
{
    if (ch == 'h') 
    {
        Console.Write("H");
    }
    else {
        Console.Write(ch);
    }
}
```

## Property Styles

- For single line read-only properties, prefer expression body properties
    (`=>`) when possible.
- For everything else, use the older `{ get; set; }` syntax.

## Expression Body Syntax

For example:

```c#
int SomeProperty => _someProperty;
```

- Judiciously use expression body syntax in lambdas and properties.
- Do not use on method definitions. 
- As with methods and other scoped blocks of code, align the closing with the
    first character of the line that includes the opening brace. See sample code
    for examples.

## Field Initializers

- Initialize auto-implemented properties only for primiative types.
- Field initializers are generally encouraged.

```csharp
// Do not init auto-implemented properties
public Decision MyValue { get; set; } = new Decision("yes");
// Remove need for the constructor
private int _myValueMaxSize;
public MyClass()
{
    _myValueMaxSize = 50;
}

// Good
public int MyValue { get; set; } = 42; 
private int _myValueMaxSize = 50;
```

## Attributes

- Attributes should appear on the line above the field, property, or method they are associated with, separated from the member by a newline.
- Multiple attributes should be separated by newlines. This allows for easier adding and removing of attributes, and ensures each attribute is easy to search for.

## Structs & Classes

- Structs are very different from classes:

    - Structs are always passed and returned by value.
    - Assigning a value to a member of a returned struct doesn’t modify the
        original - e.g. `transform.position.x = 10` doesn’t set the transform’s
        position.x to 10; `position` here is a property that returns a `Vector3`
        by value, so this just sets the x parameter of a copy of the original.

- Almost always use a class.

- Consider struct when the type can be treated like other value types - for
    example, if instances of the type are small and commonly short-lived or are
    commonly embedded in other objects. Good examples include Vector3,
    Quaternion and Bounds.

- Note that this guidance may vary from team to team where, for example,
    performance issues might force the use of structs.

## Lambdas vs Named Methods

- If a lambda is non-trivial (e.g. more than a couple of statements, excluding
    declarations), or is reused in multiple places, it should probably be a
    named method.

## IEnumerable vs IList vs IReadOnlyList

- For inputs use the most restrictive collection type possible, for example
    `IReadOnlyCollection` / `IReadOnlyList` / `IEnumerable` as inputs to methods
    when the inputs should be immutable.
- For outputs, if passing ownership of the returned container to the owner,
    prefer `IList` over `IEnumerable`. If not transferring ownership, prefer the
    most restrictive option.
## Extension Methods

- Only use an extension method when the source of the original class is not
    available, or else when changing the source is not feasible.
- Only use an extension method if the functionality being added is a ‘core’
    general feature that would be appropriate to add to the source of the
    original class.
    *   Note - if we have the source to the class being extended, and the
        maintainer of the original class does not want to add the function, prefer not using an extension method.
- Only put extension methods into core libraries that are available
    everywhere - extensions that are only available in some code will become a readability issue.
- Be aware that using extension methods always obfuscates the code, so err on the side of not adding them.
- Extension methods are static and are difficult to test.

## String Data Type

- Use [string interpolation](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/tokens/interpolated.md) to concatenate short strings, as shown in the following code.

```csharp
var displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```

- To append strings in loops, especially when you're working with large amounts of text, use a `System.Text.StringBuilder` object.
- If performance is a concern, `StringBuilder` will be faster for multiple string concatenations.

```csharp
var phrase = "lalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for (var i = 0; i < 10000; i++)
{
    manyPhrases.Append(phrase);
}
//Console.WriteLine("tra" + manyPhrases);
```
- Be aware that chained `operator+` concatenations will be slower and cause significant memory churn.
## Unsigned Data Types

In general, use `int` rather than unsigned types. The use of `int` is common throughout C#, and it is easier to interact with other libraries when you use `int`.

## Arrays

Use the concise syntax when you initialize arrays on the declaration line. In the following example, note that you can't use `var` instead of `string[]`.

```csharp
string[] vowels1 = { "a", "e", "i", "o", "u" };
```

If you use explicit instantiation, you can use `var`.

```csharp
var vowels2 = new string[] { "a", "e", "i", "o", "u" };
```

If you specify an array size, you have to initialize the elements one at a time.

```csharp
var vowels3 = new string[5];
vowels3[0] = "a";
vowels3[1] = "e";
// And so on.
```

## Delegates

Use [`Func<>` and `Action<>`](https://github.com/dotnet/docs/blob/main/docs/standard/delegates-lambdas.md) instead of defining delegate types. In a class, define the delegate method.

```csharp
public static Action<string> ActionExample1 = x => Console.WriteLine($"x is: {x}");

public static Action<string, string> ActionExample2 = (x, y) => 
    Console.WriteLine($"x is: {x}, y is {y}");

public static Func<string, int> FuncExample1 = x => Convert.ToInt32(x);

public static Func<int, int, int> FuncExample2 = (x, y) => x + y;
```

Call the method using the signature defined by the `Func<>` or `Action<>` delegate.

```csharp
ActionExample1("string for x");

ActionExample2("string for x", "string for y");

Console.WriteLine($"The value is {FuncExample1("1")}");

Console.WriteLine($"The sum is {FuncExample2(1, 2)}");
```

If you create instances of a delegate type, use the concise syntax. In a class, define the delegate type and a method that has a matching signature.

```csharp
public delegate void Del(string message);

public static void DelMethod(string str)
{
    Console.WriteLine("DelMethod argument: {0}", str);
}
```

Create an instance of the delegate type and call it. The following declaration shows the condensed syntax.

```csharp
Del exampleDel2 = DelMethod;
exampleDel2("Hey");
```

The following declaration uses the full syntax.

```csharp
Del exampleDel1 = new Del(DelMethod);
exampleDel1("Hey");
```

## `try`-`catch` & `using` Statements In Exception Handling

- Use a [try-catch](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/try-catch.md) statement for most exception handling.

```csharp
static string GetValueFromArray(string[] array, int index)
{
    try
    {
        return array[index];
    }
    catch (System.IndexOutOfRangeException ex)
    {
        Console.WriteLine("Index is out of range: {0}", index);
        throw;
    }
}
```

- Simplify your code by using the C# [using statement](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/using-statement.md). If you have a [try-finally](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/try-finally.md) statement in which the only code in the `finally` block is a call to the <xref:System.IDisposable.Dispose%2A> method, use a `using` statement instead.

  In the following example, the `try`-`finally` statement only calls `Dispose` in the `finally` block.

```csharp
Font font1 = new Font("Arial", 10.0f);
try
{
    byte charset = font1.GdiCharSet;
}
finally
{
    if (font1 != null)
    {
        ((IDisposable)font1).Dispose();
    }
}
```

  You can do the same thing with a `using` statement.

```csharp
using (Font font2 = new Font("Arial", 10.0f))
{
    byte charset2 = font2.GdiCharSet;
}
```

  In C# 8 and later versions, use the new [`using` syntax](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/using-statement.md) that doesn't require braces:

```csharp
using Font font3 = new Font("Arial", 10.0f);
byte charset3 = font3.GdiCharSet;
```

## `&&` & `||` Operators

To avoid exceptions and increase performance by skipping unnecessary comparisons, use [`&&`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-) instead of [`&`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#logical-and-operator-) and [`||`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) instead of [`|`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#logical-or-operator-) when you perform comparisons, as shown in the following example.

```csharp
Console.Write("Enter a dividend: ");
int dividend = Convert.ToInt32(Console.ReadLine());

Console.Write("Enter a divisor: ");
int divisor = Convert.ToInt32(Console.ReadLine());

if ((divisor != 0) && (dividend / divisor > 0))
{
    Console.WriteLine("Quotient: {0}", dividend / divisor);
}
else
{
    Console.WriteLine("Attempted division by 0 ends up here.");
}
```

If the divisor is 0, the second clause in the `if` statement would cause a run-time error. But the && operator short-circuits when the first expression is false. That is, it doesn't evaluate the second expression. The & operator would evaluate both, resulting in a run-time error when `divisor` is 0.

## `new` Operator

- Use one of the concise forms of object instantiation, as shown in the following declarations. The second example shows syntax that is available starting in C# 9.

```csharp
var instance1 = new ExampleClass();
```

```csharp
ExampleClass instance2 = new();
```

The preceding declarations are equivalent to the following declaration.

```csharp
ExampleClass instance2 = new ExampleClass();
```

- Use object initializers including the `new` keyword and the class with opening and closing parentheses to simplify object creation & instantiation.

```csharp
var instance3 = new ExampleClass() 
{ 
    Name = "Desktop", 
    ID = 37414,
    Location = "Redmond", 
    Age = 2.3, 
};
```

- Object Initializer Syntax is fine for ‘plain old data’ types.
- Avoid using this syntax for classes or structs with constructors to prevent nesting within initializers.
- If splitting across multiple lines, indent one block level.
- Trailing commas are encouraged.

## Argument Naming

When the meaning of a function argument is nonobvious, consider one of the following remedies:

*   Consider changing the function signature to replace a `bool` argument with
    an `enum` argument. This will make the argument values self-describing.
*   Replace large or complex nested expressions with named variables.
*   Consider using
    [Named Arguments](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
    to clarify argument meanings at the call site.

Consider the following example:

```c#
// Bad - what are these arguments?
DecimalNumber product = CalculateProduct(values, 7, false, null);
```

versus:

```c#
// Good
ProductOptions options = new ProductOptions();
options.PrecisionDecimals = 7;
options.UseCache = CacheUsage.DontUseCache;
DecimalNumber product = CalculateProduct(values, options, completionDelegate: null);
```

## Static Members

Call [static](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/static.md) members by using the class name: *ClassName.StaticMember*. This practice makes code more readable by making static access clear.  Don't qualify a static member defined in a base class with the name of a derived class.  While that code compiles, the code readability is misleading, and the code may break in the future if you add a static member with the same name to the derived class.

## LINQ Queries

- Use meaningful names for query variables. The following example uses `seattleCustomers` for customers who are located in Seattle.

```csharp
var seattleCustomers = from customer in customers
                       where customer.City == "Seattle"
                       select customer.Name;
```

- Use aliases to make sure that property names of anonymous types are correctly capitalized, using Pascal casing.

```csharp
var localDistributors =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { Customer = customer, Distributor = distributor };
```

- Rename properties when the property names in the result would be ambiguous. For example, if your query returns a customer name and a distributor ID, instead of leaving them as `Name` and `ID` in the result, rename them to clarify that `Name` is the name of a customer, and `ID` is the ID of a distributor.

```csharp
var localDistributors2 =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { CustomerName = customer.Name, DistributorID = distributor.ID };
```

- Use implicit typing in the declaration of query variables and range variables.

```csharp
var seattleCustomers = from customer in customers
                       where customer.City == "Seattle"
                       select customer.Name;
```

- Align query clauses under the [`from`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/from-clause.md) clause, as shown in the previous examples.

- Use [`where`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/where-clause.md) clauses before other query clauses to ensure that later query clauses operate on the reduced, filtered set of data.

```csharp
var seattleCustomers2 = from customer in customers
                        where customer.City == "Seattle"
                        orderby customer.Name
                        select customer;
```

- Use multiple `from` clauses instead of a [`join`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/keywords/join-clause.md) clause to access inner collections. For example, a collection of `Student` objects might each contain a collection of test scores. When the following query is executed, it returns each score that is over 90, along with the last name of the student who received the score.

```csharp
var scoreQuery = from student in students
                 from score in student.Scores!
                 where score > 90
                 select new { Last = student.LastName, score };
```
- In general, prefer single line LINQ calls and imperative code, rather than
    long chains of LINQ. Mixing imperative code and heavily chained LINQ is
    often hard to read.
- Prefer member extension methods over SQL-style LINQ keywords - e.g. prefer
    `myList.Where(x => x > 5)` to `myList where x`.
- Avoid `Container.ForEach(...)` for anything longer than a single statement.

### Return Statements

- Assign eventual return values to a variable then return the variable.

```csharp
    string a = FunctionA();
    return a;
```
- Try to have only 1 return statement at the end of the method.

```csharp
// bad
public bool Decision() 
{ 
    bool decision = DecisionA();

    if ( !decision ) {
        decision = DecisionB();

        if ( !decision ) {
          decision = DecisionC();
        } else {
            return false; //avoid it.
        }
    } else {
      return false; // avoid it.
    } else {
        return false;
    }
 
    return decision; // should be single source of truth
}

// good
public bool Decision() 
{ 
   // TODO: Soahail
 
    return decision; // should be single source of truth
}
```