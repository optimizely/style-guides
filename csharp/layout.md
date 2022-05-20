# Layout Conventions

Good layout uses formatting to emphasize the structure of your code and to make the code easier to read. 

## Organization

-  Modifiers occur in the following order: `public protected internal private
    new abstract virtual override sealed static readonly extern unsafe volatile
    async`.
-  Namespace `using` declarations go at the top, before any namespaces. `using` import order is alphabetical, apart from `System` imports which always go first.
- In general, namespaces should be no more than 2 levels deep.
- Prefer a flat file and namespace structure where possible.
- Use file scoped namespaces like `namespace X.Y.Z;` (note the semicolon and lack of braces). 
```csharp
namespace X.Y.Z;

using System;

class X
{
}
```
- Don't force file/folder layout to match namespaces.
- For shared library/module code, use namespaces. For leaf 'application' code.
- New top-level namespace names must be globally unique and recognizable.
- File level namespaces
-  Class member ordering:
    -  Group class members in the following order:
        -  Nested classes, enums, delegates and events.
        -  Static, const and readonly fields.
        -  Fields and properties.
        -  Constructors and finalizers.
        -  Methods.
    -  Within each group, elements should be in the following order:
        -  Public.
        -  Internal.
        -  Protected internal.
        -  Protected.
        -  Private.
    -  Where possible, group interface implementations together.

## Whitespace Rules

-  A maximum of one statement per line.
-  A maximum of one declaration per line.
-  A maximum of one assignment per statement.
-  Indentation of 2 spaces, no tabs.
-  Column limit: 100.
-  No line break before opening brace.
-  No line break between closing brace and `else`.
-  Braces used even when optional.
-  Space after `if`/`for`/`while` etc., and after commas.
-  No space after an opening parenthesis or before a closing parenthesis.
-  No space between a unary operator and its operand. One space between the
    operator and each operand of all other operators.
-  Line wrapping developed from Google C++ style guidelines, with minor
    modifications for compatibility with Microsoft's C# formatting tools:
    -  In general, line continuations are indented 4 spaces.
    -  Line breaks with braces (e.g. list initializers, lambdas, object
        initializers, etc) do not count as continuations.
    -  For function definitions and calls, if the arguments do not all fit on
        one line they should be broken up onto multiple lines, with each
        subsequent line aligned with the first argument. If there is not enough
        room for this, arguments may instead be placed on subsequent lines with
        a four space indent. The code example below illustrates this.


```csharp
// `using` goes at the top, outside the namespace.
using System;

// Namespaces are PascalCase and file scoped (unless unsupported).
namespace MyNamespace;                           

// Interfaces start with 'I'
public interface IMyInterface
{
  // Methods are PascalCase and space after comma.
  public int Calculate(float value, float exp);
}

// Enumerations are PascalCase.
public enum MyEnum                               
{
  // Enumerators are PascalCase and have explict backing integer declared
  // ensuring a default 0 value (make it your "unset" value when in doubt)                                    
  No = 0,
  // Prefer trailing commas in declarations and initializers
  Yes = 1,                                        
}

// Classes are PascalCase.
public class MyClass
{
  // Public member variables are PascalCase
  public int Foo = 0;

  // Field initializers are encouraged
  public bool NoCounting = false;     

  private class Results {
    public int NumNegativeResults = 0;
    public int NumPositiveResults = 0;
  }
  
  private int[] _someTable =                     
  {
    // Container initializers use a 2 space indent
    2, 3, 4,
  }
  
  // Private member variables are _camelCase.
  private Results _results;
  
  // const does not affect naming convention.
  private const int _bar = 100;

  public static int NumTimesCalled = 0;

  public MyClass() 
  {
    _results = new Results() 
    {
      // Object initializers use a 2 space indent.
      NumNegativeResults = 1,
      NumPositiveResults = 1,
    };
  }

  public int CalculateValue(int mulNumber)  
  {
    // Local variables are camelCase.
    var resultValue = Foo * mulNumber;    

    NumTimesCalled++;

    Foo += _bar;

    // No space after unary operator and space after 'if'.
    if (!NoCounting)                             
    {
      
      // Spaces around comparison operator
      if (resultValue < 0)                       
      {// Curly braces are always used even when optional and
        _results.NumNegativeResults++;
      } 
      // No newline between brace and else.
      else if (resultValue > 0)                
      {
        _results.NumPositiveResults++;
      }
    }

    return resultValue;
  }

  public void ExpressionBodies() 
  {
    // For simple lambdas, fit on one line if possible, no brackets or braces required.
    Func<int, int> increment = x => x + 1;

    // Closing brace aligns with first character on line that includes the opening brace.
    Func<int, int, long> difference1 = (x, y) =>
    {
      long diff = (long)x - y;
      return diff >= 0 ? diff : -diff;
    };

    // If defining after a continuation line break, indent the whole body.
    Func<int, int, long> difference2 =
      (x, y) => {
        long diff = (long)x - y;
        return diff >= 0 ? diff : -diff;
      };

    // Inline lambda arguments also follow these rules. Prefer a leading newline before
    // groups of arguments if they include lambdas.
    CallWithDelegate(
        (x, y) => {
          long diff = (long)x - y;
          return diff >= 0 ? diff : -diff;
        });
  }

  // Empty blocks may be concise.
  void DoNothing() {}                             

  // If possible, wrap arguments by aligning newlines with the first argument.
  void AVeryLongFunctionNameThatCausesLineWrappingProblems(int longArgumentName,
                                                           int p1, int p2) {}

  // If aligning argument lines with the first argument doesn't fit, or is difficult to
  // read, wrap all arguments on new lines with a 4 space indent.
  void AnotherLongFunctionNameThatCausesLineWrappingProblems(
      int longArgumentName, int longArgumentName2, int longArgumentName3) {}

  void CallingLongFunctionName() 
  {    
    int veryLongArgumentName = 1234;
    int shortArg = 1;

    // If possible, wrap arguments by aligning newlines with the first argument.
    AnotherLongFunctionNameThatCausesLineWrappingProblems(shortArg, shortArg,
                                                          veryLongArgumentName);

    // If aligning argument lines with the first argument doesn't fit, or is difficult to
    // read, wrap all arguments on new lines with a 4 space indent.
    AnotherLongFunctionNameThatCausesLineWrappingProblems(
        veryLongArgumentName, veryLongArgumentName, veryLongArgumentName);
  }
}
```

- Use the team's IDE settings (smart indenting, four-character indents, tabs saved as spaces).
- If continuation lines are not indented automatically, indent them one tab stop (four spaces).
- Add at least one blank line between method definitions and property definitions.
- Use parentheses to make clauses in an expression apparent, as shown in the following code.

```csharp
if ((val1 > val2) && (val1 > val3))
{
    // Take appropriate action.
}
```