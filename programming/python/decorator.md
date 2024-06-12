## Decorators

Decorators in Python are a powerful and expressive tool that allows you to modify or extend the behavior of functions or methods. They are often used to add functionality in a clean, readable, and reusable way.

### What is a Decorator?

A decorator is a function that wraps another function or method to extend or alter its behavior. The wrapping function, called the decorator, takes the original function as an argument and returns a new function that adds some functionality.

### Basic Syntax

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper
```

To apply this decorator to a function, you use the `@` symbol above the function definition:

```python
@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

### Example: Simple Decorator

Here is a simple example of a decorator that logs the execution of a function:

```python
def log_execution(func):
    def wrapper(*args, **kwargs):
        print(f"Executing {func.__name__} function")
        result = func(*args, **kwargs)
        print(f"Finished executing {func.__name__} function")
        return result
    return wrapper

@log_execution
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
```

#### Explanation

- **log_execution Decorator**:
  - Takes a function `func` as an argument.
  - Defines an inner function `wrapper` that logs before and after calling `func`.
  - The `wrapper` function calls `func` with any arguments (`*args` and `**kwargs`) and logs the result.
  - Returns the `wrapper` function.

- **Applying the Decorator**:
  - The `@log_execution` syntax applies the decorator to the `greet` function.
  - When `greet("Alice")` is called, the `wrapper` function is executed, adding logging around the call to `greet`.

#### Output

```
Executing greet function
Hello, Alice!
Finished executing greet function
```

### Example: Decorator with Arguments

Sometimes, you might want to pass arguments to your decorator. This can be achieved by adding another level of function wrapping.

```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello!")

say_hello()
```

#### Explanation

- **repeat Decorator Factory**:
  - The `repeat` function takes an argument `n` and returns a decorator.
  - The returned `decorator` function takes the original function `func` and returns a `wrapper` function.
  - The `wrapper` function calls `func` `n` times.

- **Applying the Decorator**:
  - The `@repeat(3)` syntax applies the `repeat` decorator with `n` set to `3` to the `say_hello` function.
  - When `say_hello()` is called, the `wrapper` function executes `say_hello` three times.

#### Output

```
Hello!
Hello!
Hello!
```

### Example: Using Built-in Decorators

Python comes with some built-in decorators, such as `@staticmethod`, `@classmethod`, and `@property` for use with classes.

```python
class MyClass:
    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, new_value):
        self._value = new_value

obj = MyClass(42)
print(obj.value)  # Uses the getter, outputs: 42
obj.value = 100   # Uses the setter
print(obj.value)  # Outputs: 100
```

#### Explanation

- **@property**:
  - Makes the `value` method act like a property, so it can be accessed without parentheses.
- **@value.setter**:
  - Allows the `value` property to be set using `obj.value = new_value`.

### Conclusion

Decorators are a powerful feature in Python that allow you to modify the behavior of functions or methods in a reusable and readable way. They can be simple, like logging function calls, or complex, like managing access control or caching results. By understanding and using decorators, you can write cleaner and more maintainable code.