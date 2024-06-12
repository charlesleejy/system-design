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


## When to use decorator

Decorators in Python are a powerful and flexible tool that allow you to modify the behavior of functions or classes. They are often used to implement cross-cutting concerns such as logging, access control, instrumentation, caching, and more. Here are some examples of when to use decorators in Python:

### 1. Logging

**Use Case**: Automatically log function calls for debugging or auditing purposes.

```python
def log_function_call(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args: {args}, kwargs: {kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_function_call
def add(a, b):
    return a + b

# Example usage
add(5, 3)
```

### 2. Access Control / Authorization

**Use Case**: Enforce access control policies before executing a function.

```python
def requires_auth(func):
    def wrapper(*args, **kwargs):
        if not kwargs.get('user').is_authenticated:
            raise PermissionError("User is not authenticated")
        return func(*args, **kwargs)
    return wrapper

@requires_auth
def get_secret_data(user):
    return "Secret data"

# Example usage
class User:
    def __init__(self, is_authenticated):
        self.is_authenticated = is_authenticated

user = User(is_authenticated=True)
print(get_secret_data(user=user))  # "Secret data"

unauth_user = User(is_authenticated=False)
print(get_secret_data(user=unauth_user))  # Raises PermissionError
```

### 3. Caching

**Use Case**: Cache the results of expensive function calls to improve performance.

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Example usage
print(fibonacci(10))  # 55
```

### 4. Timing

**Use Case**: Measure the execution time of functions for performance analysis.

```python
import time

def time_it(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time} seconds")
        return result
    return wrapper

@time_it
def slow_function():
    time.sleep(2)

# Example usage
slow_function()  # slow_function took 2.0 seconds
```

### 5. Input Validation

**Use Case**: Validate inputs to a function to ensure they meet certain criteria.

```python
def validate_input(func):
    def wrapper(x):
        if not isinstance(x, int):
            raise ValueError("Input must be an integer")
        return func(x)
    return wrapper

@validate_input
def increment(x):
    return x + 1

# Example usage
print(increment(3))  # 4
print(increment("3"))  # Raises ValueError: Input must be an integer
```

### 6. Rate Limiting

**Use Case**: Limit the rate at which a function can be called to prevent abuse or overuse.

```python
import time

def rate_limit(max_calls_per_minute):
    interval = 60 / max_calls_per_minute
    last_called = [0.0]

    def decorator(func):
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_called[0]
            if elapsed < interval:
                time.sleep(interval - elapsed)
            last_called[0] = time.time()
            return func(*args, **kwargs)
        return wrapper
    return decorator

@rate_limit(max_calls_per_minute=5)
def api_call():
    print("API call made")

# Example usage
for _ in range(10):
    api_call()  # API call will be limited to 5 calls per minute
```

### 7. Context Management

**Use Case**: Implement context management (setup and teardown) for a function.

```python
def context_decorator(func):
    def wrapper(*args, **kwargs):
        print("Entering context")
        result = func(*args, **kwargs)
        print("Exiting context")
        return result
    return wrapper

@context_decorator
def my_function():
    print("Function execution")

# Example usage
my_function()
```

### 8. Retry Logic

**Use Case**: Automatically retry a function if it fails, useful for network operations or flaky dependencies.

```python
import time

def retry(retries=3, delay=1):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for attempt in range(retries):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt + 1} failed: {e}")
                    time.sleep(delay)
            raise Exception(f"All {retries} retries failed")
        return wrapper
    return decorator

@retry(retries=5, delay=2)
def unreliable_function():
    if time.time() % 2 < 1:  # Fails approximately 50% of the time
        raise ValueError("Random failure")
    return "Success"

# Example usage
print(unreliable_function())
```

### Conclusion

Decorators are a versatile feature in Python that can be used to add functionality to functions and methods in a clean, reusable, and readable way. Whether you need to add logging, enforce access control, implement caching, measure performance, validate inputs, limit the rate of function calls, manage context, or add retry logic, decorators provide a powerful and flexible solution.