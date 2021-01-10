### Java 8
Collection V/S Streams

1. Collection can be modified at any given time. 
Streams can't be modified
2. Elements in the collection can be accessed any order.
Streams can be accessed only in sequence
3. Collections are eagerly constructed. Streams are lazily constructed
4. Collections can be traversed n number of times. Streams can be traversed only once, if we traverse same stream again it throws illegalStateException, stream closed
5. Performs External iteration to iterate through all elements. Performs Internal iteration to iterate through all elements

Advantages of Streams
1. Parallelism using fork-join
2. Thread safety: An operation on a stream produces a result, but does not modify its source, it's immutable in nature, because of this nature stream operations are thread safe
3. Laziness : intermediate operations are not executed until terminal operations are called
4. Short-Circuit Behavior : Short-circuiting will terminate the processing once condition met, examples : anyMatch, allMatch, findFirst, findAny, limit
5. Order of intermediate operations are important in streams, example filter before sort
**Imp method**

*map* : It converts stream from one type to other, intermediate function
*reduce* : Used to reduce content of stream to single value, Terminal function, Syntax: reduce(initailValue, Function)
*mapping* : converts objects from one type of list to other type list
*reducing* : Converts list to single value
*counting* : counts list of values
*summing* : sum of list of elements

### Serialization:
*serialVersionUID* it should be final, static
This is used during the deserialization of an object, to ensure that a loaded class is compatible with the serialized object. If no matching class is found, an InvalidClassException is thrown.
If UID is not defined value will be calculated based on the class and fields, it dangerous if we don't define UID, if any fields changed after serialization, during deserialization id will be calculated based on the new fields which result in to new id and which leads to exception.
<br>

*transient* will not be part of the serialization, when we deserialize that field value be null
static will not be part of the serialization, hence when we deserialize static field value will be variable value which is there during deserilizable

### Garbage collection
How Garbage collection works
JVM Archetcture
Young generation and old generation
RedBlack tree

System.gc() is effectively equivalent to Runtime.gc(). System.gc()internally calls Runtime.gc().
The only difference is System.gc() is a class method where as Runtime.gc() is an instance method. So, System.gc() is more convenient.

Boxing-Autoboxing test

primitive->widening->boxing->vararguments

### Threads

Volatile keyword in Java is used as an indicator to Java compiler and  Thread that do not cache value of this variable and always read it from main memory.

Reentrant means java thread can reuse the same monitor for different synchronized methods if other method is called from the method on the same object.

Reentrant, ReentrantReadWriteLock are examples of extrinsic locking.

Reentrant locks: using lock and readWriteLock
Intrinsic Or Monitor Lock : using synchronized

```java
ReentrantReadWriteLock lock = new ReentrantReadWriteLock ();
```
lock.readLock.lock();
This means that if any other thread is writing (i.e. holds a write lock) then stop here until no other thread is writing.
Once the lock is granted no other thread will be allowed to write (i.e. take a write lock) until the lock is released.

lock.writeLock.lock();
This means that if any other thread is reading or writing, stop here and wait until no other thread is reading or writing.
Once the lock is granted, no other thread will be allowed to read or write (i.e. take a read or write lock) until the lock is released.

**ExecutorService**
1. It is an extension of the Executor interface and provides a facility for returning a Future object
2. ExecutorService provides methods to submit callable tasks -> <T> Future<T> submit(Callable<T> task), Future<?> submit(Runnable task), void execute(Runnable task);
3. ExecutorService provides method to shut down thread pool. Once the shutdown is called, the thread pool will not accept new tasks but complete any pending task.

**Executors** 

This class provides factory methods to create different kinds of thread pools

**newCachedThreadPool**
```java
ExecutorService executorService = Executors.newCachedThreadPool();
```
Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available.

**newScheduledThreadPool**
```java
ScheduledExecutorService executorService = Executors.newScheduledThreadPool(int numOfThreads);
```
Creates a thread pool that can schedule commands to run after a given delay, or to execute periodically. Sample : executorService.schedule(runnableTask,10, TimeUnit.SECONDS);

