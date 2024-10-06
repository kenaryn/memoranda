# Functional Programming (FP)
- Data are never modified, but only *transformed* from one format to another.

- FP applications only consist of *immutable data* and *pure functions* (*i.e.* neither read or write from/to outside world, pure fonctions have no side effects, no hidden states issues, etc.).
No pure function can modify a state outside its scope!   
  
- An impure function can return a different value each time it is called (*e.g.* prompt a user for his name).

1. A series of transformer functions
2. Data flows in only one direction
3. Data are never modified
4. Pure functions have one entrance and one exit (*i.e.* no hidden states nor outside-scoped values altered in the universe).


## Immutability
Immutable objects are automatically **thread-safe** and have no race-conditions issues.
Also, they can never exist in unknown or undesirable state because of an exception.


### Object companion / apply
When you call `Foo(...)` with `Foo` being a class, you actually call the apply method on the companion object of `Foo`, 
hence your code looks like:

```scala
class Foo
object Foo { // companion object of class Foo
  def apply(...): Foo = ???
}
```

*e.g.* `BigInt(2).pow(1024)`
*Nota bene* `BigInt(2)` is under the hood `BigInt.apply(2)`. "apply" means "this is how I'm used as a function" (like __call__ in python)

On a side note, you can inform the runtime to call extendions methods or implicit conversions to apply BigInt methods on a Int value
without explicitly creating a BigInt instance.


### Shadowing
Shadowing a variable is permitted, hence you can declare two variables'name identical in the same lexical scope.


### for comprehension loop
A definition in a for loop becomes a `val` automatically, hence, requires neither keyword `val` nor `var`. *e.g*

```scala
for
    i <- 1 to 3
    from = 4 - i  // no `val` nor `var`
    j <- 1 to 3
do
  [...]
```

`for i <- expr do`  
There is no `val` or `var` before the variable. Its type is the element type of the collection, 
*e.g.* for i <- `1 to n` (`i` will be of `Range` type). The scope variable extends until the end of the loop.


### Eta expansion
```scala
def isLessThan(x: Short, y: Short): Boolean = x < y
def isEven(x: Short): Boolean = x % 2 == 0
val methods = List(isLessThan, isEven)

def isLessThan(x: Short, y: Short): Boolean
def isEven(x: Short): Boolean
val methods: List[((Short, Short) => Boolean) | (Short => Boolean)] = List(rs$line$1$$$Lambda/0x0000007e01589000@1fec9d33, rs$line$1$$$Lambda/0x0000007e01588c00@372f7bc)

scala> methods(0)
val res0: ((Short, Short) => Boolean) | (Short => Boolean) = Lambda/0x0000007e01589000@1fec9d33

scala> methods(1)
val res1: ((Short, Short) => Boolean) | (Short => Boolean) = Lambda/0x0000007e01588c00@372f7bc
```

Scala cannot determine which function type it is because the types are widened to type union.
You can not pattern match functions in general, not even haskell allows that. 

**Lists** are homogenous, which allows for helpful combinators like `map` and `filter` to apply to every element of the collection. 

**Tuples** are heterogeneous, so the types in it are all tracked separately, which will allow you to call method(0)(4,1) and method(1)(3) respectively.


### Parenthesis and square brackets notations
`List()` creates actually a List object. 
*e.g.* `List(("test 1", 114))` creates a tuple object
`List[]` declares the parameters' types.
*e.g.* `List[(String, Int, Int)]` where parentheses are part of the type signature for a tuple type.


### Method / function
A method only refers to a member of a class, trait or object.

A function is declared at top-level scope outside a class, whereas nested functions are declared inside a block.
With a recursive function, you must specify the return type.


### CLI syntax
scala-cli [command] [scala_cli_options | input]... -- [program_arguments]...


### Eager / lazy evaluation
A value is set whether or not you use it.
Use `lazy val <name>` to defer initialization until it is accessed for the first time.
Useful to delay costly initialization statements or deal with circular dependencies.

A function's computation is deferred until you invoke it.

`val words1`     : evaluated as soon as `words1` is defined
`lazy val words2`: evaluated the first time `words2` is used
`def words3`     : evaluated every time `words3` is used.


### Recursivity
```scala
def sum(args: Int*): Int =
    var result = 0
    for arg <- args do result += arg
    result

def recursiveSum(args: Int*): Int =
    if args.length == 0 then 0
    else args.head + recursiveSum(args.tail*)
``` 

The recursive one is probably slower because by default varargs are `Arrays` and doing a `head` + `tail` on `Arrays` is very slow.
If rather you used a `List` then both would be very equivalent.

Ideally, you would not use any of those, but rather a more high-level solution like `sum`, `foldLeft`, or `cats combineAll`.

But, just to answer, between recursion and loops, we usually recommend newcomers to stick with recursion so they get used **to thinking immutably**. After that, it is a matter of personal preference and code style.


## Exceptions
Could be an object of type either 
- `Failure`, containing the exception that caused the computation to fail;
- `Success`, holding the computation's result.


### try / catch
`try / catch` statement handles exceptions.
`try / finally` takes some action (*e.g.* cleanup) whether or not an exception has occurred.


### Property-based testing
Because the output of a pure function depends only of its inputs, you can define some properties to those said functions.
Hence, you can attack them with a vast range of inputs, which is commonly known as property-based testing. 
See scalacheck.org


### Function signature
If a method is pure, it uses `Either`.
If a method is effectful, it uses `ZIO`.


### Collections
Whenever I need to iterate over a collection, either use a functional method built into
Scala collection classes or recursion.


### Algebra
- Set: built-in and custom Scala types (*i.e.* nouns)
- Operations: pure functions (*i.e.* verbs)
"An algebra lets us talk about the objects and the operations abstractly, and to consider the laws that these operations obey as they operate on the underlying set." - Daniel Eklund

- Algebraic equations are predictable, implying that algorithms based on functional programming are **deterministic**.


### Arithmetic operations
Arithmetic operations promote numeric type to `Int` level. Their result will also be an `Int`. You can convert back to a less-wide memory consuming memory type, like `Short`: `<int>.toShort`.


# Update as you copy
To use an object as a base to another object and never mutate a state, use `case class <object_name>` expression.
It grants you the `copy` method, immutability and pattern matching capabilities.


# Algebraic data types (ADT)
 The "algebra" means "sums" and "products":

  - "Sum" is alternation (`A | B`, meaning A *XOR* B). *e.g.* discriminated union, variant, sum type. 
  - "Product" is combination (`A B`, meaning A *AND* B together). *e.g.* class, enum, struct.