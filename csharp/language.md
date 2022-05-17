# Language Guidelines

The following sections describe practices that the C# team follows to prepare code examples and samples.

## String Data Type

- Use [string interpolation](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/tokens/interpolated.md) to concatenate short strings, as shown in the following code.

```csharp
var displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```

- To append strings in loops, especially when you're working with large amounts of text, use a `System.Text.StringBuilder` object.

```csharp
var phrase = "lalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for (var i = 0; i < 10000; i++)
{
    manyPhrases.Append(phrase);
}
//Console.WriteLine("tra" + manyPhrases);
```

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

- Avoid the use of `var` in place of [dynamic](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/builtin-types/reference-types.md). Use `dynamic` when you want run-time type inference. For more information, see [Using type dynamic (C# Programming Guide)](https://github.com/dotnet/docs/blob/main/docs/csharp/programming-guide/types/using-type-dynamic.md).

- Use implicit typing to determine the type of the loop variable in [`for`](../../language-reference/statements/iteration-statements.md#the-for-statement) loops.

  The following example uses implicit typing in a `for` statement.

```csharp
for (var i = 1; i < 42; i++)
{
    Console.WriteLine($"Universal answer is not {i}.");
}
```

- Don't use implicit typing to determine the type of the loop variable in [`foreach`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/statements/iteration-statements.md#the-foreach-statement) loops.

  The following example uses explicit typing in a `foreach` statement.

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
Console.WriteLine();
```

  > [!NOTE]
  > Be careful not to accidentally change a type of an element of the iterable collection. For example, it is easy to switch from <xref:System.Linq.IQueryable?displayProperty=nameWithType> to <xref:System.Collections.IEnumerable?displayProperty=nameWithType> in a `foreach` statement, which changes the execution of a query.

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

To avoid exceptions and increase performance by skipping unnecessary comparisons, use [`&&`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-) instead of [`&`]https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#logical-and-operator-) and [`||`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) instead of [`|`](https://github.com/dotnet/docs/blob/main/docs/csharp/language-reference/operators/boolean-logical-operators.md#logical-or-operator-) when you perform comparisons, as shown in the following example.

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

- Use object initializers to simplify object creation, as shown in the following example.

```csharp
var instance3 = new ExampleClass { Name = "Desktop", ID = 37414,
Location = "Redmond", Age = 2.3 };
```

The following example sets the same properties as the preceding example but doesn't use initializers.

```csharp
var instance4 = new ExampleClass();
instance4.Name = "Desktop";
instance4.ID = 37414;
instance4.Location = "Redmond";
instance4.Age = 2.3;
```

## Event Handling

If you're defining an event handler that you don't need to remove later, use a lambda expression.

```csharp
public Form2()
{
    this.Click += (s, e) =>
        {
            MessageBox.Show(
                ((MouseEventArgs)e).Location.ToString());
        };
}
```

The lambda expression shortens the following traditional definition.

```csharp
public Form1()
{
    this.Click += new EventHandler(Form1_Click);
}

void Form1_Click(object? sender, EventArgs e)
{
    MessageBox.Show(((MouseEventArgs)e).Location.ToString());
}
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