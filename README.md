# 📐 Design Patterns

## 📖 Design Pattern Nədir?

Design pattern-lər — proqram təminatında **təkrarlanan problemlər üçün sınaqdan çıxmış, effektiv və sistemləşdirilmiş həll üsullarıdır**. Yəni, proqramçılar hansısa vəziyyət qarşısında artıq başqalarının sınayıb istifadə etdiyi həll modelindən istifadə edirlər. Bu, həm layihənin daha **modul**, **testə uyğun**, **güvənli**, həm də **asan idarə olunan** olmasını təmin edir.

Proqram təminatında pattern-lər istifadə etmək:
- Təkrarlanan problemlər üçün daha sürətli və düzgün həll tapmağa.
- Kodun daha oxunaqlı və genişlənə bilən olmasına.
- Proqram arxitekturasının daha dayanıqlı qurulmasına kömək edir.

---

## 📦 3 Əsas Kateqoriya

Design Pattern-lər funksional məqsədlərinə görə **3 əsas qrupa** bölünür:

### 1️⃣ Creational (Yaradıcı Pattern-lər)

Obyektlərin yaradılması prosesində elastiklik və rahatlıq təmin edir.

- **Singleton** — Sistemdə tək instance olmasını təmin edir.
- **Factory Method** — Hər hansı obyektin növünü run-time-da təyin edir.
- **Abstract Factory** — Bir-birilə əlaqəli obyekt ailələrini yaradır.
- **Builder** — Kompleks obyektlərin tədricən yaradılması üçün istifadə olunur.
- **Prototype** — Obyektlərin nüsxələnməsi (clone) ilə yeni obyektlər yaradır.

---

### 2️⃣ Structural (Struktur Pattern-lər)

Obyekt və sinifləri birləşdirmək və əlaqələndirmək üçün istifadə olunur.

- **Adapter** — İki fərqli interfeysi uyğunlaşdırır.
- **Decorator** — Obyektə əlavə imkanlar əlavə edir.
- **Composite** — Obyektləri ağac strukturu kimi idarə edir.
- **Proxy** — Obyektə girişi idarə etmək üçün əvəzedici yaradır.
- **Facade** — Çoxsaylı siniflər üçün sadə interfeys təmin edir.
- **Bridge** — İnterfeyslə implementasiyanı ayırır.
- **Flyweight** — Çox sayda oxşar obyektlər üçün yaddaş sərfiyyatını azaldır.

---

### 3️⃣ Behavioral (Davranış Pattern-lər)

Obyektlər arasındakı əlaqə və davranış mexanizmlərini idarə edir.

- **Observer** — Bir obyekt dəyişəndə ona bağlı olan obyektləri xəbərdar edir.
- **Strategy** — Davranış alqoritmlərini dinamik dəyişməyə imkan verir.
- **Command** — İstənilən əməliyyatı obyekt kimi paketləyir.
- **Template Method** — Əsas metod skeleton-u, dəyişən hissələri sub-siniflərdə.
- **Iterator** — Obyekt kolleksiyasını ardıcıl gəzməyə imkan verir.
- **State** — Obyektin vəziyyətinə uyğun davranışını dəyişir.
- **Chain of Responsibility** — İstək obyektlərinin zəncirvari ötürülməsi.
- **Mediator** — Obyektlər arası əlaqəni mərkəzləşdirir.

---

## 🎯 Ən Vacib və Praktiki Design Pattern-lər

### 1️⃣ Singleton Pattern (Creational)

**Problem:** Sistemdə tək instance olması lazımdır.

**İstifadə Yeri:** 
- Database connection
- Logger
- Configuration faylı

**Kod:**
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
#### 🔓 Singleton-u Necə Qırmaq Olar?

- Bunu nece qirmaq olar?
    - Reflection ilə Qırmaq
    - Serialization ilə Qırmaq
    - Cloning(Clonable) ilə Qırmaq
 
##### 1️⃣ Reflection ilə Qırmaq
Java Reflection API vasitəsilə private constructor-u accessible edib yeni instansiya yarada bilərik.

**Kod:**

```java
import java.lang.reflect.Constructor;

public class BreakSingletonWithReflection {
    public static void main(String[] args) throws Exception {
        Singleton instance1 = Singleton.getInstance();

        Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
        constructor.setAccessible(true);  // private constructor-u public kimi edir
        Singleton instance2 = constructor.newInstance();

        System.out.println(instance1.hashCode());
        System.out.println(instance2.hashCode());
    }
}
```

**Nəticə:**
- Eyni sinifdən iki fərqli obyekt yaranacaq.

##### 2️⃣ Serialization ilə Qırmaq
Əgər Singleton Serializable edilibsə, onu serialize edib deserialize etməklə yeni obyekt yaradıla bilər.

**Kod:**

```java
import java.io.*;

public class BreakSingletonWithSerialization {
    public static void main(String[] args) throws Exception {
        Singleton instance1 = Singleton.getInstance();

        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("singleton.ser"));
        out.writeObject(instance1);
        out.close();

        ObjectInputStream in = new ObjectInputStream(new FileInputStream("singleton.ser"));
        Singleton instance2 = (Singleton) in.readObject();
        in.close();

        System.out.println(instance1.hashCode());
        System.out.println(instance2.hashCode());
    }
}
```

**Həll:**
- Bunu önləmək üçün readResolve() metodu əlavə etməliyik:

```java
protected Object readResolve() {
    return instance;
}
```

##### 3️⃣ Cloning ilə Qırmaq
Əgər Singleton Cloneable implement edibsə və clone() metodu varsa, onu klonlayıb yeni obyekt çıxarmaq olar.

**Kod:**

```java
public class Singleton implements Cloneable {
    private static Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

// Test
Singleton instance1 = Singleton.getInstance();
Singleton instance2 = (Singleton) instance1.clone();

System.out.println(instance1.hashCode());
System.out.println(instance2.hashCode());
```

**Həll:**
- ``clone()`` metodunu override edib Exception atmaq:

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException();
}
```

### 2️⃣ Factory Method Pattern (Creational)

**Problem:** Hansı obyektin yaradılacağını compile-time yox, run-time-da seçmək lazımdır.

**İstifadə Yeri:**
- Notification system (Email, SMS, Push)
- Database connector-lar

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
```

### 3️⃣ Builder Pattern (Creational)

**Problem:** Kompleks obyektlərin fərqli konfigurasiya ilə rahat yaradılması.

**İstifadə Yeri:**
- BurgerBuilder
- ComputerBuilder
- HTTP Request Builder

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
```

### 4️⃣ Observer Pattern (Behavioral)

**Problem:** Bir obyekt dəyişəndə ona bağlı digərləri xəbərdar olmalıdır.

**İstifadə Yeri:**

- YouTube Subscribe sistemi
- Event Listener-lər
- Stock Market Ticker

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
        System.out.println(name + " received: " + message);
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
```

### 5️⃣ Strategy Pattern (Behavioral)

**Problem:** Davranışı run-time-da dəyişmək.

**İstifadə Yeri:**
- Ödəniş sistemləri (CreditCard, PayPal, Bitcoin)
- Sort strategiyaları
- Compression algoritmları

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
```

### 6️⃣ Proxy Pattern (Structural)

**Problem:** Bir obyektə nəzarət və ya girişə əlavə əməliyyatlar əlavə etmək.

**İstifadə Yeri:**
- Caching sistemi
- Lazy-loading
- Təhlükəsizlik yoxlaması (Access Control)

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
        System.out.println("Proxy: yoxlama edildi.");
        realService.request();
    }
}
```
