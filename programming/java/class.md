### Class in Java

A class in Java is a blueprint for creating objects. It defines a data structure that contains fields (attributes) and methods (functions) to describe the behavior of the objects created from the class. A class can also include constructors to initialize objects and various types of nested classes and interfaces.

#### Key Components of a Class:

1. **Fields (Attributes)**: Variables that hold the state of an object.
2. **Methods**: Functions that define the behavior of an object.
3. **Constructors**: Special methods to initialize objects.
4. **Access Modifiers**: Keywords that define the accessibility of the class and its members (e.g., public, private, protected).

#### Example of a Class:

```java
public class Person {
    // Fields
    private String name;
    private int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Methods
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }

    // Additional method
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

In this example:
- The `Person` class has two fields: `name` and `age`.
- It includes a constructor to initialize these fields.
- It has getter and setter methods for each field, and a `displayInfo` method to print the person's information.

### Subclass in Java

A subclass is a class that extends another class, called the superclass. The subclass inherits fields and methods from the superclass, allowing for code reuse and the extension of existing functionality. In Java, inheritance is achieved using the `extends` keyword.

#### Key Points:
- **Inheritance**: The subclass inherits non-private members from the superclass.
- **Override**: Subclasses can override methods from the superclass to provide specific implementations.
- **Super Keyword**: The `super` keyword is used to refer to the superclass, often used to call superclass constructors or methods.

#### Example of a Subclass:

```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

// Subclass
public class Employee extends Person {
    private String department;

    public Employee(String name, int age, String department) {
        super(name, age); // Call the constructor of the superclass
        this.department = department;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    // Override the displayInfo method
    @Override
    public void displayInfo() {
        super.displayInfo(); // Call the superclass method
        System.out.println("Department: " + department);
    }
}
```

In this example:
- `Employee` is a subclass of `Person`.
- The `Employee` class inherits the fields and methods of `Person`.
- It adds a new field `department` and overrides the `displayInfo` method.

### Package in Java

A package in Java is a namespace that organizes a set of related classes and interfaces. It helps to avoid class name conflicts, control access, and make the software more modular and manageable.

#### Key Points:
- **Organization**: Packages group related classes and interfaces together.
- **Namespace Management**: Packages help avoid naming conflicts by providing unique namespaces.
- **Access Control**: Packages provide access protection, allowing you to define which classes and interfaces are accessible from other packages.
- **Reusability**: Packages promote code reuse by grouping reusable components.

#### Creating and Using Packages:

1. **Creating a Package**:
   - Use the `package` keyword followed by the package name at the top of your Java source file.

```java
// File: src/com/example/util/Utility.java
package com.example.util;

public class Utility {
    public static void printMessage(String message) {
        System.out.println(message);
    }
}
```

2. **Using a Package**:
   - Import the package using the `import` keyword in the Java source file where you want to use the classes or interfaces from the package.

```java
// File: src/com/example/Main.java
package com.example;

import com.example.util.Utility;

public class Main {
    public static void main(String[] args) {
        Utility.printMessage("Hello, World!");
    }
}
```

In this example:
- `com.example.util` is the package name.
- The `Utility` class is part of the `com.example.util` package.
- The `Main` class is part of the `com.example` package and uses the `Utility` class from the `com.example.util` package by importing it.

### Summary

| Concept   | Description                                                                                         | Example                                                                                               |
|-----------|-----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Class     | A blueprint for creating objects that defines fields, methods, and constructors.                   | `public class Person { ... }`                                                                          |
| Subclass  | A class that extends another class, inheriting its fields and methods.                              | `public class Employee extends Person { ... }`                                                         |
| Package   | A namespace that organizes related classes and interfaces, helping to avoid name conflicts and manage access. | `package com.example.util;` <br> `import com.example.util.Utility;`                                     |

Understanding these core concepts in Java—classes, subclasses, and packages—helps in creating well-organized, maintainable, and reusable code.