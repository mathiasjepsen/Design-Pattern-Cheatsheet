# Design-Pattern-Cheatsheet

#### Table of contents
[Singleton](#singleton)

## Singleton
Ensures that only one instance of a class is created and provides a global access point to the object.

```java
public class Canteen {
    
    private static Canteen canteen;
    
    public static synchronized Canteen getCanteen() {
        if (canteen == null) {
            canteen = new Canteen();
        }
        
        return canteen;
    }

    private Canteen() {}
    
}
```

A use of this could be if the canteen were to have a stock of items, then when a student buys something, everybody will see the stock decrease because there's only one instance of the Canteen. These methods will need to have the synchronized keyword or in some other way made to handle concurrency, or else race conditions might occur. 

## Factory
Creates objects without exposing the instantiation logic to the client and refers to the newly created object through a common interface.

```java

```
