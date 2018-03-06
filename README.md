# Design-Pattern-Cheatsheet
##### Table of contents
[I'm a reference-style link][Singleton]
..
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
..
.
.
.
.
.
.
.
..
.
.
.
.
.
.
.
..
.
.
.
.
.
.
.
..
.
.
.
.
.
.
.
..
.
.
.
.
.
.
.
.
## Singleton
Ensure that only one instance of a class is created and provide a global access point to the object.

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
