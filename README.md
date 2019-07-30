# Effective Java

#### Third Edition

### by Joshua Block

`Notes by Henry Cooksley`

### 1 Introduction

"This book is not intended to be read from cover to cover: each item stands on its own, more or less."

"This book is not for beginners: it assumes that you are already comfortable with Java."

"Most of the rules in this book derive from a few fundamental principles."

"Clarity and simplicity are of paramount importance."

"The user of a component should never be surprised by its behavior."

"Components should be as small as possible but no smaller."

"Code should be reused rather than copied."

"The dependencies between components should be kept to a minimum."

"Errors should be detected as soon as possible after they are made, ideally at compile time."

"Learning the art of programming, like most other disciplines, consists of first learning the rules and then learning when to break them."

"The language supports four kinds of types: interfaces (including annotations), classes (including enums), arrays, and primitives."

"The first three are known as reference types."

"Class instances and arrays are objects; primitive values are not."

"A class's members consist of its fields, methods, member classes, and member interfaces."

"A method's signature consists of its name and the types of its formal parameters; the signature does not include the method's return type."

### 2 Creating and Destroying Objects

Item 1: Consider static factory methods instead of constructors

"One advantage of static factory methods is that, unlike constructors, they have names."

"A second advantage of static factory methods is that, unlike constructors they are not required to create a new object each time they're invoked."

"A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype of their return type."

"A fourth advantage of static factories is that the class of the returned object can vary from call to call as a function of the input parameters."

"A fifth advantage of static factories is that the class of the returned object need not exist when the class containing the method is written."

"The main limitation of providing only static factory methods is that classes without public or protected constructors cannot be subclassed."

"A second shortcoming of static factory methods is that they are hard for programmers to find."

Item 2: Consider a builder when faced with many constructor parameters

"In short, the telescoping constructor pattern works, but it is hard to write client code when there are many parameters, and harder still to read it."

"Because construction is split across multiple calls, a JavaBean may be in an inconsistent state partway through its construction."

"A related disadvantage is that the JavaBeans pattern precludes the possibility of making a class immutable and requires added effort on the part of the programmer to ensure thread safety."

"The Builder pattern simulates named optional parameters as found in Python and Scala."

"The Builder pattern is well suited to class hierarchies."

"In summary, the Builder pattern is a good choice when designing classes whose constructors or static factories would have more than a handful of parameters, especially if the many of the parameters are optional or of identical type."

Item 3: Enforce the singleton property with a private constructor or an enum type

"Making a class a singleton can make it difficult to test its clients because it's impossible to substitute a mock implementation for a singleton unless it implements an interface that serves as its type."

"This approach may feel a bit unnatural, but a single-element enum type is often the best way to implement a singleton."

Item 4: Enforce noninstantiability with a private constructor

"Attempting to enforce noninstantiability by making a class abstract does not work."

"A default constructor is generated only if a class contains no explicit constructors, so a class can be made noninstantiable by including a private constructor."

Item 5: Prefer dependency injection to hardwiring resources

"Static utility classes and singletons are inappropriate for classes whose behavior is parameterized by an underlying resource."

"A simple pattern that satisfies this requirement is to pass the resource into the constructor when creating a new instance."

Item 6: Avoid creating unnecessary objects

"While String.matches is the easiest way to check if a string matches a regular expression, it's not suitable for repeated use in performance-critical situations."

"Autoboxing blurs but does not erase the distinction between primitive and boxed primitive types."

"The lesson is clear: prefer primitives to boxed primitives, and watch out for unintentional autoboxing."

Item 7: Eliminate obsolete object references

"Nulling out object references should be the exception rather than the norm."

"Generally speaking, whenever a class manages its own memory, the programmer should be alert for memory leaks."

"Another common source of memory leaks is caches."

"A third common source of memory leaks is listeners and other callbacks."

Item 8: Avoid finalizers and cleaners

"Finalizers are unpredictable, often dangerous, and generally unnecessary."

"Cleaners are less dangerous than finalizers, but still unpredictable, slow, and generally unnecessary."

"This means should never do anything time-critical in a finalizer or cleaner."

"As a consequence, you should never depend on a finalizer or cleaner to update persistent state."

"There is a severe performance penalty for using finalizers and cleaners."

"Finalizers have a serious security problem: they open your class up to finalizer attacks."

"Throwing an exception from a constructor should be sufficient to prevent an object from coming into existence; in the presence of finalizers, it is not."

"To protect nonfinal classes from finalizer attacks, write a final finalize method that does nothing."

"Just have your class implement AutoCloseable, and require its clients to invoke the close method on each instance when it is no longer needed, typically using try-with-resources to ensure termination even in the face of exceptions."

Item 9: Prefer try-with-resources to try-finally

### 3 Methods Common to All Objects

Item 10: Obey the general contract when overriding equals

"Each instance of the class is inherently unique."

"There is no need for the class to provide a “logical equality” test."

"A superclass has already overridden equals, and the superclass behavior is appropriate for this class."

"The class is private or package-private, and you are certain that its equals method will never be invoked."

"There is no way to extend an instantiable class and add a value component while preserving the equals contract, unless you're willing to forgo the benefits of object-oriented abstraction."

"Whether or not a class is immutable, do not write an equals method that depends on unreliable resources."

"Use the == operator to check if the argument is a reference to this object."

"Use the instanceof operator to check if the argument has the correct type."

"Cast the argument to the correct type."

"For each “significant” field in the class, check if that field of the argument matches the corresponding field of this object."

"When you are finished writing your equals method, ask yourself three questions: Is it symmetric? Is it transitive? Is it consistent?"

"Always override hashCode when you override equals."

"Don't try to be too clever."

"Don't substitute another type for Object in the equals declaration."

Item 11: Always override hashCode when you override equals

"You must override hashCode in every class that overrides equals."

"The key provision that is violated when you fail to override hashCode is the second one: equal objects must have equal hash codes."

"Do not be tempted to exclude significant fields from the hash code computation to improve performance."

"Don't provide a detailed specification for the value returned by hashCode, so clients can't reasonably depend on it; this gives you the flexibility to change it."

Item 12: Always override toString

"While it isn't as critical as obeying the equals and hashCode contracts, providing a good toString implementation makes your class much more pleasant to use and makes systems using the class easier to debug."

"When practical, the toString method should return all of the interesting information contained in the object, as shown in the phone number example."

"Whether or not you decide to specify the format, you should clearly document your intentions."

"Whether or not you specify the format, provide programmatic access to the information contained in the value returned by toString."

Item 13: Override clone judiciously

"Though the specification doesn't say it, in practice, a class implementing Cloneable is expected to provide a properly functioning public clone method."

"This is the case, for example, for the PhoneNumber class in Item 11, but note that immutable classes should never provide a clone method because it would merely encourage wasteful copying."

"In effect, the clone method functions as a constructor; you must ensure that it does no harm to the original object and that it properly establishes invariants on the clone."

"This is a fundamental problem: like serialization, the Cloneable architecture is incompatible with normal use of final fields referring to mutable objects, except in cases where the mutable objects may be safely shared between an object and its clone."

"Public clone methods should omit the throws clause, as methods that don't throw checked exceptions are easier to use."

"A better approach to object copying is to provide a copy constructor or copy factory."

Item 14: Consider implementing Comparable

"Use of the relational operators < and > in compareTo methods is verbose and error-prone and no longer recommended."

### 4 Classes and Interfaces

