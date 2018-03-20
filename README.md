# Design-Pattern-Cheatsheet

#### Table of contents
[Singleton](#singleton)

[Factory](#factory)

[Observer](#observer)

[Command](#command)

[Decorator](#decorator)
___
## Singleton
#### Ensures that only one instance of a class is created and provides a global access point to the object.

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
___
## Factory
#### Creates objects without exposing the instantiation logic to the client and refers to the newly created object through a common interface.

```java

```
___
## Observer
#### Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

```java
public class Canteen extends Observable {

    private final boolean hasPizzaInStock = true;

    public void changePizzaStock(boolean stock) {
        setChanged();
        notifyObservers(stock);
    }

}
```

The Canteen class acts as what's being 'observed', and the field being observed is a boolean of whether or not they have pizza in stock. **setChanged()** marks the Canteen as having been changed, which will make the **hasChanged()** method return true, allowing you to do conditional checks for whether or not a change has occurred. 

**notifyObservers(stock)** lets all observers know of the change, triggering their **update()** method and supplying them with the new value.

```java
public class Student implements Observer {

    private boolean canteenPizzaStock;
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void update(Observable o, Object arg) {
        canteenPizzaStock = (Boolean) arg;
        System.out.println(name + " sees the canteen's pizza stock as: " + canteenPizzaStock);
    }

}
```

The Student class is our 'observer', observing the canteen's stock of pizza. Whenever it changes, the Student will be notified of this and its **update()** method gets called, changing the local variable to the newly received value, and printing out the new stock. 

```java
public class Main {
    
    public static void main(String[] args) {
        Canteen canteen = new Canteen();
        Student lovro = new Student("Lovro");
        Student thomas = new Student("Thomas");
        Student mathias = new Student("Mathias");
        canteen.addObserver(lovro);
        canteen.addObserver(thomas);
        canteen.addObserver(mathias);
        canteen.changePizzaStock(false);
    }
    
}
```
```
Mathias sees the canteen's pizza stock as: false
Thomas sees the canteen's pizza stock as: false
Lovro sees the canteen's pizza stock as: false
```

Testing this out in the main, we see that once **changePizzaStock()** is called, every observer is notified of this and receives the new value.

___
## Command
#### Command decouples the object that invokes the operation from the one that knows how to perform it.

```java
public interface Command {
    void execute();
}
```

The interface Command has a single method **execute()** 

```java
public class Calculator {

    private final List<Integer> calculations = new ArrayList<>();
    private int pointer = 0;

    public int Add(Integer n) {
        if (calculations.size() > 0) {
            int headOfCalcs = calculations.get(pointer);
            int result = headOfCalcs + n;
            calculations.add(result);
            pointer++;
            return result;
        } else {
            calculations.add(n);
            return n;
        }
    }
    ...
}
```

```java
public class Add implements Command {

    private final Calculator calc;
    private final int numToAdd;

    public Add(Calculator calc, int number) {
        this.calc = calc;
        this.numToAdd = number;
    }

    @Override
    public void execute() {
        System.out.println(calc.Add(numToAdd));
    }

}
```

```java
public class Invoker {

    public void run(Command com) {
        com.execute();
    }

}
```

```java
public class ScannerWordUser {

    private final Calculator calc;
    private final Scanner scan = new Scanner(System.in);
    private final Invoker inv = new Invoker();

    public ScannerWordUser(Calculator calc) {
        this.calc = calc;
    }

    public void Run() {
        while (true) {
            System.out.print(">");
            String input = scan.nextLine();
            String operator = input.split(" ")[0];
            int value;
            Command com;

            switch (operator) {
                case "Add":
                    value = Integer.parseInt(input.split(" ")[1]);
                    com = new Add(calc, value);
                    inv.run(com);
                    break;
            }

        }
    }
}
```

___
## Decorator
#### Decorator design pattern is used to modify the functionality of an object at runtime

```java
public interface ComparatorDecorator extends Comparator<Integer> {
    
    int getNumberOfComparisons();
    
}
```

```java
public class ComparatorDecoratorImpl implements ComparatorDecorator {
    
    int comparisons = 0;
    
    @Override
    public int getNumberOfComparisons() {
        return comparisons;
    }

    @Override
    public int compare(Integer o1, Integer o2) {
        comparisons++;
        return o1.compareTo(o2);
    }
    
}
```

```java
public class DecoratorPattern {

    public static void main(String[] args) {
        List<Integer> tempList = new ArrayList<>();
        for (int i = 0; i < 1000000; i++) {
            tempList.add(i, (int) (Math.random() * (99 - 1) + 1) + 1);
        }

        ComparatorDecorator decor = new ComparatorDecoratorImpl();
        Collections.sort(tempList, decor);
        System.out.println(decor.getNumberOfComparisons());
    }

}
```
