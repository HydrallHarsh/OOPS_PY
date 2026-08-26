# Python OOP MCQ Quiz

This quiz is designed for Python fundamentals practice. Try the questions first, then check the answer key.

- Choose one option for each question.
- The questions become progressively harder.
- Question numbers are kept continuous so later sets can be appended.

## Questions

### Beginner

**1. What is an object in Python?**

A. Only a user-defined class
B. An instance of a class that contains state and behavior
C. A function that creates variables
D. A module imported from another file

**2. What is the usual purpose of `__init__` in a class?**

A. To destroy an object
B. To define a static method
C. To initialize an object's instance attributes when it is created
D. To make a class abstract

**3. What does `self` refer to inside a normal instance method?**

A. The parent class
B. The current instance receiving the method call
C. The Python interpreter
D. The class name as a string

**4. What is printed by this code?**

```python
class Dog:
    species = "canine"

d1 = Dog()
d2 = Dog()
print(d1.species, d2.species)
```

A. `canine canine`
B. `Dog Dog`
C. `None None`
D. It raises `AttributeError`

**5. Which statement correctly creates an object of class `Car`?**

A. `object Car()`
B. `new Car()`
C. `Car.create()`
D. `Car()`

**6. What is the output?**

```python
class Counter:
    def __init__(self):
        self.value = 0

c = Counter()
c.value += 3
print(c.value)
```

A. `0`
B. `1`
C. `3`
D. An error because attributes cannot be changed

### Intermediate

**7. What is printed?**

```python
class Person:
    def __init__(self, name):
        self.name = name

p = Person("Asha")
print(p.name)
```

A. `Person`
B. `name`
C. `Asha`
D. `None`

**8. Which statement best describes inheritance?**

A. Hiding all attributes from an object
B. Creating a new class that can reuse or extend another class's behavior
C. Calling a function many times
D. Converting a class into a module

**9. What is the output?**

```python
class Animal:
    def speak(self):
        return "animal"

class Cat(Animal):
    def speak(self):
        return "meow"

print(Cat().speak())
```

A. `animal`
B. `meow`
C. `speak`
D. An error because methods cannot be replaced

**10. What does `super()` commonly allow a subclass method to do?**

A. Access the next implementation in the method-resolution order, often the parent implementation
B. Create a new Python process
C. Convert an instance into a class
D. Prevent method overriding

**11. What is printed?**

```python
class Parent:
    def __init__(self):
        self.x = 10

class Child(Parent):
    def __init__(self):
        super().__init__()
        self.x += 5

print(Child().x)
```

A. `5`
B. `10`
C. `15`
D. `AttributeError`

**12. Which option demonstrates polymorphism?**

A. Different classes provide a `draw()` method, and a loop calls `item.draw()` on each object
B. A class has a private variable named `__x`
C. A class inherits from exactly one parent
D. A function has a default argument

**13. What is the main effect of a single leading underscore in `_value`?**

A. It enforces true private access at the interpreter level
B. It marks the name as intended for internal use by convention
C. It makes the value constant
D. It makes the value a class attribute

### Advanced

**14. What is printed?**

```python
class Box:
    count = 0

    def __init__(self):
        Box.count += 1

b1 = Box()
b2 = Box()
print(b1.count, b2.count, Box.count)
```

A. `0 0 0`
B. `1 1 2`
C. `1 2 2`
D. `2 2 2`

**15. What is the output?**

```python
class A:
    def value(self):
        return "A"

class B(A):
    def value(self):
        return "B" + super().value()

print(B().value())
```

A. `A`
B. `B`
C. `BA`
D. `AB`

**16. Which statement about `@classmethod` is correct?**

A. Its first parameter conventionally receives the class (`cls`), and it can be called on the class or an instance
B. It cannot access class attributes
C. Its first parameter must be `self`
D. It can only be called after an object is created

**17. Which statement about `@staticmethod` is correct?**

A. Python automatically passes `self` to it
B. Python automatically passes `cls` to it
C. It receives no automatically supplied instance or class reference
D. It can only be defined outside a class

