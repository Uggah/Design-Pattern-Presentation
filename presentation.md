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

---

## Example

Example: 
1. load image
1. shrink image resolution
1. deal with alpha-channel
1. export in different format

---

# Problem

- varying, interchangeable implementations are needed
- the order of execution shall be fixed and accessible to other developers

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

"Don't call us, we'll call you"

---

<!-- _class: lead -->

## New Problem:

**=> Not every image format needs alpha channel to be dealt with (e.g. WebP, PNG)**

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
    - It may limit some implementations
- Algorithm implementations may not require any additional steps
    - Workaround: Hooks
- Hard to maintain
    - Changes to template methods oftentimes require the implementation to be revised

---

<!-- _class: lead -->
<!-- _footer: '' -->
<!-- _paginate: false -->

# Source Code Examples

---

<!-- _footer: '' -->

## Java - InputStream

```java
public abstract class InputStream implements Closeable {
    {...}

    public abstract int read() throws IOException;

    {...}

    public int read(byte b[], int off, int len) throws IOException { // <-- TEMPLATE METHOD
        Objects.checkFromIndexSize(off, len, b.length);
        if (len == 0) {
            return 0;
        }

        int c = read(); // Call of the abstract method
        if (c == -1) {
            return -1;
        }
        b[off] = (byte)c;

        int i = 1;
        try {
            for (; i < len ; i++) {
                c = read(); // Second call of the abstract method
                if (c == -1) {
                    break;
                }
                b[off + i] = (byte)c;
            }
        } catch (IOException ee) {
        }
        return i;
    }
    {...}
}
```

---

<!-- _footer: '' -->

# Sources

E. Gamma, R. Helm, J. Vlissides, R. Johnson: Design Patterns. Elements of Reusable Object-Oriented Software. July 1, 1997

Pankaj: Template Method Design Pattern in Java. August 3, 2022. Accessed from: https://www.digitalocean.com/community/tutorials/template-method-design-pattern-in-java on March 28, 2023.

Refactoring Guru by Alexander Shvets: Template Method. Unknown release date. Accessed from: https://refactoring.guru/design-patterns/template-method on March 28, 2023.

IBM Runtime Technologies and contributors: InputStream.java. 2007. Last edited: May 17, 2022. Accessed from: https://github.com/ibmruntimes/openj9-openjdk-jdk19/blob/72f615d98f58bc072887e0869f5a6b189ef1e8c9/src/java.base/share/classes/java/io/InputStream.java#L50 on April 20, 2023.