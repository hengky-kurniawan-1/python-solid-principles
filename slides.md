---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: false
info: |
  ## SOLID Principles in Python
  Presentation slides for SOLID principles.
drawings:
  persist: false
transition: slide-left
title: SOLID Principles in Python
---

# SOLID Principles in Python

A brief overview of the SOLID principles in software design, illustrated with Python examples.

---

# What is SOLID?

SOLID is an acronym for five design principles intended to make software designs more understandable, flexible, and maintainable.

<v-clicks>

- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

</v-clicks>

---

# S - Single Responsibility Principle

A class should have one, and only one, reason to change.

<div class="flex gap-8 mt-8">
<div class="flex-1">

```python
class User:
    def __init__(self, name: str):
        self.name = name
    
    def save_to_db(self):
        print(f"Saving {self.name}...")
        
    def send_email(self, msg: str):
        print(f"Emailing {self.name}: {msg}")
```

<p style="color: #ef4444; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's bad:</strong> User class has multiple responsibilities - managing user data, database persistence, and sending emails. Changes to any of these concerns require modifying the User class.</p>

</div>
<div class="flex items-center justify-center">
<span style="color: #ef4444; font-weight: bold; font-size: 1.5rem">❌ Bad</span>
</div>
</div>

---

# S - Single Responsibility Principle

Separate concerns into distinct classes.

<div class="flex gap-8 mt-8">
<div class="flex items-center justify-center">
<span style="color: #22c55e; font-weight: bold; font-size: 1.5rem">✅ Good</span>
</div>
<div class="flex-1">

```python
class User:
    def __init__(self, name: str):
        self.name = name

class UserRepository:
    def save(self, user: User):
        print(f"Saving {user.name}...")

class EmailService:
    def send_email(self, user: User, msg: str):
        print(f"Emailing {user.name}: {msg}")
```

<p style="color: #22c55e; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's good:</strong> Each class has a single responsibility. User handles data, UserRepository handles persistence, EmailService handles notifications. Easy to modify and test independently.</p>

</div>
</div>


---

# O - Open/Closed Principle

Entities should be open for extension, but closed for modification.

<div class="flex gap-8 mt-8">
<div class="flex-1">

```python
class Discount:
    def apply(self, price, type):
        if type == "seasonal":
            return price * 0.9
        elif type == "vip":
            return price * 0.8
```

<p style="color: #ef4444; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's bad:</strong> Adding new discount types requires modifying the existing Discount class. It violates the "closed for modification" principle, risking bugs in existing code.</p>

</div>
<div class="flex items-center justify-center">
<span style="color: #ef4444; font-weight: bold; font-size: 1.5rem">❌ Bad</span>
</div>
</div>

---

# O - Open/Closed Principle

Use inheritance or polymorphism to extend behavior.

<div class="flex gap-8 mt-8">
<div class="flex items-center justify-center">
<span style="color: #22c55e; font-weight: bold; font-size: 1.5rem">✅ Good</span>
</div>
<div class="flex-1">

```python
from abc import ABC, abstractmethod

class Discount(ABC):
    @abstractmethod
    def apply(self, price): pass

class SeasonalDiscount(Discount):
    def apply(self, price): return price * 0.9

class VIPDiscount(Discount):
    def apply(self, price): return price * 0.8
```

<p style="color: #22c55e; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's good:</strong> New discount types are added by creating new subclasses, not modifying existing code. Open for extension, closed for modification. Existing code remains stable.</p>

</div>
</div>

---

# L - Liskov Substitution Principle

Subtypes must be substitutable for their base types.

<div class="flex gap-8 mt-8">
<div class="flex-1">

```python
class Bird:
    def fly(self): print("Flying")

class Penguin(Bird):
    def fly(self):
        raise NotImplementedError(
            "Can't fly!"
        )
```

<p style="color: #ef4444; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's bad:</strong> Penguin breaks the contract of Bird. Code expecting Bird behavior will fail with Penguin. Subclass violates the parent's behavior promise.</p>

</div>
<div class="flex items-center justify-center">
<span style="color: #ef4444; font-weight: bold; font-size: 1.5rem">❌ Bad</span>
</div>
</div>

---

# L - Liskov Substitution Principle

Refactor hierarchy so common interfaces hold true.

<div class="flex gap-8 mt-8">
<div class="flex items-center justify-center">
<span style="color: #22c55e; font-weight: bold; font-size: 1.5rem">✅ Good</span>
</div>
<div class="flex-1">

```python
class Bird:
    def move(self): print("Moving")

class FlyingBird(Bird):
    def fly(self): print("Flying")

class Penguin(Bird):
    def move(self): print("Swimming")
```

