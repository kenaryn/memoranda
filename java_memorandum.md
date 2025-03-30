# Installing/updating OpenJDK
```
cd ~/Downloads
curl -fsSL https://download.java.net/java/GA/jdk24/1f9ff9062db4449d8ca828c504ffae90/36/GPL/openjdk-<version>_linux-x64_bin.tar.gz
sudo tar xzf openjdk-<version>_linux-x64_bin.tar.gz -C /usr/lib/jvm
cd /usr/lib/jvm
sudo mv jdk-24 openjdk24
sed -i "set-env JAVA_HOME ‘/usr/lib/jvm/openjdk24/’" $E:HOME/.config/elvish/rc.elv
sed -i "set-env LD_LIBRARY ‘/usr/lib/jvm/openjdk24/lib/server’" $E:HOME/.config/elvish/rc.elv
elvish (to regenerate running_command file from the current interactive shell emulator session)
set paths = [$E:JAVA_HOME/bin $@paths]
java --version
rm -v ~/Downloads/openjdk*.tar.gz
```
*Nota bene*: `xbps-alternatives -l -g jdk` reports only those packages who were installed via the xbps package manager

## Installing minimal runtime
In order to shrink deployment size of the Java applicative layer, run:
`jlink --add-modules java.base --output <minimal_runtime_path>`


### Static class
A static class is NOT instantiated to call its method. At least one static method causes the class it belongs to to be 
static.


### power
`Math.pow(<base>, <exponent>)`
x ^ 0 = 1
x ^ 1 = x   


### float/double precision
- `float`: 32 bits. Mantissa: 23 bits (about 7 decimal digits), exponent: 8 bits, sign: 1 bit
- `double`: 64 bits. Mantissa: 52 (about 16 decimal digits), exponent: 11 bits, sign: 1 bit


### Default value
- Boolean (object): `null`
- boolean (primitive): `false`


### Primitives
All primitives are signed. *e.g.* `byte`'s type range value is between -128 and 127.


### Bitwise operators
They act against all numeric types (*i.e.* `byte`, `short`, `int`, `long`, `char`) and boolean, but do not short-circuit, 
to the opposite of logical operators.


### Math operations / int
Any mathematical operation involving an `int` type or smaller results in `int`. Hence, you may have to cast an operand \
in order to get the code to compile.
On the other end, compound assignment operators have two peculiarities:
- require not a cast.
- right-side operands are grouped whatever the precedence is.

*Nota bene*: assignment operators are evaluated right-to-left.


### Constant
A constant is computed at compile time and guarantee a fixed value.
Prefix the variable with `final` keyword allow you to perform a constant-to-variable assignment and prevent the need to \
proceed with a explicit cast if the value is included in the allowed range of the targeted type variable.


### Reference types
- class
- enum
- record
- array.

### Utilitary method
Make the methods static and call them with unary operator in stead of the resolution scope one. Indeed, the latter 
specifies a method reference acting as a λ-like entity. Hence: `<class>.<static_method>`

### Wrapper types
Each numeric primitive type has a corresponding class that provide useful methods to (for example) perform arithmetic.
- byte: Byte
- char: Character
- short: Short
- int: Integer
- long: Long.

### Array
An array is a data structure of **object** type that stores a fixed-size, ordered collection of elements of the same type.
Those elements are stored in contiguous memory locations.
`Arrays` type provide access to sorting and searching methods.

`ArrayList<String>`'s instance is mutable conversely to `List.of()` or `List.copyOf()`.


### Switch
Use `yield <value>` to yield an arm result in a `switch`'s block as the last statement. *e.g.*
`case 6 -> {
    logger.log(Level.INFO, "User has failed 3 times before succeeding the authentication");
    yield "Welcome, back!";
}`

*Nota bene*: a `return` statement is disallowed in a arm in order to avoid confusion with the `return` encompassing 
switch expression.

- The computational complexity to look up the arm's evaluation is constant (*i.e.* O(1)) as opposed to linear 
(*viz.* O(n)) for a worse case scenario in an if-else statement.
 
- Use total type pattern in stead of the default case to gracefully handle null references (which raise 
`NullPointerException`) and causes the last arm to be flexible by supporting the full hierarchy of the object passed to
the switch mechanism.


