## Java Programmming 4th Edition, 2005 (Arnold, Gosling, Holmes)
___

### 0. Java Virtual Machine (JVM)
Additional resources to help with understanding Java (Not part of the book).
- https://javatutorial.net/jvm-explained
- https://www.artima.com/insidejvm/ed2/index.html
- https://www.artima.com/underthehood/index.html
- https://www.javacodegeeks.com/2018/04/jvm-architecture-execution-engine-in-jvm.html
___

### 1. Introduction
- Java Class = a factory with blueprints to make:
- Instances of a Class = Objects
- Class contains Fields and Methods and other Classes/Interfaces.
- Fields = data variables describing state of Class/Objects.
- Methods = collection of statements that operate on the Fields and direct program execution.
```
class HelloWorld {
    public static void main(String[] args) {
      System.out.println("Hello, world");
    }
}
```
- *main* method, by default, is executed when running the class as an application.
- Signature of a Method = Method name + parameter list
- Method header = Method signature + modifiers + return type + exception throws list.
- Method declaration = Method header + Method body

**Primitive Data Types**

| Types | Description |
| - | - |
| boolean | *true* or *false* |
| char | UTF-16 (unsigned) |
| byte | 8-bit integer (signed) |
| short | 16-bit integer (signed) |
| int | 32-bit integer (signed) |
| long | 64-bit integer (signed) |
| float | 32-bit floating-point (IEEE 754) |
| double | 64-bit floating-point (IEEE 754) |

- every primitive type has a corresponding object type, or wrapper class. E.g. class *Integer* is a wrapper class for type *int*. Java automatically converts between primitive types and objects of wrapper class if one type is used where the other is expected (called autoboxing/unboxing).
- Reason for wrapper classes: generic classes only work with objects and don't support primitives, therefore primitive values must be converted to wrapper objects to be used by generic classes.


- // = single line commment
- /\* ... \*/ = multi-lines comments
- /\*\* ... \*/ = documentation comments


- java codes are written in Unicode

**Classes and Objects**
- Objects are created by expressions using *new* keyword.
- Instantiation = creating object from class definition (objects also called instances)


- Newly created objects are allocated within system memory called heap.
- Objects are accessed via references.
- Variables contains a reference to objects (reference type).
- Variables contains a value (primitive type).
- Object references are *null* when they do not refer to any objects.
- Unreferenced objects are reclaimed by a Garbage Collector, removing it from storage allocation heap.


- Methods can only return a single value as result. To return multiple values, an object needs to be returned holding those values.
- *this* is an implicit reference to the current object for the object methods to work on.
- Class Methods = are intended for operations on the Class itself, not on any specific instances. Such methods are declared using *static* keyword.

- **Arrays** = A collection of variables all of the same type.
- **String Objects** = A class type that deals specifically with sequences of character data. immutable.
```
System.out.println("hello world");
```
- The compiler creates a *String* object, initialized to the value of the specified string literal and pass the *String* object to the *println* method.
- Using the == to compare String objects is comparing to see if two variables refer to the same object, NOT testing if they have the same contents!
- **Annotations** = provide information about elements of a program in a structured way that is amenable to automated processing by external tool.
- **Packages** = contains a set of types and subpackages that can be imported by other codes.
- Package names are prefix for Classes within the package to prevent naming conflicts.
- Importing a package instructs the compiler to look in the package for Classes or Types it cannot find locally.

**The Java Platform**
- Java is designed for portability and specify details for all implementations.
- E.g. a Java *double* is a 64-bit IEEE 754 floating-point number across all platforms. Many languages leave the precise definitions to particular implementations, making only general guarantees such as minimum
range, or they provide a way to ask the system what the range is on the current platform.
- Source code is compiled into Java bytecode, to be run on Java Virtual Machine (JVM).
- JVM provides a runtime system, which provide access to the VM itself (e.g. to start the Garbage Collector) and the outside environment (e.g. output stream).
- Runtime system checks security-sensitive operations.
- When Classes are loaded into VM, they are checked by a verifier that ensures that bytecodes are properly formed and meet security and safety guarantees (e.g. illegal memory references).
- Platform independence, portability, protection to prevent careless/malicious classes from harming the system.

___

### 2. Classes and Objects
```
// Example of a simple Class
class Body {
  public long idNum;
  public String name;
  public Body orbits;

  public static long nextID = 0;
}

Body mercury;
mercury = new Body();
```
- *mercury* is a variable that can hold a reference to an object of type *Body*
- **The declaration does not create an object!!** It declares only a reference that is allowed to refer to a *Body* object. The object must be explicitly created.

**Class Modifiers**
- *public* = anyone can declare refences to ojects of the class, or access its public member.
- Without modifier, a class is only accessible within its own package.
- most java dev tools require a *public* class to be declared in a file with the same name, which means there can only be one public class declared per file.
- *abstract* = no instances of the class may be created. Probably contains *abstract* methods that must be implemented by a subclass.
- *final* = cannot be subclassed.
- a class cannot be both *final* and *abstract*
- *strictfp* = all floating-point in the class are evaluated strictly

**Fields**
Fields can be declared with the following modifiers:
- *static* = only one copy of the field exists, no matter the number of instances of the class. referenced externally using class name. field is initialized when the class is initialized after it is loaded.
- *final* = value cannot be changed after it has been initialized.
- a *final* field of primitive type initialized with a constant expression is a Constant Variable. The compiler does not generate bytecode to load the value of the field from the object, but inserts the value directly into the bytecode (more optimized!)
- *transient*
- *volatile*
- a field cannot be both *final* and *volatile*

- a field can be initialized with a value, which can be another field, method invocation, or expression, as long as the type is correct.
- fields declared but not initialized are assigned default values:

| Types | Initial Value |
| - | - |
| boolean | *false* |
| char | '\u0000' |
| byte, short, int, long | 0 |
| float, double | +0.0 |
| object reference | *null* |

**Access Control Modifiers**
All members of a class are accessible to code in the class itself. Access from outside the class can be controlled with:
- *private* = members can only be accessed in the class itself
- *package* = members declared with NO modifier are accessible in classes in the same package.
- *protected* = members are accessible in subclasses of the class, in classes in the same package.
- *public* = members are accessible anywhere the class is accessible.

**Construction and Initialization**

 - Constructors = used to initialize an object before the reference to the object is returned by *new*. Constructors are **NOT** Methods.
```
class Body {
    public long idNum;
    public String name = "<unnamed>";
    public Body orbits = null;
    private static long nextID = 0;

    Body() {
      idNum = nextID++;
    }
}
```

- the *Body* constructor is invoked when *new* create the object but after *name* and *orbits* have been set to their default initial values.
```
Body(String bodyName, Body orbitsAround) {
    this(); //must be the first statement!
    name = bodyName;
    orbits = orbitsAround;
}
```

