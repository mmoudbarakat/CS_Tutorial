
## S — Single Responsibility Principle (SRP)

**Idea:**  
A class should have **one job only**.

**Metaphor:**  
A **chef knife** cuts food.  
It doesn’t open cans, wash dishes, or cook.

If a tool does **too many things**, it becomes messy and fragile.

**Software intuition:**  
One reason to change = good design.

```c
Class: InvoiceCalculator
Job: calculate invoice totals

Not:
InvoiceCalculator + EmailSender + PDFGenerator
```

---

## O — Open/Closed Principle (OCP)

**Idea:**  
Software should be **open for extension but closed for modification**.

**Metaphor:**  
A **power strip**.

You don’t rebuild the wall wiring every time you add a device.  
You **plug something new in**.

**Software intuition:**  
Add behavior **without rewriting existing code**.

```c
Add new payment type
→ Create new class
→ Do not modify existing payment logic
```

---

## L — Liskov Substitution Principle (LSP)

**Idea:**  
Subclasses should **behave exactly like their parent type**.

**Metaphor:**  
A **USB cable**.

Anything that fits the USB standard should **work the same**.

Imagine plugging a USB drive that suddenly **starts charging instead of transferring data**.

That's a broken contract.

**Software intuition**

```c
If Dog extends Animal

Code using Animal
should work with Dog
without breaking behavior
```

---

## I — Interface Segregation Principle (ISP)

**Idea:**  
Clients should **not be forced to depend on methods they don't use**.

**Metaphor:**  
A **remote control**.

You don't want **100 buttons** for every possible device.

Instead:

- TV remote
    
- AC remote
    
- Game controller
    

Each tool is **specialized**.

**Software intuition**

Bad:

```c
interface Worker {
  work()
  eat()
  sleep()
}
```

Robot worker doesn't eat.

Better:

```c
interface Workable
interface Eatable
```

---

## D — Dependency Inversion Principle (DIP)

**Idea:**  
High-level modules should depend on **abstractions**, not concrete implementations.

**Metaphor:**  
A **wall power outlet**.

Your laptop depends on **electricity**, not on **a specific power plant**.

You can plug into:

- nuclear
    
- solar
    
- hydro
    

Your device doesn't care.

**Software intuition**

Bad:

```
OrderService → MySQLDatabase
```

Good:

```
OrderService → DatabaseInterface
```

Now you can swap:

- MySQL
    
- PostgreSQL
    
- MongoDB
    
- Mock DB (testing)
    

---

# Ultra-Compressed Memory Version

|Principle|Core Idea|Metaphor|
|---|---|---|
|SRP|One job per class|Chef knife|
|OCP|Extend without modifying|Power strip|
|LSP|Child must behave like parent|USB device|
|ISP|Small focused interfaces|Remote controls|
|DIP|Depend on abstractions|Power outlet|