**newFixedThreadPool**
```java
ExecutorService executorService = Executors.newFixedThreadPool(int numOfThreads);
```
Creates a fixed number of pool of worker threads

**newSingleThreadExecutor**
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
```
Creates a service of single worker thread.

Example: 
```java
class Sample {
    
    ScheduledExecutorService executorService = Executors.newScheduledThreadPool(2);

    Callable callableTask = () -> {
        try {
            System.out.println("callable waiting");
            TimeUnit.MILLISECONDS.sleep(1000);
            System.out.println("callable done");
        } catch (InterruptedException e) {
            //Left empty
        }
        return 1;
    };

    Runnable runnableTask = () -> {
        try {
            System.out.println("runnable waiting");
            TimeUnit.MILLISECONDS.sleep(1000);
            System.out.println("runnable done");
        } catch (InterruptedException e) {
            //Left empty
        }
    };

    public void test() throws ExecutionException, InterruptedException {
        System.out.println("In executor service");
        executorService.schedule(runnableTask,10, TimeUnit.SECONDS);
        Future f = executorService.submit(runnableTask);
        System.out.println("called runnable" + f.get());
        Future f2 = executorService.submit(callableTask);
        System.out.println("called callable" + f2.get());
        executorService.shutdown();
    }
}

public class ExecutorTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        new Sample().test();
    }
}
```

### Marker interfaces
The marker interface pattern is a design pattern in computer science, used with languages that provide run-time type information about objects. It provides a means to associate metadata with a class where the language does not have explicit support for such metadata. In java, it is used as interfaces with no method specified.

Example: Serialization, Cloneable, SingleThreadModel.

In AOP ThrowsAdvice

A good example of use of marker interface in java is a serializable interface. A class implements this interface to indicate that its non-transient data members can be written to a byte steam or file system.

A major problem with marker interfaces is that an interface defines a contract for implementing classes, and that contract is inherited by all subclasses. This means that you cannot “un-implement” a marker. 

In the example given, if you create a subclass that you do not want to serialize (perhaps because it depends on transient state), you must resort to explicitly throwing NotSerializableException.

### Functional interfaces
Any interface with a SAM(Single Abstract Method) is a functional interface, and its implementation may be treated as lambda expressions.

A functional interface may still have multiple default methods.

Examples: All interfaces java.util.function package, thread run method, comparator and comparable methods.
```java
@FunctionalInterface
public interface Consumer<T> {

    void accept(T t);
}
```

### Java oops concepts:
1) Inheritance: Inheritance can be defined as the process where one object acquires the properties of another. With the use of inheritance the information is made manageable in a hierarchical order.
   Type: IS-A and Has-A relationships
   
   
When to use: The subclass Is A super class or Has-A relationship and it makes sense
    Don’t use the inheritance for just to reuse the code or they don’t have Is-A relationship.
    In case of reuse of code use delegation to make the functionalities to work.
    Why to use: Avoid duplicate code
    Changes to the superclass code instantly reflect in subclass
    To promote code reuse if it’s pass is-a test
    To use polymorphism

2) Polymorphism: 
   Polymorphism is the ability of an object to take on many forms. The most common use of polymorphism in OOP occurs when a parent class reference is used to refer to a child class object.
   Type: Overloading and overriding
   Explain Overriding with exception during explaining

3) Abstraction: It's the process of hiding the implementation and complexity. In abstraction, implementation complexities are hidden using abstract classes and interfaces.

4) Encapsulation: It’s the process of hiding the information data and restricting access to the data by using access modifiers and providing getters and setters

Benefits of Encapsulation:
•	The fields of a class can be made read-only or write-only.
•	A class can have total control over what is stored in its fields.
•	The users of a class do not know how the class stores its data. A class can change the data type of a field, and users of the class do not need to change any of their code.

### Imutable object: Making class immutable
In object-oriented and functional programming, an immutable object is an object whose state cannot be modified after it is created.

Any modification on immutable object will result in another immutable object

Immutable objects are good for caching purpose because you don’t need to worry about the value changes. Other benefit of immutable class is that it is inherently thread-safe, so you don’t need to worry about thread safety in case of multi-threaded environment.

Benefits of Immutable Classes in Java
1. Immutable objects are by default thread safe, can be shared without synchronization in concurrent environment.
2. Immutable object simplifies development, because it's easier to share between multiple threads without external synchronization.
3. Immutable object boost performance of Java application by reducing synchronization in code.
4. Another important benefit of Immutable objects is re-usability, you can cache immutable objects and reuse them, much like String literals and Integers.  You can use static factory methods to provide methods like valueOf(), which can return an existing Immutable object from cache, instead of creating a new one.
   Apart from above advantages, immutable object has disadvantage of creating garbage as well
   Example: Strings, All wrapper classes like Integer, Double, java.util.Locale etc
  
   Rules:
    1. Make class final: Can’t be extended, ensure the class cannot be overridden
    2. Make all fields private and final: state can’t be modified
    3. No setter methods: state can’t be modified
    4. Provide only getter method
    5. Public constructor for object creation.

```java
public final class Contacts {