- one constructor can execute an explicit constructor invocation using *this()*.
- if no constructors are provided, Java provides a default no-arg constructor that does nothing. It has the same accessibility as the Class.
- a Copy Constructor creates an object using an existing object as input:
```
Body(Body other) {
    idNum = other.idNum;
    name = other.name;
    orbits = other.orbits;
}
```
- Initialization Block is executed at the beginning of every constructor in the class.
```
class Body {
    public long idNum;
    public String name = "<unnamed>";
    public Body orbits = null;
    private static long nextID = 0;

    {
      //Initialization Block
      idNum = nextID++;
    }

    public Body(String bodyName, Body orbitsAround) {
      name = bodyName;
      orbits = orbitsAround;
    }
}
```
- Initialization Blocks can be declared *static*, which can only refer to and initialize other static members of the class.
- Static Initializers are executed after the class is loaded, before it is actually used.


**Methods**
Method modifiers consist of the following:
- *abstract* = the method body has not been defined. to be implemented by a subclass.
- *static* = typically class method. can only access static fields and other static methods of the class as there is no object references available.
- *final* = final methods cannot be overridden in subclass.
- *synchronized*
- *native*
- *strictfp* = if method is declared in a class that declared *strictfp* then this is implicitly declared.
- an abstract method cannot be static, final, synchronized, native, or strict.
- a native method cannot be strict.
```
public static void print(String... messages) {
    // ...
}
```
- varargs methods = the last parameter in a method can be declared as a sequence of a given type. the input type of this sequence is Array.
- input parameters of methods are **passed by value**. for primitive types input, the method receives a copy of the value from the invoker.
- when the parameter is an object reference, the method receives a copy of the object reference from the invoker, that refers to the same object. Meaning the invoking method **can modify the object**.
```
class PassRef {
    public static void main(String[] args) {
      Body sirius = new Body("Sirius"); //target oject
      commonName(sirius);
    }
    public static void commonName(Body bodyRef) {
      bodyRef.name = "Dog Star";        // this statement modifies the object.
      bodyRef = null;                   // this statement only modify the variable bodyRef
    }
}
```
- the input parameter can also be declared *final* so that it's value cannot be modified during method execution and helps compiler optimize the code.
- Getters and Setters methods gives access to private fields and allows better control.
- *this* keyword can be used to reference the current object in a non-static method.


**Method Overloading**
- Methods can be declared with the same name as long as they have different signatures (number and types of parameters).
- Try not to overload methods that leads to ambiguity. Java always select a fixed-argument method over a vararg method.

**The main() Method**
- When the name of a class is provided to run a program, the system will search for the *main()* method in the class to execute.
- the method must be defined in the following format:
```
public static void main(String[] args) {
}
```
- When run, the Java bytecode interpreter finds the compiled bytecode for this class, loads them into JVM, invokes the *Class.main* with the program arguments in a String array.

**Native Methods**
- Methods implemented in native code, and is declared using *native* modifier.
- all portability and safety of the code are lost.
- Methods are implemented using an API provided by devs who wrote the VM on which the code executes.
___

### 3. Extending Classes

- Class's Contract = collection of methods and fields that are accessible from outside a class + description of these member's behavior.
- Class Extension provides 2 forms of inheritance:
  - inheritance of contract/type = subclass acquires the type of super class, and can be used polymorphically wherever the superclass can be used.
  - inheritance of implementation = subclass acquires the implementation of superclass's accessible fields and methods.


**Extended Class**
- a class that does not explicitly extends anything implicitly extends the *Object* class.
- *Object* class is the root of class hierarchy in Java.
- the extended class's constructor must delegate construction of inherited state by either explicitly (using *super*) or implicitly invoking the superclass constructor.
- if not superclass constructor is invoked, Java automatically invokes the superclass's no-arg constructor before any statments of the new subclass constructor. (if superclass does not have no-arg constructor, then it must be explicitly invoked)
- Constructors are **NOT** Methods so they are not inherited. Extended class must explicitly declare each constructor that it wishes to take from the superclass.

**Constructor Order Dependencies**
1. Object is created, memory is allocated for all its fields (including those inherited from superclass), and fields are set to default values.
2. Invoke superclass's constructor.
3. Initialize the fields using their initializers and any initialization blocks.
4. Execute the body of the constructor.
```
class X {
    protected int xMask = 0x00ff;
    protected int fullMask;
    public X() {
      fullMask = xMask;
    }
    public int mask(int orig) {
      return (orig & fullMask);
    }
}
class Y extends X {
    protected int yMask = 0xff00;
    public Y() {
      fullMask |= yMask;
    }
}
```

| Step | What Happens | xMask | yMask | fullMask |
| - | - | - | - | - |
| 0  | Fields set to default values  | 0  | 0  | 0  |
| 1  | Y constructor invoked  | 0  | 0  | 0  |
| 2  | X constructor invoked (super)  | 0  | 0  | 0  |
| 3  | Object constructor invoked  | 0  | 0  | 0  |
| 4  | X field initialization  | 0x00ff  | 0  | 0  |
| 5  | X constructor executed  | 0x00ff  | 0  | 0x00ff  |
| 6  | Y field initialization  | 0x00ff  | 0xff00  | 0x00ff  |
| 7  | Y constructor executed  | 0x00ff  | 0xff00  | 0xffff  |

if at step 5, X constructor called the Method *mask* and if the subclass Y **overridden** the Method, this step will actually **use the overridden *Mask* Method from class Y instead!!**

**Inheriting and Redefining Members**
- Overloading a Method
- Overriding a Method = replacing superclass's implementation of a method with identical signature.
- When overriding methods,
  - if superclass method return type is object reference, subclass method can change the return type to a reference of a subclass of that object.
  - (a reference to a superclass object can always hold a reference to an instance of a subclass)
  - but if method return type is primitive, then overriding method must return the same type.
- Overriding methods can only provide equal or more access to a superclass method, but not less access, as that results in an instance of subclass unusable in place of a superclass instance (that is a breach of Class Contract).
- Overriding methods can also be made *final* or *abstract*.

**Type of Reference vs Type of Object**

```
class SuperShow {
    public String str = "SuperStr";
    public void show() {
      System.out.println("Super.show: " + str);
    }
}
class ExtendShow extends SuperShow {
    public String str = "ExtendStr";
    public void show() {
      System.out.println("Extend.show: " + str);
    }
    public static void main(String[] args) {
      ExtendShow ext = new ExtendShow();    //actual class of object determined by invoking constructor
      SuperShow sup = ext;   //same object as above but a different object type reference
      sup.show();
      ext.show();
      System.out.println("sup.str = " + sup.str);
      System.out.println("ext.str = " + ext.str);
    }
}
```
In the above example, the resulting output is:
```
Extend.show: ExtendStr  //actual class of object governs the version of method called
Extend.show: ExtendStr
sup.str = SuperStr      //the type of object reference determines the class field accessed
ext.str = ExtendStr
```
The superclass's field is hidden due to overriding, but the field is still accessible through a **superclass type reference** on a **subclass object**.
The same concept applies to **static** members, where the version accessed is based on declared type of reference, not type of objects.

