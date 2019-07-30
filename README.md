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

Item 15: Minimize the accessibility of classes and members

"The rule of thumb is simple: make each class or member as inaccessible as possible."

"Instance fields of public classes should rarely be public."

"Also, you give up the ability to take any action when the field is modified, so classes with public mutable fields are not generally thread-safe."

"Note that a nonzero-length array is always mutable, so it is wrong for a class to have a public static final array field, or an accessor that returns such a field."

Item 16: In public classes, use accessor methods, not public fields

"Certainly, the hard-liners are correct when it comes to public classes: if a class is accessible outside its package, provide accessor methods to preserve the flexibility to change the class's internal representation."

"However, if a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields - assuming they do an adequate job of describing the abstraction provided by the class."

Item 17: Minimize mutability

"Don't provide methods that modify the object's state (known as mutators)."

"Ensure that the class can't be extended."

"Make all fields final."

"Make all fields private."

"Ensure exclusive access to any mutable components."

"Immutable objects are simple."

"Immutable objects are inherently thread-safe; they require no synchronization."

"Since no thread can ever observe any effect of another thread on an immutable object, immutable objects can be shared freely."

"Not only can you share immutable objects, but they can share their internals."

"Immutable objects make great building blocks for other objects, whether mutable or immutable."

"Immutable objects provide failure atomicity for free."

"The major disadvantage of immutable classes is that they require a separate object for each distinct value."

"Classes should be immutable unless there's a very good reason to make them mutable."

"If a class cannot be made immutable, limit its mutability as much as possible."

"Combining the advice of this item with that of Item 15, your natural inclination should be to declare every field private final unless there's a good reason to do otherwise."

"Constructors should create fully initialized objects with all of their invariants established."

Item 18: Favor composition over inheritance

"Unlike method invocation, inheritance violates encapsulation."

Item 19: Design and document for inheritance or else prohibit it

"In other words, the class must document its self-use of overridable methods."

"To allow programmers to write efficient subclasses without undue pain, a class may have to provide hooks into its internal workings in the form of judiciously chosen protected methods or, in rare instances, protected fields."

"Note: If ListIterator.remove requires linear time, this implementation requires quadratic time."

"The only way to test a class designed for inheritance is to write subclasses."

"Therefore, you must test your class by writing subclasses before you release it."

"Constructors must not invoke overridable methods, directly or indirectly."

"If you do decide to implement either Cloneable or Serializable in a class that is designed for inheritance, you should be aware that because the clone and readObject methods behave a lot like constructors, a similar restriction applies: neither clone nor readObject may invoke an overridable method, directly or indirectly."

"By now it should be apparent that designing a class for inheritance requires great effort and places substantial limitations on the class."

"The best solution to this problem is to prohibit subclassing in classes that are not designed and documented to be safely subclassed."

Item 20: Prefer interfaces to abstract classes

"Existing classes can easily be retrofitted to implement a new interface."

"Interfaces are ideal for defining mixins."

"Interfaces allow for the construction of nonhierarchical type frameworks."

"Interfaces enable safe, powerful functionality enhancements via the wrapper class idiom."

"For brevity's sake, the documentation comments were omitted from the previous example, but good documentation is absolutely essential in a skeletal implementation, whether it consists of default methods on an interface or a separate abstract class."

Item 21: Design interfaces for posterity

"But it is not always possible to write a default method that maintains all invariants of every conceivable implementation."

"In the presence of default methods, existing implementations of an interface may compile without error or warning but fail at runtime."

"Even though default methods are now a part of the Java platform, it is still of the utmost importance to design interfaces with great care."

"While it may be possible to correct some interface flaws after an interface is released, you cannot count on it."

Item 22: Use interfaces only to define types

"The constant interface pattern is a poor use of interfaces."

Item 23: Prefer class hierarchies to tagged classes

"In short, tagged classes are verbose, error-prone, and inefficient."

"A tagged class is just a pallid imitation of a class hierarchy."

Item 24: Favor static member classes over nonstatic

"If you declare a member class that does not require access to an enclosing instance, always put static modifier in its declaration, making it a static rather than a nonstatic member class."

Item 25: Limit source files to a single top-level class

"The lesson is clear: Never put multiple top-level classes or interfaces in a single source file."

### 5 Generics