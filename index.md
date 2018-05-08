---
layout: "index"
author: "Jens Piegsa"
title: "JUnit 5"
summary: "JUnit 5. Goin green"
---

[![Donate](https://img.shields.io/badge/Donate-PayPal-blue.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=WZJTZ3V8KKARC)
[![Fork on GitHub](https://img.shields.io/github/forks/JensPiegsa/junit-going-green.svg?style=flat&label=Fork%20on%20GitHub&color=blue)](https://github.com/JensPiegsa/junit-going-green#fork-destination-box)
[![Issues](https://img.shields.io/github/issues-raw/JensPiegsa/junit-going-green.svg?style=flat&label=Comments%2FIssues)](https://github.com/JensPiegsa/junit-going-green/issues)

//[junit](http://junit.jens-piegsa.com/).[jens-piegsa.com](http://jens-piegsa.com/)/

# Migration from JUnit 4 to JUnit 5

## Introduction

### Do I have to migrate all tests?

* like in the transition from version three to four JUnit 4 and JUnit 5 tests can co-exist, in general there is no need to migrate all tests immediately

## Recipes

### Replace @Test imports

* `import org.junit.Test;` gets `import org.junit.jupiter.api.Test;`

### Replace HierarchicalContextRunner with @Nested

* instead of `@RunWith(HierarchicalContextRunner.class)` on the top-level class, annotate all [`@Nested`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Nested.html) classes

### Use @DisplayName("...")

* describe top-level test classes, @Nested test classes and @Test methods via `@DisplayName("...")` (from package org.junit.jupiter.api) to better communicate tested behaviour

### Transform @Test(expected = *Exception.cass) to `assertThrows(...)`

* new [`@Test`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Test.html) annotation no longer carries expected exception, instead use [`assertThrows(...)`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Assertions.html#assertThrows(java.lang.Class,org.junit.jupiter.api.function.Executable)):

```java
@Test
void simpleExceptionTesting() {
    assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException("a message");
    });
}
```

or

```java
@Test
void advancedExceptionTesting() {
    final Throwable exception = 
            assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException("a message");
    });
    assertEquals("a message", exception.getMessage());
}
```

# Additional material

* [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

---

## Contribute
{: .js-toc-ignore }

Feel free to fork this project, send me pull requests, and issues through the project's [GitHub page](https://github.com/JensPiegsa/junit-goin-green).
