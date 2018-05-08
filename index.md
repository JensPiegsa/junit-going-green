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

## JUnit Migration Recipes

### Replace `@Test` imports

* `import org.junit.Test;` becomes [`import org.junit.jupiter.api.Test;`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Test.html)

### Replace `@Before` with `@BeforeEach`

* and [`import org.junit.jupiter.api.BeforeEach;`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeEach.html)

### Replace `@After` with `@AfterEach`

* and [`import org.junit.jupiter.api.AfterEach;`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterEach.html)

### Replace `@BeforeClass` with `@BeforeAll`

* and [`import org.junit.jupiter.api.BeforeAll;`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeAll.html)

### Replace `@AfterClass` with `@AfterAll`

* and [`import org.junit.jupiter.api.AfterAll;`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterAll.html)

### Replace `HierarchicalContextRunner` with `@Nested`

* instead of `@RunWith(HierarchicalContextRunner.class)` on the top-level class, annotate all [`@Nested`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Nested.html) classes

### Use `@DisplayName("...")`

* annotate top-level test classes, `@Nested` test classes and `@Test` methods via [`@DisplayName("...")`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/DisplayName.html) to better describe tested behaviour (you may like to form sentences along the nesting)

### Migrate `@Test(expected = Throwable.class)`

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

* or in [BDD style](https://martinfowler.com/bliki/GivenWhenThen.html) use *catch-exception* like this:

```java
import static com.googlecode.catchexception.apis.BDDCatchException.thenThrown;
import static com.googlecode.catchexception.apis.BDDCatchException.when;

class MyTest {
    @Test
    void catchExceptionTesting() {
        // given
        // ...
        
        when(() -> {
            throw new IllegalArgumentException("a message");
        });
        
        thenThrown(IllegalArgumentException.class);
    }
}
```

which requires this dependency in your pom.xml:

```xml
<dependency>
    <groupId>eu.codearte.catch-exception</groupId>
    <artifactId>catch-exception</artifactId>
    <version>2.0.0-beta-1</version>
    <scope>test</scope>
</dependency>
```

## Mockito migration recipes

### Replace usage of `MockitoJUnitRunner` with `@ExtendWith(MockitoExtension.class)`

* add this dependency to your pom.xml:

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>${mockito.version}</version>
    <scope>test</scope>
</dependency>
```

```java
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class MyTest { ... }
```

# Snippets

## How to assert exception message

```java
import static com.googlecode.catchexception.apis.BDDCatchException.caughtException;
import static com.googlecode.catchexception.apis.BDDCatchException.when;
import static org.assertj.core.api.BDDAssertions.then;

@DisplayName("UserBuilder")
class UserBuilderTest {
	
    @Test
    @DisplayName("should raise IllegalArgumentException on missing username.")
    void shouldRaiseExceptionOnMissingUsername() {

        // given
        final UserBuilder userBuilder = new UserBuilder();

        when(() -> userBuilder.build());

        then(caughtException())
                .isInstanceOf(IllegalArgumentException.class)
                .hasMessageContaining("username");
    }
}
```

* required Maven dependencies in pom.xml:

```xml
<dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-core</artifactId>
    <version>3.9.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>eu.codearte.catch-exception</groupId>
    <artifactId>catch-exception</artifactId>
    <version>2.0.0-beta-1</version>
    <scope>test</scope>
</dependency>		
```

# Additional material

* [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

---

## Contribute
{: .js-toc-ignore }

Feel free to fork this project, send me pull requests, and issues through the project's [GitHub page](https://github.com/JensPiegsa/junit-goin-green).