- this is something to consider when passing a subclass object to a method that is expecting superclass instance as input parameter. the method may access the wrong field through direct field access.
- therefore it is good practice to have *private* fields that are only accessed using getter/setter methods.

- if a method in the superclass is *private*, it cannot be inherited or overridden. even if the subclass has a method with identical signature, **invocations of private methods always invoke the implementation declared in the current class (so if execution and invocation occurs in the superclass code block, then superclass method is invoked)**

- the *super* prefix will always invoke a superclass implementation, regardless of the current object type or reference type.


**Compatibility**
- **Assignment Compatibility** = reference types are compatible with the declared type, subtype of, or a type that can be converted to the declared type. (a subtype reference cannot be assigned a supertype object).
- the same rule apply when returning an expression from a method, must be compatible with declared return type.
- *null* is compatible with any reference type.
- **narrowing** = supertype object to subtype reference. (must be explicitly casted).
- **widening** = subtype object to supertype reference.
- *instanceof* can be used for type testing, before performing a legit casting or type specific methods.

**Protected Modifier**
```
//Subclass method
public void doSomething(Subclass instance) {
    Attribute A = instance.A; // A is a protected field
}
```
- The above code will work. The accessing class is the same as the parameter type, so it is able to access a *protected* member. However, the below will throw error:

```
public void doSomething(Superclass instance) {
    Attribute A = instance.A;
}
```
- This is because there is no guarantee of keeping the class contract, the superclass parameter type may reference an entirely different subclass and this access would violate the contract (the subclass may have modified the protected members).
- However, for *static* members, such access is fine as a subclass cannot override a *static* member, only hide it.


**Final Modifier**
- marking a class *final* = class is not extensible
- marking individual methods *final* still allows the class to be extended for more functionality. but fields should be private to prevent unwanted access.
- runtime system determines the actual class of object at method invocation, and binds the correct method implementation. marking a method *final* simplifies and optimize this process.**inlining** = replacing method invocation with the method body itself.
- as *final* classes are not extensible, type checks are faster and can happen at compile time, instead of runtime.


**Abstract Classes and Methods**
```
abstract class Benchmark {
    abstract void benchmark(); //needs to be implemented by subclass

    public final long repeat(int count) {
      long start = System.nanoTime();
      for (int i = 0; i < count; i++)
        benchmark();
      return (System.nanoTime() - start);
    }
}
```
- the **Template Method** pattern = part of the "expertise" method needs to be implemented by someone else (e.g. the benchmark method), without compromising the methods already established (e.g. the repeat method).
- all subclasses that is not abstract must implement all *abstract* methods.
- subclasses may override superclass methods to make them *abstract*. Useful technique when a class's default implementation is invalid for part of the class hierarchy.


**The Object Class**
- root of the class hierarchy.
- defines some default methods inherited by all objects:
    - `public boolean equals (Object obj)` = default implementation is to compare *this* reference to *obj* reference `this == obj`. Should be overwritten to determine if Objects are equal based on business logic.
    - `public int hashCode()` = returns a hash code to be used in hash tables. Should be overwritten if business logic has notion of equality.
    - `protected Object clone() throws CloneNotSupportedException` = returns a clone of this object as a new object.
    - `public final Class<?> getClass()` = Returns the type token that represents the class of this object. For each class T there is a type token **Class<T>** (read as "class of T") that is an instance of the generic class **Class**.
    - `protected void finalize() throws Throwable` = finalize object during garbage collection.
    - `public String toString()` = Returns a string representation of the object. Method is invoked whenever an object reference is used within a string concatenation expression (+ operator).

**Object Identity and Equality**
- Identity = reference equality. two references referring to the same Object.
- Equivalence = value equality. two objects of equal value, which may or may not be identical objects.
- in default *Object* implementation, identity is the same as equivalence.

**Cloning Strategies**
- factors to take note when implementing cloning:
  - implement the **Cloneable** interface in order to provide a *clone* method. (this interface is actually empty. explicitly declaring that it is implemented just helps send the signal to Java that cloning is supported)
  - (override) **clone** method from **Object** class which simply copies all fields in the object.
  - `CloneNotSupportedException`, which can be used to signal that the *clone* method should not be invoked.
- 4 attitudes towards cloning:
  - Support **clone** method. implements **Cloneable** and declares **clone** method to throw no exceptions.
  - Conditionally support cloning. might be a collection of class that can be cloned in principle but may not be successfully cloned unless all its content can be cloned, and will have occasions to throw unsupported exceptions.
  - Allow subclasses to support **clone** but don't publicly support it. Does not implement **Cloneable** but it the *clone* method implementation isn't correct, provides protected *clone* implementation that clone its fields correctly.
  - Forbid **clone**. Does not implement **Cloneable** and provides a *clone* method that always throw unsupported exception. A subclass can actually extend a cloneable superclass but don't support cloning.


- Simplest way to design for cloning is to implement *Cloneable* interface and override *clone* method.
- subclass should always try to use *super.clone()* to prevent creating additional problems as the superclass may already be handling some issues itself.
- **final** fields that requires constructors to set values may encounter problems when cloning, if constructor is not invoked and field values cannot be set.


- default cloning implementation is **shallow cloning**. values of each fields are copied.
- **deep cloning** recursively clones the object referenced by the fields as well, e.g. each objects in an array are cloned.
- **Object Serialization** mechanism allows another way to do deep cloning.


**Class Extension Framework**
- **Public** members are contract for users of the class.
- **Protected** members are contract for extenders of the class.
- **Private** members are to prevent any unwanted direct access to data and methods of the class.


- Java uses single-inheritance model of OOP to prevent confusing problems of multiple typing.
- to achieve multiple-inheritance, Java provides interface inheritance. a subclass can only inherit a single superclass but may implement multiple interfaces.
___

### 4. Interfaces

The fundamental unit for programming is the class, but the fundamental unit of object-oriented design is the type. Interfaces define types in an abstract form.

We use interfaces instead of abstract class when we want other developers to inherit the interface but allow the freedom to extend from various classes and no restrictions on implementations. Abstract classes provides partial implementation that may be *final* hence other developers extending the class cannot modify it, this also means that the developers do not have the freedom to extend any other class.

**Interface Constants**
- Implicitly *public, static, final*. Must have initializers, cannot be blank.
- Requires nested class to define shared, modifiable data in the interface.

