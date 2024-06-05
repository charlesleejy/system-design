
## Object-Oriented Programming (OOP) in Java

Object-Oriented Programming (OOP) is a programming paradigm that uses objects and classes to design and develop applications. Java is a widely-used object-oriented programming language that supports the principles of OOP, including encapsulation, inheritance, polymorphism, and abstraction. Here’s an explanation of these principles with examples:

### 1. Classes and Objects

Class:
- A blueprint for creating objects.
- Defines the properties (fields) and behaviors (methods) that the objects created from the class will have.

Object:
- An instance of a class.
- Represents a specific implementation of the class.

```java
// Define a class
public class Car {
    // Fields (attributes)
    private String model;
    private int year;
    private double price;

    // Constructor
    public Car(String model, int year, double price) {
        this.model = model;
        this.year = year;
        this.price = price;
    }

    // Methods (behaviors)
    public void displayInfo() {
        System.out.println("Model: " + model);
        System.out.println("Year: " + year);
        System.out.println("Price: $" + price);
    }

    // Getter and Setter methods
    public String getModel() {
        return model;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}

// Main class to create and manipulate objects
public class Main {
    public static void main(String[] args) {
        // Create an object of the Car class
        Car car1 = new Car("Toyota Camry", 2021, 24000.00);
        
        // Call methods on the object
        car1.displayInfo();
        
        // Modify object properties
        car1.setPrice(23000.00);
        
        // Display updated info
        car1.displayInfo();
    }
}
```

### 2. Encapsulation

Encapsulation:
- The practice of keeping fields (data) within a class private and providing public methods to access and modify those fields.
- Helps protect the integrity of the data and hide the internal implementation details.

```java
// Example of encapsulation in the Car class above
// Fields are private, and public getter and setter methods are provided
```

### 3. Inheritance

Inheritance:
- A mechanism where one class (subclass) can inherit the fields and methods of another class (superclass).
- Promotes code reuse and establishes a natural hierarchy.

```java
// Define a superclass
public class Vehicle {
    private String brand;

    public Vehicle(String brand) {
        this.brand = brand;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public void displayBrand() {
        System.out.println("Brand: " + brand);
    }
}

// Define a subclass that inherits from Vehicle
public class Car extends Vehicle {
    private String model;
    private int year;

    public Car(String brand, String model, int year) {
        super(brand); // Call the constructor of the superclass
        this.model = model;
        this.year = year;
    }

    public void displayInfo() {
        displayBrand();
        System.out.println("Model: " + model);
        System.out.println("Year: " + year);
    }
}

// Main class to demonstrate inheritance
public class Main {
    public static void main(String[] args) {
        Car car1 = new Car("Toyota", "Camry", 2021);
        car1.displayInfo();
    }
}
```

### 4. Polymorphism

Polymorphism:
- The ability of a single interface to represent different underlying forms (data types).
- Enables a method to perform different functions based on the object that it is acting upon.
- Achieved through method overriding and method overloading.

Method Overriding:
- When a subclass provides a specific implementation of a method that is already defined in its superclass.

```java
// Define a superclass
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// Define a subclass that overrides the makeSound method
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

// Main class to demonstrate polymorphism
public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Animal(); // Animal reference and object
        Animal myDog = new Dog();       // Animal reference but Dog object

        myAnimal.makeSound(); // Outputs: Animal makes a sound
        myDog.makeSound();    // Outputs: Dog barks (polymorphic behavior)
    }
}
```

Method Overloading:
- When multiple methods have the same name but different parameters.

```java
// Define a class with overloaded methods
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}

// Main class to demonstrate method overloading
public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("Sum of integers: " + calc.add(5, 3));      // Outputs: 8
        System.out.println("Sum of doubles: " + calc.add(5.5, 3.5));   // Outputs: 9.0
    }
}
```

### 5. Abstraction

Abstraction:
- The concept of hiding the complex implementation details and showing only the essential features of an object.
- Achieved using abstract classes and interfaces.

Abstract Class:
- A class that cannot be instantiated and may contain abstract methods (methods without implementation).

```java
// Define an abstract class
public abstract class Shape {
    private String color;

    public Shape(String color) {
        this.color = color;
    }

    // Abstract method (no implementation)
    public abstract double area();

    public String getColor() {
        return color;
    }
}

// Subclass that provides implementation of the abstract method
public class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

// Main class to demonstrate abstraction
public class Main {
    public static void main(String[] args) {
        Shape shape = new Circle("Red", 5.0);
        System.out.println("Color: " + shape.getColor());
        System.out.println("Area: " + shape.area());
    }
}
```

Interface:
- A reference type in Java that can contain only constants, method signatures, default methods, static methods, and nested types.
- Interfaces cannot contain implementation code for methods (except for default methods and static methods).

```java
// Define an interface
public interface Drawable {
    void draw();
}

// Class implementing the interface
public class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

// Main class to demonstrate interface implementation
public class Main {
    public static void main(String[] args) {
        Drawable drawable = new Circle();
        drawable.draw();
    }
}
```

### Conclusion

Object-Oriented Programming in Java is a fundamental paradigm that allows developers to create modular, reusable, and maintainable code. By understanding and applying the principles of encapsulation, inheritance, polymorphism, and abstraction, developers can build robust and scalable Java applications. The examples provided illustrate how these principles can be implemented in Java code, forming the basis for designing complex software systems.