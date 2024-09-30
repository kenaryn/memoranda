# FP
- Data are never modified, but only *transformed* from one format to another.
- FP applications only consist of *immutable data* and *pure functions* (*i.e.* neither read or write from/to outside world, pure fonctions have no side effects).


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

### for loop
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

