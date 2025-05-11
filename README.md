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

#### 📌 Enum Singleton Nədir?

**Normalda Singleton pattern-də:**
- private constructor yazılır,
- static instance dəyişəni olur,
- və `getInstance()` metodu ilə instance qaytarılır.

**Amma bu yanaşmanın bəzi zəiflikləri var:**
- Reflection ilə private constructor-u çağırmaq
- Serialization ilə obyektin klonunu çıxarmaq

**Enum Singleton isə:**
- Reflection-a qarşı təhlükəsizdir
- Serialization-a qarşı təhlükəsizdir
- Thread-safe-dir
- Java Enum-ları JVM tərəfindən bir dəfə yüklənir və çox stabildir.

| Zəiflik                  | Normal Singleton                                             | Enum Singleton            |
| :----------------------- | :----------------------------------------------------------- | :------------------------ |
| Reflection ilə qırmaq    | Mümkün                                                       | Mümkün deyil              |
| Serialization ilə qırmaq | Mümkün                                                       | Mümkün deyil              |
| Thread-safe              | Yox (əgər `synchronized` və ya `volatile` istifadə olunmasa) | Bəli (JVM özü təmin edir) |

**Niyə?**
- Çünki Java Enum-ların instance-larını JVM özü idarə edir və bu instance-lar əvvəlcədən static olaraq yüklənir, ona görə də Reflection və Serialization orada keçmir.

**📖 Enum Singleton Kodu:**

```java
public enum DatabaseConnectionManager {
    INSTANCE;

    private String connection;

    // Constructor (yalnız bir dəfə çağırılır)
    DatabaseConnectionManager() {
        // Məsələn connection yaradırıq
        connection = "Connected to Database";
        System.out.println("Connection yaradıldı!");
    }

    public String getConnection() {
        return connection;
    }

    public void disconnect() {
        connection = "Disconnected";
        System.out.println("Connection kəsildi!");
    }
}
```

**📖 Necə İstifadə Edilir?**
```java
public class Main {
    public static void main(String[] args) {
        // İlk dəfə instance çağırılır
        DatabaseConnectionManager manager1 = DatabaseConnectionManager.INSTANCE;
        System.out.println(manager1.getConnection());

        // İkinci dəfə çağıranda yenidən yaradılmır
        DatabaseConnectionManager manager2 = DatabaseConnectionManager.INSTANCE;
        System.out.println(manager2.getConnection());

        // Eyni instance-ı disconnect edək
        manager1.disconnect();

        // Yenə manager2 ilə connection statusuna baxaq
        System.out.println(manager2.getConnection());
    }
}
```

**📌 İzah:**
- INSTANCE — bizim Singleton obyektimizdi.
- DatabaseConnectionManager() constructor yalnız bir dəfə çağırılır (proqramda ilk dəfə çağıranda).
- Hər dəfə DatabaseConnectionManager.INSTANCE çağıranda eyni instance-a referans qaytarılır.
- Nə qədər manager1, manager2 yazsan da — eyni connection-u paylaşır.

**📌 Faydası:**
- ✅ Reflection ilə qırmaq olmur
- ✅ Serialization ilə qırmaq olmur
- ✅ Thread-safe
- ✅ Sadə, qəşəng və oxunaqlı


### 📌 Problem nədir? — Thread Safety nədir?

**Java-da proqram çox vaxt çoxlu thread-lərlə işləyir. Məsələn:**

- Birdən çox istifadəçi eyni anda sistemə daxil ola bilər.
- Birdən çox prosess paralel işləyə bilər.

**Thread Safety o deməkdir ki:**
- Birdən çox thread eyni anda eyni obyektə və ya metoda müraciət edəndə proqramın vəziyyəti pozulmasın, səhv nəticələr yaranmasın.

#### 📌 Singleton-da problem harda çıxır?
**Düşün:**
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // təhlükəli nöqtə
        }
        return instance;
    }
}
```

- İndi iki `thread` eyni anda `getInstance()` çağırsa və `instance` hələ `null` olsa:
    - Hər ikisi `if (instance == null)` yoxlayır → `true`.
    - İkisi də yeni obyekt yaradır və `Singleton pattern` pozulur.


#### 📌 Thread-safe etmək üçün yollar

##### 📌 1️⃣ synchronized Metod
```java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

**Nə baş verir?**
- `synchronized` o deməkdir ki, eyni anda yalnız bir `thread` metoda girə bilər.
- Başqası girməyə çalışanda gözləyir.

**✅ Üstünlüklər:**
- Sadə və təhlükəsiz.

**❌ Çatışmazlıqlar:**
- Performans zəifliyinə səbəb ola bilər, çünki hər dəfə metoda girəndə lock alır.

##### 📌 2️⃣ Double-Checked Locking (DCL)
**Bu daha optimallaşdırılmış variantdır:**
```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**Niyə 2 dəfə `if (instance == null)` yoxlayırıq?**
- Birinci yoxlama lock almadan işlədir (performans üçün).
- İkinci yoxlama `synchronized` içindədir ki, əmin olaq bir thread yaradır.

**Niyə `volatile`?**
- `volatile` deyir ki, bu dəyişən bütün thread-lər üçün həmişə ən son dəyəri görsün.
- Java memory modelində cache məsələlərinə görə lazımdı.

**✅ Üstünlüklər:**
- Daha sürətli.
- Thread-safe.

---

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

---

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

---

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

---

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

---

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

| Creational (Yaradıcı) | Structural (Struktural) | Behavioral (Davranış)   |
| :-------------------- | :---------------------- | :---------------------- |
| Singleton             | Adapter                 | Observer                |
| Factory Method        | Decorator               | Strategy                |
| Abstract Factory      | Proxy                   | Command                 |
| Builder               | Facade                  | State                   |
| Prototype             | Composite               | Chain of Responsibility |
|                       | Bridge                  | Template Method         |
|                       | Flyweight               | Mediator                |
|                       |                         | Iterator                |

