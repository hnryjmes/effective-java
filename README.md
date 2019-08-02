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

Item 26: Don't use raw types

"If you use raw types, you lose all the safety and expressiveness benefits of generics."

"As a consequence, you lose type safety if you use a raw type such as List, but not if you use a parameterized type such as List<Object>."

"You can put any element into a collection with a raw type, easily corrupting the collection's type invariant; you can't put any element (other than null) into a Collection<?>."

"You must use raw types in class literals."

"This is the preferred way to use the instanceof operator with generic types."

Item 27: Eliminate unchecked warnings

"Eliminate every unchecked warning that you can."

"If you can't eliminate a warning, but you can prove that the code that provoked the warning is typesafe, then (and only then) suppress the warning with an @SuppressWarnings("unchecked") annotation."

"Always use the SuppressWarnings annotation on the smallest scope possible."

"Every time you use a @SuppressWarnings("unchecked") annotation, add a comment saying why it is safe to do so."

Item 28: Prefer lists to arrays

Item 29: Favor generic types

Item 30: Favor generic methods

"The type parameter list, which declares the type parameters, goes between a method's modifiers and its return type."

Item 31: Use bounded wildcards to increase API flexibility

"For maximum flexibility, use wildcard types on input parameters that represent producers or consumers."

"PECS stands for producer-extends, consumer-super."

"Do not use bounded wildcard types as return types."

"If the user of a class has to think about wildcard types, there is probably something wrong with its API."

"Comparables are always consumers, so you should generally use Comparable<? super T> in preference to Comparable<T>."

"The same is true of comparators; therefore, you should generally use Comparator<? super T> in preference to Comparator<T>."

"As a rule, if a type parameter appears only once in a method declaration, replace it with a wildcard."

Item 32: Combine generics and varargs judiciously

"This cast fails, demonstrating that type safety has been compromised, and it is unsafe to store a value in a generic varargs array parameter."

"In essence, the SafeVarargs annotation constitutes a promise by the author of a method that is typesafe."

"This example is meant to drive home the point that it is unsafe to give another method access to a generic varargs parameter array, with two exceptions: it is safe to pass the array to another varargs method that is correctly annotated with @SafeVarargs, and it is safe to pass the array to a non-varargs method that merely computes some function of the contents of the array."

"The rule for deciding when to use the SafeVarargs annotation is simple: Use @SafeVarargs on every method wth a varargs parameter of a generic or parameterized type, so its users won't be burdened by needless and confusing compiler warnings."

Item 33: Consider typesafe heterogeneous containers

### 6 Enums and Annotations

Item 34: Use enums instead of int constants

"To associate data with enum constants, declare instance fields and write a constructor that takes the data and stores it in the fields."

"Switches on enums are good for augmenting enum types with constant-specific behavior."

"Use enums any time you need a set of constants whose members are known at compile time."

"It is not necessary that the set of constants in an enum type stay fixed for all time."

Item 35: Use instance fields instead of ordinals

"Never derive a value associated with an enum from its ordinal; store it in an instance field instead."

Item 36: Use EnumSet instead of bit fields

"In summary, just because an enumerated type will be used in sets, there is no reason to represent it with bit fields."

Item 37: Use EnumMap instead of ordinal indexing

"In summary, it is rarely appropriately to use ordinals to index into arrays: use EnumMap instead."

Item 38: Emulate extensible enums with interfaces

"In summary, while you cannot write an extensible enum type, you can emulate it by writing an interface to accompany a basic enum type that implements the interface."

Item 39: Prefer annotations to naming patterns

"There is simply no reason to use naming patterns when you can use annotations instead."

"But all programmers should use the predefined annotation types that Java provides."

Item 40: Consistently use the Override annotation

"Therefore, you should use the Override annotation on every method declaration that you believe to override a superclass declaration."

Item 41: Use marker interfaces to define types

"First and foremost, marker interfaces define a type that is implemented by instances of the marked class; marker annotations do not."

"Another advantage of marker interfaces over marker annotations is that they can be targeted more precisely."

"The chief advantage of marker annotations over marker interfaces is that they are part of the larger annotation facility."

"If you find yourself writing a marker annotation type whose target is ElementType.TYPE, take the time to figure out whether it really should be an annotation type or whether a marker interface would be more appropriate."

### 7 Lambdas and Streams

Item 42: Prefer lambdas to anonymous classes

"Omit the types of all lambda parameters unless their presence makes your program clearer."