**Interface Methods**
- Implicitly *abstract*.
- Therefore, methods cannot be *static*. *abstract* methods are meant to be overridden in a subclass (and overriding occurs in the context of dynamic lookup, i.e. at runtime), but *static* methods are looked up statically at compile time, hence they cannot be overridden.

**Extending Interfaces**
- Unlike class, interfaces support multiple inheritance. New interface definition may extend multiple interfaces.
- **Hiding Constants**
  - a subinterface inherits all static constants from superinterface.
  - if subinterface declares a new constant with same name, the superinterface's constant will be hidden
  - But it can still be access by using the *qualified name* (e.g. superinterface.constant)
  - Or if subinterface has been implemented in a class, the class object can still access superinterface constant through *explicit cast*.
  - If a subinterface inherits multiple superinterfaces containing same named constants, any simple reference from the subinterface implementation will throw compile-time error due to ambiguity in reference.
- **Inheriting Methods**
  - Similarly, there is only one implementation of method even if the same method signature is inherited from multiple superinterfaces.
  - The return type of one of these similar methods *must be a subtype* of all the other method return types, and the implementation *must return this common subtype*, otherwise a compile-time error will occur.
  - It may be impossible to have an implementation that honors all inherited contracts.

**Composition & Forwarding**
- a strategy of implementing multiple interfaces (e.g. A & B) can be:
  - first extend from an implementation class of A.
  - in this subclass, compose another interface implementation object of B.
  - all method calls for interface A can be handled by inherited methods in the subclass.
  - all method calls for interface B can be forwarded to that previously composed object. forward all return values.

**Marker Interfaces**
- Do not declare any methods. It is simply to mark that a class implementing this interface has some general properties.
- e.g. *Clonable*, *Serializable*, *Externalizable*
___

### 5. Nested Classes and Interfaces

Two main reasons to use nested classes and interfaces:
1. allow types to be structured and scoped into logically related groups.
2. can be used to connect logically related objects simply and effectively.

Nested types are considered part of the enclosing type, and each can access all members of the other.

**Static Nested Types** - accessibility of nested types are determined by enclosing types. Interfaces are always static.

**Inner Classes** - non-static nested classes. Associated with instances of a class. Conventionally, each instance should only access their corresponding nested class instances.

There is no limit on the level of nesting but try not to go deeper than one level to preserve readibility.

**Extending Inner Classes** - this requires objects of the extended class to still be associated with objects of the original enclosing class or a subclass. If we only want to extend the inner class as a stand alone class, then we need to explicitly supply an explicit reference to the enclosing class in the extended class constructor, for example:
```
class Outer {  
  class Inner {
    ...
  }
}


class Unrelated extends Outer.Inner {
  Unrelated(Outer ref) {
    ref.super(); //this calls the constructor of the original Inner class
  }
}
```
Inner classes inherit the fields and methods of the outer class, but will hide all inherited elements once an element of the same name has been declared. (all overloaded outer methods will be hidden as long as there is a declaration of a single inner method).

**Local Inner Classes** - can be defined within method body, constructors, or initialization blocks. They cannot be accessed outside of the block. Inner classes may access local variables and method parameters that are declared *final*, to prevent illegal states of elements being accessed.

**Anonymous Inner Class** - defined in the *new* expression itself, cannot have an explicit extends or implements clause, nor can it have any modifiers.

**Implementation of Nested Types** - the compiler applies source code transformation to make nested types compatible with older JVMs. So note that when bundling class files, strange file names with $ are usually inner classes and needs to be included. Also, when using reflection to created nested class instances, these transformed names need to be used.
___

### 6. Enumeration types

In Java, enum is a special kind of class. References to the class are completely type-safe as only the enum constants can be assigned.
```
enum Suit {
  CLUBS, DIAMONDS, HEARTS, SPADES;

  other fields;
  other methods();
  even constructor Suit(String name);
}

- enum is the declaration keyword
- Suit is the identifier name
- CLUBS... are the enum constants, each refers to an instance of the enum class.
```
enum type has two static methods automatically generated by the compiler:
```
public static E[] values() - returns array containing enum constants in original order.
public static E valueOf(String name) - returns enum constant with given name.
```

enum constructors are private, and they cannot use a non-constant static field of the enum. Reason being the constructors are the first codes to be executed in static initialization. As a work-around, code the logic in an initialization block instead.

**Constant Specific Behavior**
Each enum constants can be extended by anonymous inner classes to give constant specific behavior, which is good so that all constant specific actions are checked at compile time instead.

However, enum constant class body cannot declare static members or define constructors. Also, the anonymous inner classes have no enclosing instance as enum constants are implicitly static fields.

For enum constant methods to be directly accessible, the methods must be explicitly declared in the enum type itself. If the enum type declares an abstract method, every enum constant must implement the method.

**java.lang.Enum**
Cannot be extended directly. But is implicitly extended by all enums. Establishes useful properties:
1. *clone* method is overridden to be declared *final* and to make it impossible to clone an enum instance.
2. *hashCode* and *equals* method overridden to ensure equivalence is the same as identity.
3. *compareTo* method is implemented and defined for enum constants to have natural ordering based on the their order of declarations.

There are two efficient enum-specific collection classes: *EnumSet* and *EnumMap* for enum to work with collections.

**Design Choices**
Enums are convenient and recognised by other parts of the language. Even though it can provide constant specific behavior, it cannot provide specialization of behavior through subclassing.
___

### 7. Tokens, Values, and Variables

I have chose to omit this section as it was good information on how java interprets the lexical elements of the language. but I would hardly need to refer to it when programming.
___

### 8. Primitives as types

In cases where an Integer is required, an object may incur too much overhead, and a plain primitive *int* will do. While *int* may be fast and convenient, primitives cannot be stored into a hashtable, hence Java provides a wrapper class corresponding to each primitive type. The wrapper class also provide a home for methods and variables related to the type.
- **Boxing conversion** : convert a primitive value to a wrapper object.
- **Unboxing conversion** : extract a primitive value from a wrapper object.

**Key Characteristics**
- Each wrapper class defines an immutable object; Once a primitive value is wrapped, the object will always have the same value. (hence instead of creating a new instance of the class with the same value, using a **valueOf** method call may return a cached instance, improving efficiency).
- Each class has constants MIN_VALUE, MAX_VALUE, SIZE (bits required to represent value), and TYPE (which returns a *class* instance).
- Float and Double class has got special POSITIVE_INFINITY, NEGATIVE_INFINITY, and NaN (not-a-number) constants. NaN==NaN returns false as it is not a value.
- the **Void** class is a special exception, with no values to wrap, a placeholder representing no return type, it is useful in reflection.

Do refer back to the original text for more details when working on relevant and specific use cases.
___

### 9. Operators and Expression

