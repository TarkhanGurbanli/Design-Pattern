# Design Patterns

## 📖 Nədir Design Pattern?

Design pattern-lər — proqram təminatı arxitekturasında təkrarlanan problemlərə verilmiş, sınanmış, səlisləşdirilmiş həll yollarıdır. Yəni, hansısa problem qarşısında proqram təminatı aləmində artıq istifadə olunmuş, effektiv bir metod.

## 📦 3 Önəmli Kateqoriya:

### 1️⃣ Creational (Yaradıcı)

Obyektlərin yaradılması prosesi ilə bağlı problemləri həll edir.

* **Singleton**
* **Factory Method**
* **Abstract Factory**
* **Builder**
* **Prototype**

### 2️⃣ Structural (Struktur)

Obyektləri və sinifləri bir-birilə necə birləşdirmək lazım olduğunu təyin edir.

* **Adapter**
* **Decorator**
* **Composite**
* **Proxy**
* **Facade**
* **Bridge**
* **Flyweight**

### 3️⃣ Behavioral (Davranış)

Obyektlər arası əlaqə və davranış modellərini təşkil edir.

* **Observer**
* **Strategy**
* **Command**
* **Template Method**
* **Iterator**
* **State**
* **Chain of Responsibility**
* **Mediator**

---

## 🎯 Vacib və Praktiki Design Pattern-lər:

### 1️⃣ Singleton Pattern (Creational)

**Problem:** Proqramda sadəcə bir obyektin olması lazımdır.

**Real Dünya:** Konfiqurasiya faylı, Database connection, Logger.

**Kod:**

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // private constructor
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 2️⃣ Factory Method Pattern (Creational)

**Problem:** Hansı obyektin yaradılacağı compile-time yox, run-time-da təyin olunmalıdır.

**Real Dünya:** Mesaj tipləri: Email, SMS, PushNotification.

**Kod:**

```java
interface Notification {
    void notifyUser();
}

class EmailNotification implements Notification {
    public void notifyUser() {
        System.out.println("Email notification sent.");
    }
}

class SMSNotification implements Notification {
    public void notifyUser() {
        System.out.println("SMS notification sent.");
    }
}

class NotificationFactory {
    public Notification createNotification(String type) {
        if (type.equals("EMAIL")) {
            return new EmailNotification();
        } else if (type.equals("SMS")) {
            return new SMSNotification();
        }
        return null;
    }
}

// İstifadə
NotificationFactory factory = new NotificationFactory();
Notification notification = factory.createNotification("EMAIL");
notification.notifyUser();
```

### 3️⃣ Builder Pattern (Creational)

**Problem:** Complex obyektlərin yaradılması prosesini sadələşdirmək.

**Real Dünya:** BurgerBuilder, ComputerBuilder

**Kod:**

```java
class Computer {
    private String CPU;
    private String RAM;

    public static class Builder {
        private String CPU;
        private String RAM;

        public Builder setCPU(String CPU) {
            this.CPU = CPU;
            return this;
        }

        public Builder setRAM(String RAM) {
            this.RAM = RAM;
            return this;
        }

        public Computer build() {
            Computer computer = new Computer();
            computer.CPU = this.CPU;
            computer.RAM = this.RAM;
            return computer;
        }
    }
}

// İstifadə
Computer computer = new Computer.Builder()
                        .setCPU("Intel i7")
                        .setRAM("16GB")
                        .build();
```

### 4️⃣ Observer Pattern (Behavioral)

**Problem:** Bir obyekt dəyişəndə digər obyektlər avtomatik xəbərdar olmalıdır.

**Real Dünya:** YouTube Subscribe, Event listener-lər.

**Kod:**

```java
interface Observer {
    void update(String message);
}

class Subscriber implements Observer {
    private String name;

    public Subscriber(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " got message: " + message);
    }
}

class Channel {
    private List<Observer> subscribers = new ArrayList<>();

    public void subscribe(Observer o) {
        subscribers.add(o);
    }

    public void notifySubscribers(String message) {
        for (Observer o : subscribers) {
            o.update(message);
        }
    }
}

// İstifadə
Channel channel = new Channel();
Subscriber a = new Subscriber("Elvin");
Subscriber b = new Subscriber("Rashid");

channel.subscribe(a);
channel.subscribe(b);

channel.notifySubscribers("New Video Uploaded!");
```

### 5️⃣ Strategy Pattern (Behavioral)

**Problem:** Eyni işi fərqli alqoritmlərlə icra etmək.

**Real Dünya:** Payment Gateway-lər, sort strategiyaları.

**Kod:**

```java
interface PaymentStrategy {
    void pay(int amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " with Credit Card.");
    }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " with PayPal.");
    }
}

class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}

// İstifadə
ShoppingCart cart = new ShoppingCart();
cart.setPaymentStrategy(new CreditCardPayment());
cart.checkout(250);
cart.setPaymentStrategy(new PayPalPayment());
cart.checkout(100);
```

### 6️⃣ Proxy Pattern (Structural)

**Problem:** Bir obyektə nəzarət etmək və ya ona girişə əlavə əməliyyatlar əlavə etmək lazımdırsa.

**Real Dünya:** VPN proxy, təhlükəsizlik yoxlaması, caching sistemi.

**Teoriya:**
Proxy, real obyektə əvəzedici kimi çıxış edir. İstifadəçi Proxy-dən istifadə edir, o isə əsl obyektə qərara əsasən müraciət edir və ya etməz.

**Kod:**

```java
interface Service {
    void request();
}

class RealService implements Service {
    public void request() {
        System.out.println("Real Service is called.");
    }
}

class ProxyService implements Service {
    private RealService realService;

    public void request() {
        if (realService == null) {
            realService = new RealService();
        }
        System.out.println("Proxy: yoxlama keçirildi.");
        realService.request();
    }
}

// İstifadə
Service service = new ProxyService();
service.request();
```

**İzah:**

* İstifadəçi birbaşa `RealService`-ə müraciət etmir.
* Əvvəlcə `ProxyService`-ə müraciət edir.
* `ProxyService` lazım olsa `RealService`-i yaradır və ya yoxlama keçirir.
* Bu, təhlükəsizlik, caching, lazy-loading üçün əla texnikadır.

---

## ✅ Nəticə:

Bu design pattern-lər istənilən Java proyektində real istifadə edilir. Daha dəqiq arxitektura, test asanlığı, dayanıqlı və genişlənən sistem təşkil edir.

Daha çox pattern istəyirsənsə, davamını yazaram. Sən dəyərli developer kimi bu pattern-lərin məntiqini anlayıb istənilən proyektə tətbiq etməlisən!