"Unlike methods and classes, lambdas lack names and documentation; if a computation isn't self-explanatory, or exceeds a few lines, don't put it in a lambda."

"Therefore, you should rarely, if ever, serialize a lambda."

"Don't use anonymous classes for function objects unless you have to create instances of types that aren't functional interfaces."

Item 43: Prefer method references to lambdas

"Where method references are shorter and clearer, use them; where they aren't, stick with lambdas."

Item 44: Favor the use of standard functional interfaces

"If one of standard functional interfaces does the job, you should generally use it in preference to a purpose-built functional interface."

"Don't be tempted to use basic functional interfaces with boxed primitives instead of primitive functional interfaces."

"Always annotate your functional interfaces with the @FunctionalInterface annotation."

Item 45: Use streams judiciously

"Overusing streams makes programs hard to read and maintain."

"In the absence of explicit types, careful naming of lambda parameters is essential to the readability of stream pipelines."

"Using helper methods is even more important for readability in stream pipelines than in iterative code because pipelines lack explicit type information and named temporary variables."

"You could fix the program by using a cast to force the invocation of the correct overloading [...] but ideally you should refrain from using streams to process char values."

"So refactor existing code to use streams and use them in new code only where it makes sense to do so."

"If you're not sure whether a task is better served by streams or iteration, try both and see which works better."

Item 46: Prefer side-effect-free functions in streams

"The forEach operation should be used only to report the result of a stream computation, not to perform the computation."

"It is customary and wise to statically import all members of Collectors because it makes stream pipelines more readable."

"The same functionality is available directly on Stream, via the count method, so there is never a reason to say collect(counting())."

Item 47: Prefer Collection to Stream as a return type

"Therefore, Collection or an appropriate subtype is generally the best return type for a public, sequence-returning method."

"But do not store a large sequence in memory just to return it as a collection."

Item 48: Use caution when making streams parallel

"Even under the best of circumstances, parallelizing a pipeline is unlikely to increase its performance if the source if from Stream.iterate, or the intermediate operation limit is used."

"Do not parallelize stream pipelines indiscriminately."

"As a rule, performance gains from parallelism are best on streams over ArrayList, HashMap, HashSet, and ConcurrentHashMap instances; arrays; int ranges; and long ranges."

"Not only can parallelizing a stream lead to poor performance, including liveness failures; it can lead to incorrect results and unpredictable behavior."

"Under the right circumstances, it is possible to achieve near-linear speedup in the number of processor cores simply by adding a parallel call to a stream pipeline."

### 8 Methods

Item 49: Check parameters for validity

"The Objects.requireNonNull method, added in Java 7, is flexible and convenient, so there's no reason to perform null checks manually anymore."

Item 50: Make defensive copies when needed

"You must program defensively, with the assumption that clients of your class will do their best to destroy its invariants."

"Date is obsolete and should no longer be used in new code."

"To protect the internals of a Period instance from this sort of attack, it is essential to make a defensive copy of each mutable parameter to the constructor and to use the copies as components of the Period instance in place of the originals."

"Note that defensive copies are made before checking the validity of the parameters, and the validity check is performed on the copies rather than on the originals."

"To prevent this sort of attack, do not use the clone method to make a defensive copy of a parameter whose type is subclassable by untrusted parties."

"To defend against the second attack, merely modify the accessors to return defensive copies of mutable internal fields."

Item 51: Design method signatures carefully

"Choose method names carefully."

"Don't go overboard in providing convenience methods."

"When in doubt, leave it out."

"Avoid long parameter lists."

"Long sequences of identically typed parameters are especially harmful."

"For parameter types, favor interfaces over classes."

"Prefer two-element enum types to boolean parameters, unless the meaning of the boolean is clear from the method name."

Item 52: Use overloading judiciously

"Because the classify method is overloaded, and the choice of which overloading to invoke is made at compile time."

"The behavior of this program is counterintuitive because selection among overloaded methods is static, while selection among overridden methods is dynamic."

"Therefore you should avoid confusing uses of overloading."

"A safe, conservative policy is never to export two overloadings with the same number of parameters."

"These restrictions are not terribly onerous because you can always give methods different names instead of overloading them."

"Therefore, do not overload methods to take different functional interfaces in the same argument position."

Item 53: Use varargs judiciously

Item 54: Return empty collections or arrays, not nulls

"In summary, never return null in place of an empty or collection."

Item 55: Return optionals judiciously

"Never return a null value from an Optional-returning method: it defeats the entire purpose of the facility."