**Arithmetic Operations** - Particularly for floating points:

| x | y | x/y | x%y |
| - | - | - | - |
| finite | +- 0.0 | +- inifnity | NaN |
| finite | +- infinity | +- 0.0 | x |
| +- 0.0 | +- 0.0 | NaN | NaN |
| +- infinity | finite | +- infinity | NaN |
| +- infinity | +- infinity | NaN | NaN |

*strictfp* modifiers make all fp evaluation strict so that values from the same operations always equals across all JVMs. Non strict evaluations offer the machines running the code to optimise the operations as they deem fit.

**Increment & Decrement** - Comparing two expressions:
```
arr[index()]++;
arr[index()] = arr[index()] + 1;
```
the second expression evaluates the index twice, hence the first expression is more efficient.

Also, increment and decrement can be prefixed or post-fixed. The difference being, a prefix increment expression performs the operation first before returning a value (an expression *i++* has a return value of *i* by default). A post-fixed expression returns the current value, then performs the increment operation.
```
int i = 16;
System.out.println(++i);
System.out.println(i++);
System.out.println(i);

//printed values:
17
17
18
```

**Relational & Equality** - Floating point special cases
```
Double.NaN == Double.NaN //returns false
Double.NaN != Double.NaN //returns true (this can be used to test for NaN)
any Number != Double.NaN //returns true
//testing for NaN may use Double.isNaN() method
```

Equality test for reference identity not object equivalence: *ref1 == ref2* is true if both reference the same object or if both are *NUll* even if they are declared as different type references.

Object equivalence *Object.equals()* assumes an object is equal only to itself. So for *String*, this method has been overridden to determine if two String object has the same contents. So they may be equivalent, but have different references.

**Logical Operators** - Conditional AND (&&) will only evaluate the right operand if the left operand is true, while conditional OR (||) will stop evaluating right operand if left operand is true, making them more efficient. A trick to make operations safe:
```
if (0 <= ix && ix < array.length && array[ix] != 0) {
  //this avoids array index out-of-range error
}
```

**Bit Manipulation Operators** - bitwise operators only apply to integer types.
- bitwise AND (&)
- bitwise OR (|)
- bitwise XOR (^)
- << shift bits left, filling with zeros
- \>> shift bits right, filling with highest (sign) bit
- \>>> shift bits right, filling with zeros

The output size of any shift operations will be based on the left input. However, if the shift count requested exceeds the input size or is negative, (e.g. shift 35 or -29 on a 32bit int), the count will first be masked (by input type size -1, in this e.g. it is 31. therefore both shifts are the same as a shift of 3).

**Operator Precedence** - the following is a list of operators in order of precedence from highest to lowest:
- postfix operators `[] . (params) expr++ expr--`
- unary operators `++expr --expr +expr -expr ~ !`
- creation or cast `new (type)expr`
- multiplicative `* / %`
- additive `+ -`
- shift `<< >> >>>`
- relational `< > >= <= instanceof`
- equality `== !=`
- AND `&`
- exclusive OR `^`
- inclusive OR `|`
- conditional AND `&&`
- conditional OR `||`
- conditional `?:`
- assignment `= += -= *= /= %= >>= <<= >>>= &= ^= |=`

**Member Access** - a recap
- *Static Members* are accessed using either type name (e.g. *Math.something*) or using an object reference.
  - members retrieve are declared in the class, or inherited by the class.
  - when using object reference, member will be retrieve from the *declared type* of object reference, not the actual object itself. (therefore this object can be *null* value and the access will still work)
- *Non-Static Members* are accessed using object reference or implicitly using *this* keyword, if the member is in current object or the enclosing object.
  - member retrieved are also based on *declared type*
- *Method Access* however are based on the actual class of the object, regardless of the declared type of reference.

For more details on how Java compiler determines the right method to be invoked, please refer to the original text.

___

### 10. Control Flow

1. *ifelse*

```
if (expression)
  statement1
else
  statement2
```

2. *switch*

```
switch (expression)
  case n : statements
    break
  case m : statements
    break
  ...
  default : statements
```

3. *while* and *dowhile*

```
while (expression)
  statements

do
  statements
while (expression)
```

4. *for*

```
for (initialization-expression ; loop-expression ; update-expression)
  statement

for (Type loop-variable : set-expression)
  statement
```

5. *label*, *break* and *continue*

```
label: statement

//used in a loop
break; //the default breaks out of inner most loop
break label; //breaks out of the specified outer loop tagged by label

//used in a loop
continue; //the default skips the loop to the next iteration
continue label; //skips the specified loop tagged by label to the next iteration
```

___

### 11. Generic Types
Generic types can be used in place of *Object* types as generic references to allow more flexibility and also prevents runtime exceptions caused by wrong class casting.

**Declarations**

```
class Funnel<E> {
  private E headElement;
  private E tailElement;
  public void add(E element) {...}
  public E get(int index) {...}
}
```

The convention of using a single letter name as a type variable to represent a concrete type is:
- *E* for elements
- *K* for keys
- *V* for Values
- *T* for any types in general

All invocations of a generic class are simply expressions of that one class. E.g. *Funnel<String>* and *Funnel<Number>* are the same class when evaluating their class types (which is *Funnel.class*) at runtime.

Due to the fact that there is only one single class regardless of parameterized invocations, all **static** members within the class cannot be declared with a parameterized type *E*. Similarly, as the compiler needs to know one single definition to generate **new objects or arrays** in a generic class, declaration of objects or arrays using parameterized types will not compile:

```
class Funnel<E> {
  ...
  private E element = new E();  //this object creation is invalid

  public E[] toArray(size) {
    E[] arr = new E[size];   //this array creation is invalid
  }
}
```

For generic classes to work with arrays, the caller will have to create the array of the desired type and size, before passing it as parameter to the generic class.

The compiler actually handles type parameter but using the most general possible types (often *Object*) and uses type casting to ensure correctness.

**Bounded Type Parameters**

```
interface SortedCollections<E extends Comparable<E> & CharSequence> {
  ...
}
```

E is restricted to a type that implements or extends a few interfaces (multiple interfaces can be separated by an & character), providing an upper bound and guaranteed support of certain methods applicable to the interfaces.

**Nested Generic Types** - there are 2 designs

```
//using nested static class
class SingleLinkQueue<E> {
  static class Cell<E> { //the type variables are distinct names!
    ...
  }
  private Cell<E> head;
  private Cell<E> tail;
}

//using inner class (non-generic)
class SingleLinkQueue<E> {
  class Cell {
    private E element; //same type variable of outer class
    ...
  }
  private Cell head;
  private Cell tail;
}
```

In the first design, using nested generic classes can lead to added complexity due to hiding of variable types.

**Working with Generic Types - Subtyping**

