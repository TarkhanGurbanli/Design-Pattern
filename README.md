# Design-Pattern

# 📌 Java Design Pattern-lər - Detallı İzah və Nümunələr (Azərbaycan Dilində)

## 📖 Giriş

Design Pattern-lər — proqram təminatı dizaynında təkrarlanan problemlər üçün sınaqdan çıxmış həllərdir. Yəni, proqramçılar illərlə qarşılaşdıqları problemləri eyni və ya oxşar yollarla həll etmək üçün nümunələr (patternlər) yaradıb. Bu pattern-lər proqramın daha oxunaqlı, genişlənə bilən və dəstəklənə bilən olmasına kömək edir.

## 📚 Design Pattern Növləri

Design Pattern-lər əsasən 3 kateqoriyaya bölünür:

1. **Creational (Yaradıcı) Design Patterns**
2. **Structural (Struktural) Design Patterns**
3. **Behavioral (Davranış) Design Patterns**

---

## 🛠️ 1️⃣ Creational (Yaradıcı) Design Pattern-lər

Obyektlərin yaradılması prosesinə nəzarət edən pattern-lərdir. Məqsəd, obyektin necə yaradılacağını müstəqil etməkdir.

**Bu kateqoriyaya aid pattern-lər:**

* Singleton
* Factory Method
* Abstract Factory
* Builder
* Prototype

### 🔹 Singleton Pattern

Bir class-dan yalnız 1 obyektin yaradılmasını təmin edir.

**Nümunə:**

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 🔹 Factory Method Pattern

Obyektlərin yaradılmasını subclass-lara həvalə edir.

**Nümunə:**

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Circle cizildi");
    }
}

class ShapeFactory {
    public Shape getShape(String type) {
        if (type.equalsIgnoreCase("circle")) {
            return new Circle();
        }
        return null;
    }
}
```

### 🔹 Abstract Factory Pattern

Bir-biri ilə əlaqəli factory-lərin yaradılmasını təmin edir.

**Nümunə:**

```java
interface Button {
    void render();
}

class WindowsButton implements Button {
    public void render() {
        System.out.println("Windows Button");
    }
}

interface GUIFactory {
    Button createButton();
}

class WindowsFactory implements GUIFactory {
    public Button createButton() {
        return new WindowsButton();
    }
}
```

### 🔹 Builder Pattern

Çox mürəkkəb obyektlərin addım-addım yaradılmasını təmin edir.

**Nümunə:**

```java
class Product {
    private String partA;
    private String partB;

    public void setPartA(String partA) { this.partA = partA; }
    public void setPartB(String partB) { this.partB = partB; }
}

class ProductBuilder {
    private Product product = new Product();

    public ProductBuilder buildPartA() {
        product.setPartA("A hissəsi");
        return this;
    }

    public ProductBuilder buildPartB() {
        product.setPartB("B hissəsi");
        return this;
    }

    public Product getResult() {
        return product;
    }
}
```

### 🔹 Prototype Pattern

Obyektin surətinin (clone) yaradılması.

**Nümunə:**

```java
class Person implements Cloneable {
    public String name;

    public Person(String name) {
        this.name = name;
    }

    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

---

## 🏗️ 2️⃣ Structural (Struktural) Design Pattern-lər

Sinif və obyektləri birləşdirərək daha böyük strukturlar yaratmaq üçün istifadə olunur.

**Bu kateqoriyaya aid pattern-lər:**

* Adapter
* Bridge
* Composite
* Decorator
* Facade
* Flyweight
* Proxy

### 🔹 Adapter Pattern

Uyğunsuz interfeysləri uyğunlaşdırır.

**Nümunə:**

```java
interface Target {
    void request();
}

class Adaptee {
    void specificRequest() {
        System.out.println("Adaptee methodu");
    }
}

class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void request() {
        adaptee.specificRequest();
    }
}
```

### 🔹 Decorator Pattern

Obyektə əlavə məsuliyyətlər yükləyir, lakin sinfi dəyişmədən.

**Nümunə:**

```java
interface Notifier {
    void send();
}

class EmailNotifier implements Notifier {
    public void send() {
        System.out.println("Email göndərildi");
    }
}

class SMSNotifier implements Notifier {
    private Notifier notifier;

    public SMSNotifier(Notifier notifier) {
        this.notifier = notifier;
    }

    public void send() {
        notifier.send();
        System.out.println("SMS göndərildi");
    }
}
```

---

## 🎭 3️⃣ Behavioral (Davranış) Design Pattern-lər

Obyektlər arasındakı ünsiyyəti və məsuliyyət bölgüsünü müəyyən edir.

**Bu kateqoriyaya aid pattern-lər:**

* Chain of Responsibility
* Command
* Iterator
* Mediator
* Memento
* Observer
* State
* Strategy
* Template Method
* Visitor

### 🔹 Observer Pattern

Bir obyektin vəziyyəti dəyişəndə ona bağlı digər obyektlərə xəbər verir.

**Nümunə:**

```java
interface Observer {
    void update(String message);
}

class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " xəbər aldı: " + message);
    }
}

class Subject {
    private List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}
```

---

## 📌 Nəticə

Bu faylda Java-da ən çox istifadə edilən Design Pattern-ləri kateqoriyalar üzrə bölüb, hər birini nümunə ilə izah etdik. Real layihələrdə bu pattern-lərin tətbiqi kodun strukturlaşdırılması və təkrarlanan problemlərin həlli üçün olduqca vacibdir.

**Əlavə:** Digər pattern-ləri və daha dərindən izahları əlavə etmək mümkündür.

İstəyirsənsə, hər pattern üçün real layihə nümunəsi də yaza bilərəm.

---

## 📃 Müəllif: Sənin Adın

## 📅 Tarix: 2025-05-08