    private final String name;
    private final String mobile;

    public Contacts(String name, String mobile) {
        this.name = name;
        this.mobile = mobile;
    }
   
    public String getName(){
        return name;
    }
   
    public String getMobile(){
        return mobile;
    }
}
```
### Checked v/s Unchecked exceptions

Exception : Checked exceptions need to be declared in a method or constructor's (using throws} clause if they can be thrown by the execution of the method or constructor and propagate outside the method or constructor boundary.
Checked Exception: Handled by developers and thrown during compile time.
Unchecked Exception: Runtime time exceptions.

Exceptions with polymorphism
Checked and unchecked exceptions always should be subclass exceptions of the base class methods defined exception.

Checked exceptions with constructor is reverse of polymorphic methods

Use checked exceptions when the client code can take some useful recovery action based on information in exception. Use unchecked exception when client code cannot do anything. For example, convert your SQLException into another checked exception if the client code can recover from it and convert your SQLException into an unchecked (i.e. RuntimeException) exception, if the client code cannot do anything about it.
use checked exceptions for recoverable conditions and unchecked exceptions for programming errors

### comparator and comparable
What’s difference between comparator and comparable interface in java.
Comparator
1) It's there in java.util.Comparator package			|
2) Uses int compare (Object ob1, Object ob2)
3) We build a class separate from the class whose instances we want to sort.
4) We can create many sort sequences.

Example:
```java
import java.util.*;

class EmployeeSort implements Comparator<Employee> {
    
    public int compare(Employee one, Employee two) {
    return one.getName.compareTo(two.getName);
    }
}
/////
Collection< Employee> list=new ArrayList< Employee>()
EmployeeSort sortPattern=new EmployeeSort ();
Collection.sort(list, sortPattern);
```
Comparable
1) It's there in java.long.* package
2) It overrides the compareTo(Object ob) method.
3) We must modify the class whose instances you want to sort.
4) Only one sort sequence can be created.

Example:
```java
class Employee implements Comparable {
public int compareTo(Object o) { // takes an Object rather
// than a specific type
Employee d = (Employee)o;
return name.compareTo(d.getName());
}}
```
Comparator is useful if many sort sequences are need to be created and the class which we want use is present in different library and it can’t be modified.

### Design Patterns
What's design pattern: It's well defined solution to common problems, Industry standard and language independent.
1. Design patterns are not algorithms.
2. These are language independent strategies for solving common object-oriented design problems.
3. These patterns provide efficient and effective solutions to common problems in software development.
4. Design pattern are based on the base principles of object orientated design.
	Program to an 'interface', not an 'implementation'.
	Favor 'object composition' over 'class inheritance'
   
**Advantages:**

Code robust
Code re-usability
Highly maintainable
Loosely coupled

Design Patterns can be divided into three parts
1.	Creational Patterns: This pattern is used to create objects in a form that they can follow the architecture principle of loose coupling, encapsulation and abstraction
2.	Structural Patterns: This pattern is used to form larger object structures from many different objects
3.	Behavioural Patterns: This pattern is used to manage relationships, interaction, algorithms and responsibilities between various objects