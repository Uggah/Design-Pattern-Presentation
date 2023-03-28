---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
footer: Lucca Greschner - Design Patterns - SoSe 2023
marp: true
---

# TEMPLATE METHOD PATTERN

---

# Context

Algorithm / functionality is implemented in ordered steps

Example: 
1. load image
1. shrink image resolution
1. deal with alpha-channel
1. export in different format

---

# Problem

- varying, interchangeable implementations are needed
- the order of execution shall be fixed and accessible for other developers

---

# Forces

---

# Solution

---

# Structure



---

# Participants and Responsibilities

| Abstract class | Concrete class |
| --- | --- |
| Implements template method | Implements abstract methods defined in abstract class |
|  | |


---

# Strategies

---

# Consequences

---

<!-- _class: lead -->
<!-- _footer: '' -->
<!-- _paginate: false -->

# Source Code Examples

---

## Java - InputStream

```java
public abstract class InputStream implements Closeable {
    {...}

    public abstract int read() throws IOException;

    public int read(byte[] b) throws IOException { // <--- TEMPLATE METHOD
        return read(b, 0, b.length);
    }

    {...}
}
```

---

# Sources

E. Gamma, R. Helm, J. Vlissides, R. Johnson: Design Patterns. Elements of Reusable Object-Oriented Software. July 1, 1997

Pankaj: Template Method Design Pattern in Java. August 3, 2022. Accessed from: https://www.digitalocean.com/community/tutorials/template-method-design-pattern-in-java on March 28, 2022.