"Optionals are similar in spirit to checked exceptions, in that they force the user of an API to confront the fact that there maybe no value returned."

"Container types, including collections, maps, streams, arrays, and optionals should not be wrapped in optionals."

"As a rule, you should declare a method to return Optional<T> if it might not be able to return a result and clients will have to perform special processing if no result is returned."

"Therefore, you should never return an optional of a boxed primitive type, with the possible exception of the “minor primitive types,” Boolean, Byte, Character, Short, and Float."

"More generally, it is almost never appropriate to use an optional as a key, value, or element in a collection or array."

Item 56: Write doc comments for all exposed API elements

"To document your API properly, you must precede every exported class, interface, constructor, method, and field declaration with a doc comment."

"The doc comment for a method should describe succinctly the contract between the method and its client."

"This illustrates the general principle that doc comments should be readable both in the source code and in the generated documentation."

"To avoid confusion, no two members or constructors in a class or interface should have the same summary description."

"When documenting a generic type or method, be sure to document all type parameters."

"When documenting an enum type, be sure to document the constants as well as the type and any public methods."

"When documenting an annotation type, be sure to document any members as well as the type itself."

"Whether or not a class or static method is thread-safe, you should document its thread-safety level, as described in Item 82."

"The only way to know for sure, however, is to read the web pages generated by the Javadoc utility."

### 9 General Programming

Item 57: Minimize the scope of local variables

"The most powerful technique for minimizing the scope of a local variable is to declare it where it is first used."

"Nearly every local variable declaration should contain an initializer."

"Therefore, prefer for loops to while loops, assuming the contents of the loop variable aren't needed after the loop terminates."

"A final technique to minimize the scope of local variables is to keep methods small and focused."

Item 58: Prefer for-each loops to traditional for loops

Item 59: Know and use the libraries

"By using a standard library, you take advantage of the knowledge of the experts who wrote it and the experience of those who used it before you."

"For most uses, the random number generator of choice is now ThreadLocalRandom."

"Numerous features are added to the libraries in every major release, and it pays to keep abreast of these additions."

"The libraries are too big to study all the documentation, but every programmer should be familiar with the basics of java.lang, java.util, and java.io, and their subpackages."

Item 60: Avoid float and double if exact answers are required

"The float and double types are particularly ill-suited for monetary calculations because it is impossible to represent 0.1 as a float or double exactly."

"The right way to solve this problem is to use BigDecimal, int, or long for monetary calculations."

Item 61: Prefer primitive types to boxed primitives

"Applying the == operator to boxed primitives is almost always wrong."

"In nearly every case when you mix primitives and boxed primitives in an operation, the boxed primitive is auto-unboxed."

"Autoboxing reduces the verbosity, but not the danger, of using boxed primitives."

"When your program does mixed-type computations involving boxed and unboxed primitives, it does unboxing, and when your program does unboxing, it can throw a NullPointerException."

Item 62: Avoid strings where other types are more appropriate

"Strings are poor substitutes for other value types."

"Strings are poor substitutes for enum types."

"Strings are poor substitutes for aggregate types."

"Strings are poor substitutes for capabilities."

Item 63: Beware the performance of string concatenation

"Using the string concatenation operator repeatedly to concatenate n strings requires time quadratic in n."

"To achieve acceptable performance, use a StringBuilder in place of a String to store the statement under construction."

"The moral is simple: "Don't use the string concatenation operator to combine more than a few strings unless performance is irrelevant."

Item 64: Refer to objects by their interfaces

"If appropriate interface types exist, then parameters, return values, variables, and fields should all be declared using interface types."

"If you get into the habit of using interfaces as types, your program will be much more flexible."

"It is entirely appropriate to refer to an object by a class rather than an interface if no appropriate interface exists."

"If there is no appropriate interface, just use the least specific class in the class hierarchy that provides the required functionality."

Item 65: Prefer interfaces to reflection

"You lose all the benefits of compile-time type checking, including exception checking."

"The code required to perform reflective access is clumsy and verbose."

"Performance suffers."

"You can obtain many of the benefits of reflection while incurring few of its costs by using it only in a very limited form."

"If this is the case, you can create instances reflectively and access them normally via their interface or superclass."

Item 66: Use native methods judiciously

"It is rarely advisable to use native methods for improved performance."

Item 67: Optimize judiciously

"Strive to write good programs rather than fast ones."

"Strive to avoid design decisions that limit performance."

"Consider the public consequences of your API design decisions."

"It is a very bad idea to warp an API to achieve good performance."

