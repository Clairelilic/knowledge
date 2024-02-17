# knowledge# Object-oriented Design 

## Fundamental Concepts of OOP

| Concept       | Description                                                                                                                                                            | Benefits                                                                                                                                                              |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Encapsulation | -Bundling data and methods into a single class to restrict access to the object's components. <br>-Key words: public, private and protected. <br>-By encapsulation, you can't access data directly so as to keep the data integrity. You can enforce constraints, perform validation, or execute additional logic before accessing or modifying data.  <br>-e.g. class person,private valuables:name & age, public method:  getter & setter.                   | - Data Hiding<br>- Reduce Complexity<br>- Increase Flexibility                                                                                               |
| Abstraction   | -Focusing on the essential qualities of an object and hide inplementation details; <br>-Abstraction is achieved through the use of abstract classes, interfaces, and inheritance.  e.g. public abstract class Vehicle, public class truck extends Vehicle  | - Simplification<br>- Modularity<br>- Reusability                                                                                                            |
| Inheritance   | -Allowing a child class to take on the properties and methods of an existing parent class. <br>-e.g. class Animal, class Dog extends Animal                                                                             | - Reusability<br>- Hierarchical Class<br>- Overriding                                                                                          |
| Polymorphism  | Enabling objects of different classes to be treated as objects of a common superclass; <br>-Keywords: upcasting & downcasting <br>-e.g. Animal dog = new Dog(); Dog dog = new Dog() | - Flexibility<br>- Simplicity<br>- Reusability                                                                                                          |

The fundamental concepts of object-oriented programming (OOP) serve as the building blocks for design patterns.

## Types of Design Patterns

### 1.Creational

Creational patterns are focused on ways to instantiate objects or groups of objects. They can help manage the 
creation process, making a system more independent of how its objects are created, composed, and represented.

**1.1 Singleton**

Ensures a class has only one instance and provides a global point of access to it.

class Singleton:
private static instance: Singleton

```
private constructor() {}

public static getInstance():
    if (instance is null):
        instance = new Singleton()
    return instance
```

**1.2 Factory**

Defines an interface for creating an object but lets subclasses decide which class to instantiate. Factory Method 
lets a class defer instantiation to subclasses.


```
interface Product {}

class ConcreteProductA implements Product {}
class ConcreteProductB implements Product {}

abstract class Creator {
    abstract method createProduct(): Product
}

class ConcreteCreatorA extends Creator {
    method createProduct(): Product {
        return new ConcreteProductA()
    }
}

class ConcreteCreatorB extends Creator {
    method createProduct(): Product {
        return new ConcreteProductB()
    }
}

```

**Factory vs. Overriding constructor in subclasses**

| Aspect                             | Factory                                                                   | Overriding constructor in subclasses                                |
|------------------------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| **Flexibility in Object Creation** | High flexibility, as the pattern can return different subclasses based on the input or context. | Lower flexibility, as the subclass type is fixed once instantiated. |
| **Encapsulation of Creation Logic**| Encapsulates object creation logic within a method, separating the instantiation logic from the client code. | Mixes object creation logic with object initialization, coupling instantiation details with class definition. |
| **Use Case**                       | Useful when there are multiple derived classes and the decision about which class to instantiate might change dynamically. | Suitable when a subclass needs to extend or modify only the initialization behavior of its base class. |
| **Code Reusability**               | Promotes reusability and scalability by abstracting the creation process into creator classes or methods. | May lead to code duplication if similar initialization logic is needed across multiple subclasses. |
| **Complexity**                     | Adds a layer of abstraction which may introduce additional complexity if not needed.    | Simpler and more straightforward for basic customization needs.     |


### 2.Structural

Structural patterns are concerned with how classes and objects are composed to form larger structures. They help 
ensure that if one part of a system changes, the entire system doesn't need to do the same.

**2.1 Adapter**

Allows objects with incompatible interfaces to work together. It acts as a bridge between two incompatible 
interfaces. This pattern involves a single class, called the Adapter, which joins functionalities of independent or 
incompatible interfaces.

**Components**

- **Target** Interface: This is the interface that the client expects or uses.
- **Adaptee**: This is the class that has the existing behavior but an incompatible interface.
- **Adapter**: This is the class that implements the Target interface and contains a reference to an Adaptee object. It 
  translates calls to the Target interface into calls to the Adaptee's interface.

```
INTERFACE Target {
  METHOD request()
}

CLASS Adaptee {
  METHOD specificRequest() {
    // Implementation for a specific request
  }
}

CLASS Adapter IMPLEMENTS Target {
  PRIVATE adaptee: Adaptee

  CONSTRUCTOR(adaptee: Adaptee) {
    THIS.adaptee = adaptee
  }

  // Implement the Target interface request method
  METHOD request() {
    // Translate (adapt) the Target interface request to the Adaptee's specific request
    adaptee.specificRequest()
  }
}

CLASS Client {
  METHOD run(target: Target) {
    // The client expects to call the request method on the Target interface
    target.request()
  }
}

// Usage
adaptee = NEW Adaptee()
adapter = NEW Adapter(adaptee)
client = NEW Client()

client.run(adapter)
```

