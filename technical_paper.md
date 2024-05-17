# SOLID Principles

The SOLID principles were introduced by Robert C. Martin, These principles provide a set of guidelines that helps a developer to write clean and efficient code. The SOLID stands for five principles:

1. Single Responsibility Principle
2. Open/Closed Principle
3. Liskov Substitution Principle
4. Interface Segregation Principle
5. Dependency Inversion Principle

We will try to understand the SOLID principles using `Car` class example.

## 1. Single Responsibility

This principle states that a class should have only one responsibility. And, it should only have one reason to change.

```java
class Car {
    private String name;
    private String company;
    private int fuelPercent;

    public Car(String name, String company,int fuelPercent) {
        // Construtor method
    }

    public void refuel(int amount) {
        //Refuel Method
    }
}
```
In this code, we store the `name`, `company` and `fuelPercent` and added some method `refuel`. However, this code violates the single responsibility principle. To fix this issue, we should implement a separate class that deals with fuel service.

```java
class Car {
    private String model;
    private String manufacturer;

    public Car(String model, String manufacturer) {
        this.model = model;
        this.manufacturer = manufacturer;
    }

    // Getters,Setters and other car-related methods
}

class FuelService {
    private int fuelPercent;

    public FuelService() {
        this.fuelPercent = 0;
    }

    public void refuel(int amount) {
        this.fuelPercent += amount;
    }

    public double getFuelPercent() {
        return fuelPercent;
    }
}
```


## 2. Open/Closed Principle

Classes should be open to extension but closed for modification. By doing this, we are stopping the modification of existing code and causing new bugs.

```java
class Car {
    public void drive() {
        //drive method
    }
}

class ElectricCar extends Car {
    public void drive() {
        // drive method for electric car
    }
}
```

This above code does not fully follow the Open/Closed Principle. In this code, both `Car` and `ElectricCar` classes are modifying the `drive` method. So to make this code follow OCP principle we add interface on both `Car` and `Electric Car`.

```java
interface Drivable {
    void drive();
}

class Car implements Drivable {
    public void drive() {
    }
}

class ElectricCar implements Drivable {
    public void drive() {
    }
}

class CarService {
    public void startDriving(Drivable vehicle) {
        vehicle.drive();
    }
}
```


## 3. Liskov's Substitution Principle

It states that Child classes should be able to replace their parent classes without causing any problems. This means that if you have a class that is a child of another class, it should work just as well as the parent class in any situation, ensuring there are no surprises or errors.

```java
class Car {
    public void refuel(int amount) {
        // refuel method
    }
}

class ElectricCar extends Car {
    @Override
    public void refuel(int amount) {
        throw new Exception("Electric cars don't use fuel");
    }
}
```

In this example, an `ElectricCar` throws an Exception when `refuel` is called, which violates the expectations set by the Car class. We need to ensure that any subclass can be used in place of its superclass without causing unexpected behavior. One way to refactor the code is to introduce an abstraction that distinguishes between vehicles that can be refueled.

```java
abstract class Vehicle {
    // common vehicle method like drive
}

class Car extends Vehicle {
    public void refuel(int amount) {
        // refuel method
    }
}

class ElectricCar extends Vehicle {
    public void recharge(int percent) {
        // recharge method
    }
}
```

## 4. Interface Segregation Principle

The Interface Segregation Principle says that big interfaces should be divided into smaller ones. This way, classes that use these interfaces only need to worry about the specific methods they actually need, making things simpler and more focused.

```java
interface Vehicle {
    void drive();
    void refuel(int amount);
    void recharge(int percent);
}

class Car implements Vehicle {
    public void drive() {
        // drive method
    }

    public void refuel(double amount) {
        // refuel method
    }

    public void recharge(double percent) {
        throw new Exception("Normal Cars cannot be recharged");
    }
}
```
In this example, the `Car` class is forced to implement the recharge method even though cars cannot recharge like electric vehicles do. This violates the Interface Segregation Principle. To make this code follow ISP, we need to split the Vehicle interface into smaller interfaces that determine specific behaviors.

```java
interface Drivable {
    void drive();
}

interface Fuelable {
    void refuel(int amount);
}

interface Rechargeable {
    void recharge(int amount);
}

class Car implements Drivable, Fuelable {
    public void drive() {
        // drive method
    }

    public void refuel(int amount) {
        // refuel method
    }
}

class ElectricCar implements Drivable, Rechargeable {
    public void drive() {
        // drive method
    }

    public void recharge(int amount) {
        // recharge method
    }
}
```

## 5. Dependency Inversion Principle

The Dependency Inversion Principle means that high-level parts of a program shouldn't depend on low-level parts. Instead, both should depend on abstract ideas, making the system more flexible and easier to manage.

```java
class Engine {
    public void start() {
        System.out.println("Engine On");
    }
}

class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();
    }

    public void start() {
        engine.start();
    }
}
```
The `Car` depends directly on the `Engine` class, which is a low-level module or class. There's no interface or abstract class to separate the `Car` dependency from the `Engine` class. To make the code follow DIP, we can introduce an interface.

```java
interface Engine {
    void start();
}

class PetrolEngine implements Engine {
    public void start() {
        System.out.println("Petrol Engine On");
    }
}

class ElectricEngine implements Engine {
    public void start() {
        System.out.println("Electric Engine On");
    }
}

class Car {
    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.start();
    }
}
```

## Conclusion

This paper gave an introduction to the SOLID principles. By following each principle, we were able to write clean and efficient code.


## Reference 
* Anupadhyay, ["SOLID Principles in Programming: Understand With Real Life Examples."](https://www.geeksforgeeks.org/solid-principle-in-programming-understand-with-real-life-examples/).
* Sam Millington, ["A Solid Guide to SOLID Principles."](https://www.baeldung.com/solid-principles)
* Samuel Oloruntoba, Anish Singh Walia, ["SOLID: The First 5 Principles of Object Oriented Design."](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
* Kudvenkat, ["Solid Design Principles."](https://youtube.com/playlist?list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&si=TRQ_yypHtGOIJrFS)