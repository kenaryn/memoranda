# Functional Programming (FP)
- Data are never modified, but only *transformed* from one format to another.

- FP applications only consist of *immutable data* and *pure functions* (*i.e.* neither read or write from/to outside world.

## Pure function
- A pure fonctions cause no semantically observable side effects nor output, no hidden states issues, etc.).
It can not read or modify a state outside its lexical scope (*viz.* outside world).
- A pure function is total, *i.e.* always return a answer for any input and never throws an exception.

*NB*: a hidden value is a variable accessed by a back door.

1. A series of transformer functions
2. Data flows in only one direction
3. Data are never modified
4. Pure functions have one entrance and one exit (*i.e.* no hidden states nor outside-scoped values altered in the universe).


### Pure function's universe
- Its input received through its front door
- Its algorithm
- The output it produces.


## Error-handling
- Use `Option` data type when you care not about error messages, because you know what is going to be.
- Use `Either` or `Try` when exceptions can occur, such as accessing internet resources, DB and files.


## Scope
A top-level definition is scoped to a special invisible object, which matters for *opaque types*.
Hence, a `foo()` invocation is `special$object.foo()` (the actual name is `finalename$package`, but that is an implementation detail).


## REPL
Each line is evaluated as a different compilation unit so what may look like a double definition, which is illegal behaviour, is in stead evaluated as a shadowing in a inner scope and thereforce, evaluates successfully to a result.
  
## Impure / effectul function
- An impure function can return a different value each time it is called (*e.g.* prompt a user for his name).

