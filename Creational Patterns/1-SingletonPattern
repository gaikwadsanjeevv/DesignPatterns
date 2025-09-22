## Singleton Pattern  
1. Concept Explanation  

Definition: A design pattern that ensures a class has only one instance in the entire application and provides a global point of access to it.  

Purpose:  
To save memory (don’t create multiple objects unnecessarily).  
To maintain shared state across the application.  
In Spring Boot: All beans are Singleton by default (unless you change the scope).  

#### (a) Classic Java Singleton (without Spring)  
```java
public class DatabaseConnection {
    // Step 1: Create a private static instance
    private static DatabaseConnection instance;

    // Step 2: Private constructor (no external instantiation)
    private DatabaseConnection() {
        System.out.println("Database Connection created!");
    }

    // Step 3: Public static method to provide global access
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection(); // lazy initialization
        }
        return instance;
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        DatabaseConnection c1 = DatabaseConnection.getInstance();
        DatabaseConnection c2 = DatabaseConnection.getInstance();

        System.out.println(c1 == c2); // true → same object
    }
}
✔️ Output: Only one instance is created, reused every time.
```  
#### (b) Singleton in Spring Boot (Default Scope)  

```java
// ==========================
// File: MyService.java
// ==========================

// Importing Spring annotation to mark a class as a Spring-managed component (bean)
import org.springframework.stereotype.Component;

// @Component tells Spring: "Create an object of this class and manage it as a bean in the IoC container"
// By default, this bean will have a Singleton scope (only one instance for the whole application)
@Component
public class MyService {

    // A simple business logic method that will be called by other classes
    public void serve() {
        System.out.println("Service is running...");
    }
}


// ==========================
// File: MyController.java
// ==========================

// Import for @Autowired (used for dependency injection)
import org.springframework.beans.factory.annotation.Autowired;
// Import for @Component (to mark this class as a Spring bean)
import org.springframework.stereotype.Component;

// @Component makes this class a Spring bean, so that Spring can detect it during component scanning
@Component
public class MyController {

    // @Autowired tells Spring: "Find a matching bean of type MyService from the container and inject it here"
    // Since MyService is annotated with @Component, Spring will create its bean and inject the SAME instance here.
    @Autowired
    private MyService service1;

    // Another @Autowired field of the same type (MyService).
    // Because Spring beans are Singleton by default, service1 and service2 will point to the SAME object in memory.
    @Autowired
    private MyService service2;

    // A method to check whether service1 and service2 are pointing to the same bean instance.
    // It prints "true" because Spring reuses the same singleton bean for both injections.
    public void checkSingleton() {
        System.out.println(service1 == service2); 
        // Output: true → proves that MyService bean is a Singleton (default scope in Spring)
    }
}


🔹 Flow of What Happens at Runtime

When the Spring Boot application starts, the IoC (Inversion of Control) container is initialized.
It scans for all classes annotated with @Component, @Service, @Repository, @Controller etc.
It finds MyService → creates one Singleton instance and registers it as a bean.
It finds MyController → creates a Singleton instance for it too.
When constructing MyController, Spring sees the @Autowired fields:
Finds a MyService bean in the container.
Injects the SAME MyService object into both service1 and service2.
Calling checkSingleton() prints true → confirming Singleton behavior.
```

✅ So to summarize:  
@Component → Registers MyService as a bean in IoC container.  
@Autowired → Requests Spring to inject that bean wherever needed.  
Result → You see both Singleton behavior and Dependency Injection in action.  

