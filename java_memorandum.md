### Static class
A static class is NOT instantiated to call its method. At least one static method causes the class it belongs to to be static.

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
Prefix the variable with `final` keyword allow you to perform a constant-to-variable assigment et prevent the need to \
proceed with a explicit cast if the value is included in the allowed range of the targeted type variable.


### Package
A package is a group of related types, such as classes.

### API
An API is a suite of pre-defined types.

### Reference types
- class
- enum
- record 
- array.

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
### Class
A class is a blueprint of template for your object. The class defines fields (your data) and methods (to manipulate that
data).

### Object
An object is your in-memory representation of your class.