A key point to note regarding parameterized types, unlike arrays, where *Integer[]* is a subtype of *Number[]*, *List<Integer>* is **NOT** a subtype of *List<Number>*. The definition means we are having a type with objects compatible to List and has element declared to be Number.

Instead a proper way to define the parameterized type to meet such subtyping requirements is to use wildcard argument.

```
List<? extends Number>
``` 

This indicates that we require a List of any type, as long as that type is *Number* or a subclass of *Number*. Therefore, *Number* forms the **upper bound** on the type expected.

```
List<? super Integer>
```

This example specifies that the wildcard type must be a supertype of *Integer*. Therefore, *Integer* forms the **lower bound** on the type expected.

Note that wildcard types can have only an upper bound or lower bound, unlike bounded type parameters. 

With wildcard, the subtyping requirements can be met when working with classes. For example, *List<? extends Integer>* is a subtype of *List<? extends Number>*, *List<? super Number>* is a subtype of *List<? super Integer>*.

```
interace SortedCollection<E extends Comparable<E>> {}         //this is overly restrictive
interace SortedCollection<E extends Comparable<? super E>> {} //this is better as it does not require type E itself to implement Comparable interface when it could inherit from a supertype

SingleLinkQueue<? extends Number> nQueue = new SingleLinkQueue<Number>();
nQueue.add(Integer.valueof(25)); //this is invalid. It won't compile as wildcard type is a subtype of Number, and there is no guarantee that all methods will work on a Number queue.

SingleLinkQueue<? super Number> nQueue = new SingleLinkQueue<Number>();
nQueue.add(Integer.valueof(25)); //this is valid. The required wildcard is always a supertype of Number, therefore a Number queue (lower bound) is definitely valid for the given input.
```

Wildcards can provide flexibility but requires a bit of planning to implement a good design.

*Subsequent topics have been skipped as I didn't understand the content. Perhaps I will revisit this chapter in future.*


___

### 12. Exceptions and Assertions
When an exception is thrown, the statement or expression that caused the exception complete abruptly. This causes the call chain to gradually unwind as each block or method invocation completes abruptly until the exception is caught. If the exception is not caught, the thread of execution terminates, after giving the thread's *UncaughtExceptionHandler* a chance to handle the exception.

- Synchronous exception occurs directly as a result of the execution of a particular instruction or by a *throw* statement.
- Asynchronous exception occurs only when JVM encounters an internal error, or when deprecated JVMTI methods are being used to stop JVM execution.

Overriding or implementing method is not allowed to declare more checked exceptions in the *throws* clause than the inherited method does. Otherwise, written code expecting inherited method will not be able to catch the new checked exceptions and handle them properly.

**finally**

Once an exception occurs, actions after the point at which the exception occurred do not take place. The next action to occur will be in either a finally block, or a catch block that catches the exception.

If a *finally* clause is present with a *try*, its code is executed after all other processing in the *try* block is complete. This happens no matter how completion was achieved, whether normally, through an exception, or through a control flow statement such as *return* or *break*.

*finally* clause is usually used to clean up internal state or to release non-object resources such as open files stored in local variables. It can also be used to clean up after *break*, *continue* and *return* without any *catch* clause.

There are two main coding idiom for using *finally*:

```
pre();
try {
  ...
} finally {
  post();
}

// The first idiom will only execute post() if pre() was successful and excution enters try block. If pre() fails then excution exits current method, and exception needs to the caught by the caller.

try {
  val = pre();
  ...
} finally {
  if (val != null)
    post();
}

// The second idiom will check if pre() was successful in the finally block before performing post(). Event though this idiom has an additional if condition, it prevents nested try-catch blocks as in the first idiom, hence keeping the code simple.
```

A point to note, when execution enters the *finally* clause, it carries a reason. If the *finally* clause exits execution with its own reason, such as through *return* statement or through an exception, the original reason will be overwritten. Example:

```
try {
  ...
  return 1;
} finally {
  return 2;
}
```

So in the *finally* clause, the return value of 1 is remember, but because the block returns value of 2, so when the block exits, the whole *try-finally* segment will only return value of 2. In fact even if an exception happened in the *try* block, the value of 2 will still be returned. 

**catch**

A general *catch* clause, one that catches *Exception* type, is usually a poor implementation of code as it is not comprehensive enough to handle the various causes of exceptions properly.

You cannot put a superclass exception before a subclass exception to be picked up by the catch clauses, as JVM examines the clauses in order, and the subclass will always be skipped, if the superclass is caught.

Only a one exception will be handled by any single encounter with a *try* clause. Therefore, if a *catch* clause throws an another exception, the subsequent *catch* clause will not pick it up. Exception thrown this way requires the caller to handle it.

**Exception Chain**

The idiomatic way to define a new exception class is to provide at least 4 constructor forms or variant that deal with exception specific data, to enable the custom exception to still capture the root cause when chaining exceptions.

```
class CustomException extends Exception {
  public CustomException() {}

  public CustomException(String details) {
    super(details);
  }

  public CustomException(Throwable cause) {
    super(cause);
  }

  public CustomException(String details, Throwable cause) {
    super(details, cause);
  }
}

// calling the custom exception can now easily retrieve the root cause
try {
  ...
} catch (IOException e) {
  throw new CustomException(e);
}
```

*initCause* and *getCause* are another way to retrieve the root cause of the exception.

**Stack Traces**

When an exception is created, a stack trace of the call is saved in the exception object. This is done by the *throwable* constructor invoking its own *fillInStacktrace* method. You can replace it with the current stack information by invoking *fillInStackTrace* again. If an exception has a cause, then typically printStackTrace will print the current exceptions stack trace, followed by the stack trace of its cause.

A stack trace is represented by an array of *StackTraceElement* objects that you can get from *getStackTrace*. You can use this array to examine the stack or to create your own display of the information. Each *StackTraceElement* object represents one method invocation on the call stack. You can query these with the methods *getFileName, getClassName, getMethodName, getLineNumber,* and *isNativeMethod*.

**When to use Exceptions**

Exceptions are not meant for simple, expected situations. In situations where code needs to detect certain state changes or conditions, but no reasonable flag value exists, the most reasonable design is to add an explicit test method to detect the change instead of adding an exception, which should be reserved for unexpected error condition under normal execution.

Deciding which situations are expected and which are not is a fuzzy area. The point is not to abuse exceptions
as a way to report expected situations.


**Assertions**

Used to check an invariant condition that should always be true, if not throws an exception.

Assertion evaluations can be turned on or off for the entire package, class and class loader. Hence do not attend to include value assignments in an assertion statement to prevent unwanted side effects when evaluation is turned off, for example:

```
assert i++ < 10; 
//final value of i depends on assertion evaluation. bad coding!
```

**Syntax for assertions**

```
assert expr [: details];
```