## Syntactic sugar
1. Type inference
`var` requires that the variable has a non-null initializer to evaluate the more specialized type possible.
Type inference is not allowed at:
- field level
- method parameter level
- return type level.

<base_class_left_operand> = <derived_class_right_operand>
The initialized variable can now call methods of the base class but not those who belong only to the derived one. 

Use NOT type inference to store λ expressions into variables as they need an explicit target type.


2. Text block
Use Sring class' `stringIndent()` and `translateEscape` methods to remove incidental indentations when reading at runtime
an external input source.


# Memory management
## Call by reference
To shrink the program's memory footprint, replace a primitive integer (*e.g.* `int`) with an `array` (*e.g.* `int[]`),
which is a reference type and call it using `int[0]`.

## Stack
Primitives and references are stored on the **stack**. Each new method call is pushed on a stack's frame holding local
variables (*i.e.* primitives and/or references) of a the method.

## Heap
Objects are stored on the heap. An object is accessed via a reference.


## OOP
1. Class
A class is a blueprint of template for your object. The class defines fields (your data) and methods (to manipulate that
data). \
There is **only one copy of fields (or instance variables) per class**.
Use `static` keyword to declare a class member, **shared by all instances** of the class. You have not to create an object
instance to access the `static` class' members.

- *Nota bene*: an instance variable has its contents initialized in a constructor whereas a class variable's value is 
is computed in a static (*i.e.* class) initializer.
- *Addendum*: keep on an eye on **deferred immutability** of `StableValue` new type introduced in Java 25.


2. Object
An object is an in-memory representation of your class. \
**Every object instance gets a copy of its instance member.**

You need to create an object instance to refer to an instance member when in a `static` context.


3. Record
- Each parameter of a record is implicitly private and immutable (*i.e.* `final`). Hence, no setters for the 
components/fields.
- Records are `final` special classes which extend `java.lang.Record` and act as a mere carrier of data. 
- They are NOT intended to convey domain's abstractions with extensive behaviour.
- They hold built-in methods `toString()`, `equals()`, `hashCode()` and an automatically generated canonical constructor.

*Hint*: write your own compact constructor if you need to transform, validate and/or cleanse data and leave the 
fields' initialization to the canonical one.

**Immutability shall be encouraged as an immutable object can only be in one state and therefore can be shared
freely across multiple threads.**


### User input
In order to avoid buffering issues and other unexpected miseries, favour `Integer.parseInt(scanner.nextLine())` over 
`scanner.nextInt()`.


## Bytecode
Using `lavap -v <filename>`:
- ldc: load a constant.
- sipush: push short. The compiler treats the value as a `short` type.
- new: the compiler initializes a reference.


### Sealed
Whatever is marked as `sealed`, it requires at least one derived class or interface, hence re-opening a hierarchy for
at least one more level of extension.
`non-sealed` keyword basically tells the compiler: "the hierarchy is closed everywhere, except right here."


### Error redirection
Declare into the entry-point `System.setErr(System.out);` to redirect error canal towards the standard output one.


### Lexikon
- API: a suite of pre-defined types.
- Encapsulation: data abstraction (using access modifiers).
- Expression: as opposed to statements, performs computations and return a result. 
- Functional interface: provides target types for λ expressions and method references. Holds a single abstract method.
- Guarded pattern: conditional arm.
- Member: field or method.
- Package: a named group of related types (*i.e.* class, record, enum, interface).
- Programming: forming good abstractions and putting things together to minimize complexity.
- Reference: variable, field (of a class), or component (or a record)
- Static typing: relates to type verification and not type specification.
- Stream: `java.lang.stream` API designed for λ expressions. Lazily computed, potentially unbounded sequence of values.
      A stream can be processed sequentially or in parallel. It is composed of:
        - a source of elements
        - intermediate operations
        - an eager terminal operation which produces a value or a side-effect.
      Each intermediate operation returns a new stream but evaluation triggered only when terminal operation invoked.
- tuple: an immutable data structure of a finite ordered sequence of item. 
- `var`: context sensitive term. Neither a type nor an universal keyword.