<p style="color: #22c55e; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's good:</strong> All subclasses fulfill the Bird contract. Penguin properly implements move(). Subclasses are truly substitutable for their parent type, maintaining predictable behavior.</p>

</div>
</div>

---

# I - Interface Segregation Principle

Clients should not be forced to depend on methods they do not use.

<div class="flex gap-8 mt-8">
<div class="flex-1">

```python
class Worker(ABC):
    @abstractmethod
    def work(self): pass
    @abstractmethod
    def eat(self): pass

class Robot(Worker):
    def work(self): print("Working")
    def eat(self):
        raise Exception("No eat")
```

<p style="color: #ef4444; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's bad:</strong> Robot is forced to implement eat() even though it doesn't need it. Fat interface creates unnecessary dependencies and dummy implementations.</p>

</div>
<div class="flex items-center justify-center">
<span style="color: #ef4444; font-weight: bold; font-size: 1.5rem">❌ Bad</span>
</div>
</div>

---

# I - Interface Segregation Principle

Break down interfaces into smaller, specific ones.

<div class="flex gap-8 mt-8">
<div class="flex items-center justify-center">
<span style="color: #22c55e; font-weight: bold; font-size: 1.5rem">✅ Good</span>
</div>
<div class="flex-1">

```python
class Workable(ABC):
    @abstractmethod
    def work(self): pass

class Eatable(ABC):
    @abstractmethod
    def eat(self): pass

class Robot(Workable):
    def work(self): print("Working")

class Human(Workable, Eatable):
    def work(self): print("Working")
    def eat(self): print("Eating")
```

<p style="color: #22c55e; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's good:</strong> Specific interfaces allow Robot to implement only what it needs (Workable). Human implements both. Lean interfaces, cleaner code, no forced dependencies.</p>

</div>
</div>

---

# D - Dependency Inversion Principle

Depend on abstractions, not concretions.

<div class="flex gap-8 mt-8">
<div class="flex-1">

```python
class LightBulb:
    def turn_on(self): print("On")

class Switch:
    def __init__(self):
        self.bulb = LightBulb()
```

<p style="color: #ef4444; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's bad:</strong> Switch is tightly coupled to LightBulb. Hard to replace LightBulb with Fan or Lamp. High-level logic depends on low-level implementation details.</p>

</div>
<div class="flex items-center justify-center">
<span style="color: #ef4444; font-weight: bold; font-size: 1.5rem">❌ Bad</span>
</div>
</div>

---

# D - Dependency Inversion Principle

Inject dependencies via abstractions.

<div class="flex gap-8 mt-8">
<div class="flex items-center justify-center">
<span style="color: #22c55e; font-weight: bold; font-size: 1.5rem">✅ Good</span>
</div>
<div class="flex-1">

```python
class Switchable(ABC):
    @abstractmethod
    def turn_on(self): pass

class LightBulb(Switchable):
    def turn_on(self): print("On")

class Switch:
    def __init__(self, device: Switchable):
        self.device = device
```

<p style="color: #22c55e; font-size: 0.9rem; margin-top: 0.5rem"><strong>Why it's good:</strong> Switch depends on Switchable abstraction, not concrete implementations. Easily swap LightBulb with Fan or Lamp. Loose coupling, flexible design, easy to test and extend.</p>

</div>
</div>

---

# Summary

Applying SOLID principles leads to:

- **Maintainability**: Easier to fix bugs and update code.
- **Testability**: Easier to write unit tests.
- **Flexibility**: Easier to extend with new features.
- **Scalability**: Easier to manage as the project grows.

---

# Final Thoughts

> SOLID principles are guidelines, not strict rules.
>
> Not every principle must be followed in every situation.
>
> Sometimes, quick and simple solutions are needed in production.
>
> Use your judgment to balance principles with practical needs and constraints.

---

# Let's Connect!

<div class="text-center pt-12">

**Hengky Kurniawan**

<div class="flex justify-center gap-8 pt-8">
  <div>
    <a href="https://github.com/hengky-kurniawan-1" target="_blank" class="flex flex-col items-center gap-2 hover:opacity-75 transition">
      <carbon:logo-github style="width: 40px; height: 40px;" />
      <span class="text-sm">hengky-kurniawan-1</span>
    </a>
  </div>
  <div>
    <a href="https://id.linkedin.com/in/hk0104" target="_blank" class="flex flex-col items-center gap-2 hover:opacity-75 transition">
      <carbon:logo-linkedin style="width: 40px; height: 40px;" />
      <span class="text-sm">hk0104</span>
    </a>
  </div>
</div>

</div>
