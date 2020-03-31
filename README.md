# Kotlin-High-Performance
Kotlin optimizations and best practices book

## The disposable pattern
  - Disposable is resources (object) that have close() or dispose() method
  ```
  return try{
bufferedReader.read()
}catch(e: Exception){
 null
 }finally{
 bufferedReader?.close()
 }
 
```

Kotlin has extension function for this:
File().inputStream().bufferedREader().use { it.readLine() }

## Immutability
- State of an object cannot be changed after it is initialized
- We can use those objects safely for key in a map

data class Immutable(val name: String)  - decompiles to final class with private final String name, initialized in constructor, 
overriden hashCode and equals. Data classes also create copy() method.
 * copy method of immutable object allows us tor create new instance with same state or pass parameter.
 
 ## String pool
 
 Creating string with:
 val str = "A"  instead of val str = String("A") puts the string in string pool. Special section of the memory for unique strings.
 
 * intern method
 You can push a string to string pool using .intern() method:
 val a = String("a")
 a.intern()
 
* Comparing by references take less time than comparing by equals()
* StringBuilder (not thread safe)
val cat = "Cat"
cat.plus("Dog") - creates new String

Don't use concatination "a" + "b" use string templates "$a $b" - under the hood it uses StringBuilder
In multithreaded environment use String Buffer.

 ## Functional programming
 * Write in declarative style (concentrate on business logic). Code describes what it does.
 
 - pure functions
 Result of the function depend on parameters, no shared state. Examples kotlin.math
 
 - first-class functions
 Function outside of object. Not a class member. All pure functions should be declared that way.
 
 - higher-order functions
 Functions that take functions as parameters or return type.
 Strategy pattern. -> interface CompressionStrategy: ZipImplementation. Archiver class that holds it - set strategy, get strategy, archive(). This pattern in Kotlin can be implemented on one line.
 
 inline fun File.archive(strategy: () -> File): File = strategy()
 
    - Inline functions - Use Inline with high order functions otherwise function is wrapped inside object.
    
 - function composition
 Avoid code as students.filter(name > "i").filter(age < 19). Use instead:
 
 fun predicate(student) = ageLessThan19() && firstNameStartsWith(J)
 students.filter(::predicate)
 
 - typeclasses
 - Lambdas
 
 Capturing lambdas
  val lambda = { lambda body }
  lambda()
  
  This creates a lot of code in java. so preferrably - don't use it.
  
 - Closures
 - Immutability
 