**18. What happens when `Shape()` is created here?**

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

A. It creates a normal object with `area` returning `None`
B. It raises `TypeError` because `Shape` has an unimplemented abstract method
C. It raises `AttributeError` only when `area()` is called
D. It silently creates a subclass

**19. What does name mangling do to an attribute written as `self.__token` in class `Account`?**

A. It encrypts the value
B. It prevents all access from outside the class
C. It generally changes the internal name to `_Account__token` to reduce accidental overriding
D. It converts the attribute into a property

**20. Given this code, which method is called and what is printed?**

```python
class A:
    def show(self):
        print("A")

class B(A):
    def show(self):
        print("B")

class C(A):
    def show(self):
        print("C")

class D(B, C):
    pass

D().show()
```

A. `A.show`, printing `A`
B. `B.show`, printing `B`
C. `C.show`, printing `C`
D. It raises an MRO error when `show()` is called

## Answer Key And Explanations

**1. B** - An object is a concrete instance of a class. It bundles data (state) and operations (behavior). In Python, values such as integers and functions are objects too.

**2. C** - `__init__` runs after an instance is allocated and is normally used to initialize instance state such as `self.name` or `self.balance`. It is not the object's destructor; `__del__` is associated with finalization.

**3. B** - `self` is the instance on which the method was invoked. In `obj.method()`, Python passes `obj` as the first argument automatically.

**4. A** - `species` is a class attribute. Both instances can read it through the class, so each lookup returns `"canine"`.

**5. D** - Calling the class, `Car()`, constructs and returns an instance (assuming its constructor arguments are satisfied). `new` is not Python syntax for object creation.

**6. C** - The constructor sets `value` to `0`, then `+= 3` updates that instance attribute to `3`.

**7. C** - The argument `"Asha"` is assigned to `self.name` during construction, so `p.name` evaluates to `"Asha"`.

**8. B** - Inheritance lets a derived class reuse, specialize, or extend a base class. A child class receives inherited members unless it overrides them.

**9. B** - `Cat` overrides `Animal.speak`, so normal dynamic dispatch selects `Cat.speak()` and returns `"meow"`.

**10. A** - `super()` returns a proxy that delegates to the next class in the method-resolution order. It is commonly used to extend a parent constructor or method without duplicating it.

**11. C** - `super().__init__()` first creates `self.x = 10`; the child then adds `5`, resulting in `15`.

**12. A** - Polymorphism means code can use a common interface (`draw`) while the concrete object's class determines the implementation that runs.

**13. B** - A single underscore is a non-enforced convention signaling that a name is internal or protected-like. It does not block access.

**14. D** - `Box.count` is shared by the class. The two constructions change it from `0` to `1` and then to `2`. Neither instance has its own `count`, so both `b1.count` and `b2.count` dynamically read the final class value: `2 2 2`.

**15. C** - `B.value()` prefixes `"B"` and then calls the parent implementation through `super()`, which returns `"A"`; together they produce `"BA"`.

**16. A** - A class method receives the class as its first argument (`cls`). It can access or update class-level state and can be invoked as `Class.method()` or through an instance.

**17. C** - A static method is just a function stored in a class namespace. Python does not automatically pass `self` or `cls`; any needed arguments must be explicit.

**18. B** - A class with at least one unimplemented abstract method cannot be instantiated. Calling `Shape()` raises `TypeError` until a concrete subclass implements `area`.

**19. C** - Python name-mangles `__token` to a class-qualified form such as `_Account__token`. This helps avoid accidental name collisions in subclasses; it is not encryption or absolute privacy.

**20. B** - Python computes a method-resolution order for `D(B, C)`. `B` appears before `C`, and `B` inherits `show` from its own class definition, so `B.show()` runs and prints `B`.

## Score Guide

- **0-7:** Review class/object basics, constructors, and attributes.
- **8-14:** Solid foundation; revisit inheritance and method types.
- **15-17:** Strong OOP understanding; check the explanations for subtle lookup rules.
- **18-20:** Ready for more challenging questions involving descriptors, data model methods, and cooperative multiple inheritance.