**2.2 Composite**

In scenarios where you need to treat individual objects and compositions of objects uniformly. It allows you to 
compose objects into tree structures to represent part-whole hierarchies.

**Components**

- **Component**: An interface or abstract class with operations that are common to both simple and complex elements of 
the tree.
- **Leaf**: Represents leaf objects in the composition. A leaf has no children.
- **Composite**: Defines behavior for components having children. Stores child components and implements child-related 
  operations in the Component interface.

```
// Component Interface
INTERFACE Component {
  FUNCTION operation()
}

// Leaf Class: Represents end objects of a composition. Cannot have children.
CLASS Leaf IMPLEMENTS Component {
  FUNCTION operation() {
    PRINT "Leaf operation"
  }
}

// Composite Class: Can contain other Leaf objects or Composite objects.
CLASS Composite IMPLEMENTS Component {
  PRIVATE children = []

  // Add a child (Leaf or Composite) to the collection
  FUNCTION add(child: Component) {
    children.ADD(child)
  }

  // Remove a child from the collection
  FUNCTION remove(child: Component) {
    children.REMOVE(child)
  }

  // Executes operation for Composite itself and its children
  FUNCTION operation() {
    PRINT "Composite operation"
    FOR EACH child IN children {
      child.operation()
    }
  }
}

// Example Usage
FUNCTION main() {
  // Create Leaf objects
  leaf1 = NEW Leaf()
  leaf2 = NEW Leaf()

  // Create a Composite object
  composite = NEW Composite()
  
  // Add Leaf objects to the Composite object
  composite.add(leaf1)
  composite.add(leaf2)
  
  // Create another Composite and add the first Composite as a child
  rootComposite = NEW Composite()
  rootComposite.add(composite)
  
  // Perform operation on the root composite, which cascades to its children
  rootComposite.operation()
}

// Run the example
main()
```

### 3.Behavioral

Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects. They 
describe not just patterns of objects or classes but also the patterns of communication between them.

**3.1 Observer**

Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are 
notified and updated automatically.

```
class Subject {
    private observers: list of Observer = []

    public attach(observer: Observer):
        this.observers.add(observer)

    public detach(observer: Observer):
        this.observers.remove(observer)

    public notify():
        for (observer in this.observers):
            observer.update()

class Observer {
    public update():
        // Update implementation
}
```

**3.2 Strategy**

Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm
vary independently from clients that use it.

```
interface Strategy {
  execute(data: Data): Result
}

class ConcreteStrategyA implements Strategy {
  execute(data: Data): Result {
    // Implement algorithm A
  }
}

class ConcreteStrategyB implements Strategy {
  execute(data: Data): Result {
    // Implement algorithm B
  }
}

// Context class that uses a Strategy
class Context {
  private strategy: Strategy

  constructor(strategy: Strategy) {
    this.strategy = strategy
  }

  setStrategy(strategy: Strategy) {
    this.strategy = strategy
  }

  executeStrategy(data: Data): Result {
    return this.strategy.execute(data)
  }
}

// Usage
let data = new Data()
let context = new Context(new ConcreteStrategyA())
let resultA = context.executeStrategy(data)

context.setStrategy(new ConcreteStrategyB())
let resultB = context.executeStrategy(data)
```
This `Context` class maintains a reference to a Strategy object. It can be configured with a ConcreteStrategy 
instance to use the specific algorithm. The context delegates the work to the encapsulated strategy instead of implementing multiple 
versions of the algorithm on its own.

## SOLID Principles of OOD

| Principle            | Acronym | Description                                                                                                                |
|----------------------|---------|----------------------------------------------------------------------------------------------------------------------------|
| Single Responsibility| S       | A class should have only one reason to change, meaning it should have only one job or responsibility.            |
| Open/Closed          | O       | Objects or entities should be open for extension but closed for modification, allowing systems to be extended without altering existing code. |
| Liskov Substitution  | L       | Subtypes must be substitutable for their base types without altering the correctness of the program.                       |
| Interface Segregation| I       | Clients should not be forced to depend upon interfaces they do not use. This principle suggests splitting large interfaces into smaller, more specific ones. <br>e.g.  Animal interface with methods: walk(), swim(). However, not all animals can walk, swim. Instead of having a single monolithic interface, we can segregate the interfaces into smaller, cohesive ones such as Walkable, Swimmable, and Flyable, and have classes implement only the interfaces they need.
| Dependency Inversion | D       | High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. |


### How SOLID Principles Help

- **Maintainability**: Following SOLID principles leads to a design that is easier to maintain because it reduces 
dependencies and makes the system more modular.
- **Scalability**: With principles like OCP, systems become more scalable since new functionality can be added with 
  minimal changes to the existing code.
- **Reusability**: The principles encourage the development of reusable components by ensuring that components have 
  single responsibilities and by promoting the use of interfaces.
- **Testability**: A design adhering to SOLID principles is easier to unit test. For example, the SRP ensures that 
  classes have only one reason to change, simplifying the tests for those classes.

## References

ChatGPT 4