"He could have added one more: measure performance before and after each attempted optimization."

Item 68: Adhere to generally accepted naming conventions

### 10 Exceptions

Item 69: Use exceptions only for exceptional conditions

"The moral of this story is simple: Exceptions are, as their name implies, to be used only for exceptional conditions; they should never be used for ordinary control flow."

"A well-designed API must not force its clients to use exceptions for ordinary control flow."

Item  70: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors

"The cardinal rule in deciding whether to use a checked or an unchecked exception is this: use checked exceptions for conditions from which the caller can reasonably be expected to recover."

"Use runtime exceptions to indicate programming errors."

"Therefore, all of the unchecked throwables you implement should subclass RuntimeException (directly or indirectly)."

Item 71: Avoid unnecessary use of checked exceptions

Item 72: Favor the use of standard exceptions

"Do not reuse Exception, RuntimeException, Throwable, or Error directly."

Item 73: Throw exceptions appropriate to the abstraction

"To avoid this problem, higher layers should catch lower-level exceptions and, in their place, throw exceptions that can be explained in terms of the higher-level abstraction."

"While exception translation is superior to mindless propagation of exceptions from lower layers, it should not be overused."

Item 74: Document all exceptions thrown by each method

"Always declare checked exceptions individually, and document precisely the conditions under which each one is thrown using the Javadoc @throws tag."

"Use the Javadoc @throws tag to document each exception that a method can throw, but do not use the throws keyword on checked exceptions."

"If an exception is thrown by many methods in a class for the same reason, you can document the exception in the class's documentation comment rather than documenting it individually for each method."

Item 75: Include failure-capture information in detail messages

"To capture a failure, the detail message of an exception should contain the values of all parameters and fields that contributed to the exception."

"Because stack traces may be seen by many people in the process of diagnosing and fixing software issues, do not include passwords, encryption keys, and the like in detail messages."

Item 76: Strive for failure atomicity

"Generally speaking, a failed method invocation should leave the object in the state that it was in prior to the invocation."

Item 77: Don't ignore exceptions

"An empty catch block defeats the purpose of exceptions, which is to force you to handle exceptional conditions."

"If you choose to ignore an exception, the catch block should contain a comment explaining why it is appropriate to do so, and the variable should be named ignored."

### 11 Concurrency

Item 78: Synchronize access to shared mutable data

"Synchronization is required for reliable communication between threads as well as for mutual exclusion."

"Do not use Thread.stop."

"Synchronization is not guaranteed to work unless both read and write operations are synchronized."

"In other words, confine mutable data to a single thread."

"In summary, when multiple threads share mutable data, each thread that reads or writes the data must perform synchronization."

Item 79: Avoid excessive synchronization

"To avoid liveness and safety failures, never cede control to the client within a synchronized method or block."

"As a rule, you should do as little work as possible inside synchronized regions."

Item 80: Prefer executors, tasks, and streams to threads

Item 81: Prefer concurrency utilities to wait and notify

"Given the difficulty of using wait and notify correctly, you should use the higher-level concurrency utilities instead."

"Therefore, it is impossible to exclude concurrent activity from a concurrent collection; locking it will only slow the program."

"For example, use ConcurrentHashMap in preference to Collections.synchronizedMap."

"For interval timing, always use System.nanoTime rather than System.currentTimeMillis."

"Always use the wait loop idiom to invoke the wait method; never invoke it outside of a loop."

"There is seldom, if ever, a reason to use wait and notify in new code."

Item 82: Document thread safety

"The presence of the synchronized modifier in a method declaration is an implementation detail, not a part of its API."

"To enable safe concurrent use, a class must clearly document what level of thread safety it supports."

"Lock fields should always be declared final."

Item 83: Use lazy initialization judiciously

"Under most circumstances, normal initialization is preferable to lazy initialization."

"If you use lazy initialization to break an initialization circularity, use a synchronized accessor because it is the simplest, clearest alternative."

"If you need to use lazy initialization for performance on a static field, use the lazy initialization holder class idiom."

"If you need to use lazy initialization for performance on an instance field, use the double-check idiom."

Item 84: Don't depend on the thread scheduler

"Any program that relies on the thread scheduler for correctness or performance is likely to be nonportable."

"Threads should not run if they aren't doing useful work."

"When faced with a program that barely works because some threads aren't getting enough CPU time relative to others, resist the temptation to “fix” the program by putting in calls to Thread.yield."

"Thread.yield has no testable semantics."

"Thread priorities are among the least portable features of Java."

### 12 Serialization