the details are optional. If detail is present and is a *Throwable*, it will become the cause of the *AssertionError*, else it will become a string message for the error details.

**When to use Assertions**

Assertions are used to detect errors that should never happen in proper program execution. It provides a mechanism to detect such errors should it really occur. Turning assertion evaluation on or off should never affect the proper program execution.

Assertions may be used to check that internal states are always valid or control flows are always running as expected.

Assertions may be turned on or off by selecting the options in CLI when running the JVM, or configured in packages and classes (more details in later chapters).

Even if the assertions are turned off, the code still exists as overhead consuming JVM resources even if it is not executed. For complete removal, a Java programming idiom may be used to help prevent the compiler from compiling the code:

```
private static final boolean ASSERT_FLAG = false;
if (ASSERT_FLAG) {
    // assert something
}
```

The above variable ASSERT_FLAG is a compile-time constant (it is static, final, and initilised with a constant value). References to ASSERT_FLAG will be replaced at compile-time with the constant boolean value, therefore the compiler will totally remove the if statement since it determines that it will never be executed. This same trick may be applied to completely remove print debug statements.


___

### 13. Strings and Regex

Java represents text consisting of Unicode characters as sequences of *char* values using the UTF-16 encoding format. The *java.lang.CharSequence* interface is implemented by classes that represents text:
- String
- StringBuilder
- StringBuffer (thread-safe implementation of StringBuilder)

*StringBuilder* and *StringBuffer* provides a mutable representation to the otherwise immutable *String* objects. Performance of these classes can be made more efficient by specifying the allocated buffer size on creation instead of allowing JVM to dynamically grow the buffer capacity.

**String Comparisons**

Characters are compared by evaluating their Unicode values.
- *equals* method compares two String objects to determine if they have the same length and contains the same Unicode characters. (Variants include *equalsIgnoreCase*, *contentEquals*)
- *compareTo* method determines if a String is greater, less than, or equals to the other using Unicode character ordering. Sorted list of String can use this method to perform Binary Search.
- *startsWith* and *endsWith* method checks if a String begins or ends with certain character sequence.

Equivalence *==* only checks if two references are the same, but does not compare String content. Therefore most of the time, it will give the wrong result. This will work however, if both variables are referencing *String literals*.

```
String str = "abc";
return str == "abc"; // this returns true!
```

This evaluation is faster than comparing contents but only works for String literals. To take advantage of this, the *intern* method can be used.
- *intern* method always return the same String object if the parameter has the same contents. Therefore the output of *intern* invocation may safely be used in *==* comparison.

Two Strings with the same content are guaranteed to have the same hash code. The *String* class overrides *Object.hashCode* to guarantee this behaviour.

**Compiling and Matching Regex**

Evaluating regular expression is compute intensive. The optimised model of compiling a regex pattern only once, and using it to match a single character sequence multiple times, is:
1. Create a *Pattern* object from a regex.
2. Applies *Pattern* object to a particular *CharSequence* to obtain a *Matcher* object.
3. Ask *Matcher* object to perform operations on the regex and input sequence.

**Regex Efficiency**

Given the following example to match two parts of a String separated by a comma:

```
pattern 1 = (.*),(.*)
pattern 2 = ([^,]*),([^,]*)
```

In pattern 1, the matcher will attempt to consume the entire input, then back up to the last comma. In pattern 2, the matcher will go as far as the first comma and stop. However, the best practice is to avoid trading clarity for efficiency, unless coding a performance critical piece of code, and to test the output carefully to ensure that there is indeed performance gain.


___

### 14. Threads

A sequence of steps executed one at a time is called a thread.

In multi-threaded programs, threads can perform tasks independent of each other, but share access to the same objects and resources. This sharing of access is one of the most useful features of multi-threading, but also one of the greatest pitfalls.

Model-View-Controller designs are some of the typical use cases to implement a multi-threaded system.

**Using Runnable**

```
public class CustomTask implements Runnable {
    public CustomTask(params) {
        // initialise internal states
    }
    
    public void run() {
        // computation to be run in threads
    }
}

// in main thread
Runnable task = new CustomTask(params);
new Thread(task).start();
```

The above example is the recommended practice instead of extending the *Thread* class to create a custom thread and override the *run* method within a thread. 

*Runnable* interface abstracts the concept of work and allows work to be associated with a worker thread. Here is the sequence of execution:
- When *start()* method is invoked on the *Thread* object, it spawns a new thread of control and returns.
- JVM will go on to invoke the *run()* method on this new thread.
- The *run()* method of this thread executes the *run()* method of the *Runnable* object that is passed into the thread.
- When *run()* method returns, the thread exits.
- The thread can also be stopped by invoking the *interrupt()* method.
- You can interact with a running thread using other access methods.

The below *Server* class creates a thread once it is initialised to always accept jobs from a queue and execute them. This is a conservative design to prevent unwanted invocation of the *run()* method, since it is *public* as part of the *Runnable* interface:

```
class Server {
    private final Queue queue = new Queue();
    
    public Server () {
        Runnable service = new Runnable() {
            // a Runnable inner class only accessible internally
            public void run() {
                while (true) do {
                    executeTask(queue.remove());
                }
            }
        }
    }
    
    public void insert(Job job) {
        queue.add(job);
    }
    
    private void executeTask(job) {
        // the actual task to be performed by the threads
    }
}
```

**Synchronization**

**synchronized** methods invoked by one thread on an object will first acquire the lock of this object, blocking all subsequent calls to this method acting on the same object. When the thread completes execution of the *synchronized* method and release the lock, other threads will resume execution.
- Synchronization forces the threads to be mutually exclusive in time.
- If a synchronized method is called within an outer synchronized method, and both requires the lock on the same object, execution will continue. This prevents a thread from blocking itself while it already has a lock. Therefore, locks are owned per thread.
- Synchronized methods do not require explicit acquiring and releasing of locks, reducing the chance of coding error.
- Synchronization does not guarantee order of execution between two threads. Only guarantees mutual exclusion.

**Static Synchronized Methods**
- A static synchronized method acquires lock of the *Class* object.
- Two threads cannot execute the synchronized method of the same class at teh same time.
- The *Class* object lock has no effect on any objects of that class.

**Synchronized Statements**

```
synchronized (object) {
    statements
}
```

This ensures that the statements are only executed when the lock for the object has been acquired.
- Actually, a *synchronized* method is actually a syntactic shorthand for the above, with the object being *this*.
- Synchronized statements provides more freedom to only synchronize a small region of code instead of the whole method.
- It also provides more flexibility to synchronize on objects other than *this*.
- For inner object and enclosing object, their locks are independent. If the inner object needs to synchronize with the outer object, it may use *synchronized* statement instead of having the enclosing class create a synchronized method just for the inner class to use.
- To synchronize on *Class* object, use the class literal e.g. *Something.class*.