1. Read/write hidden inputs (*i.e.* variables not explicitly passed into the function's front door as input parameters)
2. Mutate given parameters
3. Mutate parameters outside the function local's cope
4. Perform some sort of I/O with outside world.

Either has no input parameters or return `Unit` type.


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
*Nota bene* `BigInt(2)` is under the hood `BigInt.apply(2)`. "apply" means "this is how I'm used as a function" 
(like __call__ in python)

On a side note, you can inform the runtime to call extensions methods or implicit conversions to apply `BigInt` methods 
on a `Int` value without explicitly creating a `BigInt` instance.


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
val methods: List[((Short, Short) => Boolean) | (Short => Boolean)] = 
  List(rs$line$1$$$Lambda/0x0000007e01589000@1fec9d33, rs$line$1$$$Lambda/0x0000007e01588c00@372f7bc)

scala> methods(0)
val res0: ((Short, Short) => Boolean) | (Short => Boolean) = Lambda/0x0000007e01589000@1fec9d33

scala> methods(1)
val res1: ((Short, Short) => Boolean) | (Short => Boolean) = Lambda/0x0000007e01588c00@372f7bc
```

Scala cannot determine which function type it is because the types are widened to type union.
You can not pattern match functions in general, not even haskell allows that. 

**Lists** are homogenous, which allows for helpful combinators like `map` and `filter` to apply to every element of the collection. 

**Tuples** are heterogeneous, so the types in it are all tracked separately, which will allow you to call `method(0)(4,1)` \
and `method(1)(3)` respectively.


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
scala-cli <command> [scala_cli_options / input]... -- <program_arguments>...


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

But, just to answer, between recursion and loops, we usually recommend newcomers to stick with recursion so they get \
used **to thinking immutably**. After that, it is a matter of personal preference and code style.


## Exceptions
Could be an object of type either 
- `Failure`, containing the exception that caused the computation to fail;
- `Success`, holding the computation's result.


An exception returns the special type `Nothing`, which resolves any type because it is a subtype of everything.
It is called a **bottom type**.

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
"An algebra lets us talk about the objects and the operations abstractly, and to consider the laws that these operations \
obey as they operate on the underlying set." - Daniel Eklund

- Algebraic equations are predictable, implying that algorithms based on functional programming are **deterministic**.


### Arithmetic operations
Arithmetic operations promote numeric type to `Int` level. Their result will also be an `Int`. You can convert back to \
a less-wide memory consuming memory type, like `Short`: `<int>.toShort`.


## Update as you copy / case classes
In FP, you create new objects with updated fields based on existing objects.
To use an object as a base to another object and never mutate a state, use `case class <object_name>` expression.
Case classes use `val` fields by default.

It grants you the following benefits:
- `copy` method (useful to clone an object)
- immutability
- pattern matching capabilities
- `apply` method (hence, no need to use `new` keyword)
- `unapply` method (the class implementing it is called an **extractor**, which enable match/case expressions)
- *accessor* methods
- `equals` and `hashCode` methods (can perform `==` comparison between objects)
- `toString` method (useful for debugging).


# Algebraic data types (ADT)
 The "algebra" means "sums" and "products":

  - "Sum" is alternation (`A | B`, meaning A *XOR* B). *e.g.* discriminated union, variant, sum type. 
  - "Product" is combination (`A B`, meaning A *AND* B together). *e.g.* class, enum, struct.


### Pipelines
1. Unix pipeline: `who | cut -d ' ' -f1 | uniq | wc -l | tr -d ' '`

2. Bash script
```bash
WHO=`who`
RES1=`echo $WHO | cut -d ' ' -f1`
RES2=`echo $RES1 | uniq`
RES3=`echo $RES2 | wc -l`
RES4=`echo $RES3 | tr -d ' '`
echo $RES4
```

3. First pass of a Scala program
```scala
val who: Seq[String] = getUsers()  // impure function
val res1 = cut(who, " ", 1)
val res2 = uniq(res1)
val res3 = countLines(res2)
val res4 = trim(res3)
println(res4)                      // statement
```

4. Combining expressions (thanks to referential transparency feature of pure functions): combinator
Intermediate values are removed and their right-side expression are passed as argument to ther pure functions.
`val nbUsers = println(trim(countLines(uniq(cut(getUsers, " ", 1)))))`

5. Scala **combinator** final form (or **functional composition**): a chain of functions applied to some data source.
```scala
val numUsers = who.cut(delimiter= " ", field=1)
               .uniq
               .wc(lines = true)
               .tr(find=" ", replace="")
```


### Single modules in Scala
- Trait with Object
- Singleton Object: an object is a singleton, meaning that there is exactly *one instance* of it.
- Top-level definition


### Terminology
- `def` keyword means *method*
- `=>` thick arrow means *function*


### Currying
`2 + 3` is semantically equivalent to the `+` method on the `2` object then passing the `3` object as its argument.


### Predef / standard library
`Predef` is a bunch of types and functions that need not a import.


## ZIO
### ZIO types
Type aliases to simplify:
- `ZIO[Any, Nothing, Int]` == `UIO[Int]`
- `ZIO[Any, Throwable, Int]` == `Task[Int]`
- `ZIO[Any, CustomError, Int]` == `IO[CustomError, Int]`

*NB*: [R, E, A]::=  R: environment type (with possible dependencies). E: error type. A: success type.

### ZIO conceptual model
`ZIO[Connection, String, Int]` == `f(Connection) => Either[String, Int]`


### Benchmark
Comparison between `toInt` and `toIntOption` which consumes the object without needing to handle an exception:
```scala
val valid = "123456789"
val invalid = "123456asd"

def benchmark[T](times: Int)(f: () => T): Long = {
  var i = times
  val start = System.currentTimeMillis
  while (i >= 0) {
    try{
      f()
    } catch {case _ => ()}
    i = i - 1
  }
  System.currentTimeMillis - start
}

println(benchmark(1000000)(() => valid.toInt))          // 59
println(benchmark(1000000)(() => valid.toIntOption))    // 78
println(benchmark(1000000)(() => invalid.toInt))        // 3039
println(benchmark(1000000)(() => invalid.toIntOption))  // 74
```

### Lexikon
- Composable: combine small proven parts to create a larger rock-solid system.


### Nothing / exception
`Nothing` is a bottom type, which means a subtype of all other types.