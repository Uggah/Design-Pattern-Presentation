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

<!-- _class: lead -->
<!-- _footer: '' -->
<!-- _paginate: false -->

# Solution

==> **TEMPLATE METHOD PATTERN**

---


```java
public abstract Image loadImage(byte[] imageBytes);

public abstract Image shrinkResolution(Image image);

public abstract Image handleAlphaChannel(Image image);

public abstract byte[] export(Image image);

public final byte[] optimize(byte[] imageBytes) { // <-- template method
    Image image = loadImage(imageBytes);
    image = shrinkResolution(image);
    image = handleAlphaChannel(image);
    return export(image);
}
```

---

<!-- _class: lead -->

## Hollywood Principle

"Don't call us, we call you"

---

<!-- _class: lead -->

**=> Not every image format needs alpha channel to be dealt with (i.e. WebP)**

---

```java
protected abstract Image loadImage(byte[] imageBytes);

protected abstract Image shrinkResolution(Image image);

protected Image handleAlphaChannel(Image image) { // <-- hook method
    return image // DEFAULT: Do nothing
}

protected abstract byte[] export(Image image);

public final byte[] optimize(byte[] imageBytes) { // <-- template method
    Image image = loadImage(imageBytes);
    image = shrinkResolution(image);
    image = handleAlphaChannel(image);
    return export(image);
}
```

---

<!-- _class: lead -->

**Template Method Pattern = Security Pattern?**
Another example: Handling and securing http requests

---

```java
private final boolean authenticateHttpRequest(HttpRequest) {
    // IMPLEMENTATION
}

private final boolean authorizeHttpRequest(HttpRequest httpRequest, Role neededRole) {
    // IMPLEMENTATION
}

protected abstract HttpResponse handleHttpRequestInternal(HttpRequest httpRequest);

public final HttpResponse handleHttpRequest(HttpRequest httpRequest) {
    if (!authenticateHttpRequest(httpRequest)) {
        return new HttpResponse(401) // Unauthorized
    }

    if (!authorizeHttpRequest(httpRequest, Role.ADMIN)) {
        return new HttpResponse(403) // Forbidden
    }

    handleHttpRequestInternal(httpRequest);
}
```

---

# Participants and Responsibilities

| Abstract class | Concrete class |
| --- | --- |
| Implements template method | Implements abstract methods defined in abstract class |
| Can implement final methods --> Forces child classes to have a certain behaviour |  |

---

# Consequences

- The algorithm always has to follow the same order of execution
- Algorithm implementations may not require any additional steps
    - Workaround: Hooks


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

Pankaj: Template Method Design Pattern in Java. August 3, 2022. Accessed from: https://www.digitalocean.com/community/tutorials/template-method-design-pattern-in-java on March 28, 2023.