**Synchronization Design**

1. Client-side synchronization makes all clients of a shared object use *synchronized* statements to lock the object before using it.
2. Server-side synchronization makes those shared object accessible only through their own *synchronized* method, which guarantees synchronization.
3. To use a class that is not synchronized in a multi-threaded environment, the class can be extended to override methods and making them *synchronized* and/or forward the method calls through the *super* reference.
4. Same as the above when working with interfaces, an alternate implementation can be provided that wraps methods of the interface in synchronized methods, that eventually forward the calls to another unsynchronized object that implements the same interface.

The way that synchronization is enforced in a class is an implementation detail, and therefore must be clearly documented for users and programmers to effectively use and extend the class.

**Wait and Notify**

The standard design pattern for wait and notification:

```
//Waiting Thread
synchronized void doWhenCondition() {
    while (!condition) {
        wait();
    }
    executeTask();
}
```

- Having the entire execution in a *synchronized* method ensures that other thread cannot modify the condition while this thread is running.
- *wait* method **atomically** pauses the thread and releases the lock. When thread restarts, it will also atomically acquire the lock. This prevents race hazard.
- The condition test should always be in a loop to ensure that it is checked every time the thread restarts. It is not correct to assume that the thread has woken due to a legitimate notification.

```
//Notification Thread
synchronized void changeCondition() {
    condition = true;
    notifyAll(); // wakes up all waiting threads
    or
    notify();    // wakes up one waiting thread
}
```

Different threads maybe waiting on different conditions. Using *notify()* instead of *notifyAll()* may wake up the wrong thread and the condition make go undetected by the appropriate waiting thread.

*notify* is an optimization that can be applied when:
- all threads are waiting on the same condition.
- at most one thread can benefit from the condition update.
- this is contractually true for all possible subclasses.

Implementing this design in the *Server* class for more efficient handling of job queue:

```
class Server {
    private final Queue queue = new Queue();
    
    public Server () {
        Runnable service = new Runnable() {
            public void run() {
                while (true) do {
                    Job job = remove();
                    executeTask(job);
                }
            }
        }
    }
    
    public synchronized void insert(Job job) {
        queue.add(job);
        notifyAll();
    }
    
    public synchronized Job remove() throws InterruptedException {
        while (queue.size() == 0) {
            wait();
        }
        return queue.remove();
    }
    
    private void executeTask(job) {
        // the actual task to be performed by the threads
    }
}
```

*wait* can also complete due to time-out or an interrupt.
- If *wait* completes due to time-out, there is no indication of this expiration. The thread has to track the elapsed time itself.
- And also, as the time-out mechanism still requires the lock on the object to be acquired, time-out actually cannot guarantee that *wait* will ever return in finite amount of time.

**Thread Scheduling**

- On a system with N available processors, usually N of the highest-priority runnable threads will be executing.
- Lower-priority threads generally run only when higher-priority threads are blocked, or at times to prevent starvation through a feature known as priority aging (no guarantee though).
- A running thread can run until it performs a blocking operation (wait, sleep, some kind of I/O), or until it gets preempted by a higher-priority thread, or by the thread scheduler.

When preemption occurs depend on JVM and there is no guarantee even though generally preference is given to higher-priority threads. Therefore thread priority and preemption should never be relied on for algorithm correctness.

A thread's priority is initially the same as the thread that created it, but may be changed at any time that the thread is running.
- Methods are *setPriority* and *getPriority*.
- *Thread* class constants are *MIN_PRIORITY*, *MAX_PRIORITY*, *NORM_PRIORITY* etc.

General practices that may be adopted when dealing with priorities:
- continuously running threads (main app) can run on lower priority.
- threads to handle rare event (e.g. user input) can run on higher priority to preempt the lower priority threads.
- since such events occur less frequently, the lower priority threads will still be allocated most of the processing time.
- use delta values around the normal priority level (*NORM_PRIORITY+1*,*NORM_PRIORITY-1*) instead of using the extreme values, which may lead to undesirable impact on relative priority of other applications running on the same system.
- it is possible to use *availableProcessors* method of the *Runtime* class to check number of processors available and adjust the behavior of algorithms (split tasks into smaller chunks etc) to take advantage of the number of processors. (note that number available processors can change at any time).

Threads may voluntarily reschedule by calling the *Thread* class static rescheduling methods:
- *sleep*, puts the currently executing thread to sleep for **at least** specified number of milliseconds. It is not exact due to thread scheduling, granularity and accuracy of system clock etc. If thread is interrupted while sleeping, an *InterruptedException* is thrown.
- *yield*, suggests to the scheduler that the currently executing thread need not run at present time, but the final scheduling decision still lies with the scheduler.

**Deadlocks**

Deadlocks can occur between two threads and two locks. Each thread has already acquired a lock, but simultaneously wants to acquire the lock that is in the other thread's possession.
- the programmer is responsible for avoiding deadlocks. Runtime system can neither detects nor prevents deadlocks.
- A common technique to overcome this is resource ordering: always acquire locks from a fixed ordering of objects across all threads. This prevents unwanted blocking and resource contention.

**Ending Thread Execution**

Via Thread Cancelling
- sending interrupt to a thread does not halt its execution.
- the interrupted thread is responsible for checking for interruption and performs the necessary clean up. Certain IO operations can throw interrupted exception while some cannot.
- if the thread is currently executing *wait* or *sleep*, then receiving an interrupt signal will cause the thread to throw an *InterruptedException*.
- a common design pattern is to catch an interrupted exception in a method, and re-interrupt the thread in the catch clause. This is necessary as catching the exception clears the interrupted status of the thread. Also, re-interrupting instead of throwing an exception, allows the method caller to handle the interruption in its own implementation instead of having all callers handle an exception.

Via Waiting
- a thread can wait for another thread to terminate using *join* method on another thread, which will waits indefinitely until the target thread terminates.
- if *join* returns, thread is guaranteed that the target has successfully returned from the *run* method.
- *join* comes in different variant that will return once a given time has elapsed, or when target thread terminates, which ever occurs first.

**Ending Application Execution**

- each application starts with one thread that executes the Java *main* method.
- the main thread can create other user threads or daemon threads (you can use setDaemon(true) on thread creation).
- user threads are part of application. even when the *main* method exits and the origin thread terminates, the application is still considered to be running until all user threads terminates.
- when the last user thread terminates, all daemon threads will be forced to terminate abruptly.
- *System* or *Runtime* *exit* method can be used to terminate the application by stopping the current execution of JVM. (applications can install special threads to be run before shutdown).
- certain classes inherently creates threads so using these classes may cause the application to persist. Use of *exit* method to terminate the threads may be necessary.

**Memory Model: Synchronization and volatile**

