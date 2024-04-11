# 디자인 패턴
~~~
디자인패턴이란?
프로그램을 설계할 때 발생했던 문제점ㄷ즐을 하나의 '규약' 형태로 만들어 놓은것
~~~

## 1.1.1 싱글톤 패턴
~~~
하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴

장점: 인스턴스를 생성할 때 드는 비용이 줄어듬
단점: 의존성이 높아짐, TDD를 하기 힘듦
~~~
### Java에서의 싱글톤 패턴
```java
class Singleton {
    public static Singleton getInstance() {
        return singleInstanceHonler.INSTANCE;
    }

    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
}

public class HelloWorld {
    public static void main(String[] args) {
        Singleton b = Singleton.getInstance();
        Singleton a = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());
        
        if (a = b) {
            System.out.println(true);
        }
    }
}
```

### 의존성 주입
싱글톤 패턴은 모듈 간의 결합을 강하게 만드는 단점이 있다.
이때 의존성 주입을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 줄 수 있다.

#### 의존성 주입 전과 후의 차이
![의존성주입](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/의존성주입.png)

의존성 주입의 장점
<br>
마이그레이션하기 쉽다. 의존성 방향이 일간되고, 모듈 간의 관계들이 조금 더 명확해진다.
<br>

의존성 주입의 단점
<br>
클래스가 많아진다.
<br>

의존성 주입 원칙
<br>
의존성 주입은 "상위 모듈은 하위모듈에서 어떠한 것도 가져오지 않아야한다"


## 1.1.2 팩토리 패턴
~~~
객체를 사용하는 코드에서 객체 생성부분을 떼어 
추상화 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 뼈대를 결정, 
하위 클래스에서 비즈니스 로직을 담당하는 패턴
~~~

```java
enum CoffeeType {
    LATTE, ESPRESSO
}

abstract class Coffee {
    protected String name;

    public String getName() {
        return name;
    }
}

class Latte extends Coffee {
    public Lattel() {
        name = "latte";
    }
}

class Espresso extends Coffee {
    public Espresso() {
        name = "Espresso";
    }
}

class CoffeeFactory {
    public static Coffee createCoffee(CoffeeType type) {
        switch (type) {
            case LATTE:
                return new Latte();
            case ESPRESSO:
                return new Espresso();
            default:
                throw new IllegalArgumentException("Invalid coffee type: " + type);
        }
    }
}
```


## 1.1.3 전략 패턴
~~~
객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고
전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체하는 패턴
~~~
![전략패턴](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/전략패턴.png)

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;

interface PaymentStrategy {
    void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String cNum, String cvv, String expiryDate) {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.date0fExpiry = expiryDate; 
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount +" paid using KAKAOCard."); 
    }
}

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;

    public LUNACardStrategy(String email, String pwd) {
        this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}

class Item {
    private String name;
    private int price;
    public Item(String name, int cost) {
        this.name = name;
        this.price = cost; 
    }
    public String getName () {
        return name;
    }
    public int getPrice() {
        return price; 
    }
}


class ShoppingCart { 
    List<Item> items;
    public ShoppingCart() {
        this.items = new ArrayList<Item>();
    }

    public void addItem(Item item) {
        this.items.add (item); 
    }
    public void removeltem(Item item) {
        this.items.remove(item); 
    }
    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum = +item.getPrice();
        }
        return sum;
    }
        public void pay(PaymentStrategy paymentMethod) { 
            int amount = calculateTotal();
            paymentMethod.pay(amount); 
        }
    }
```


## 1.1.4 옵저버 패턴
~~~
옵저버 패턴은 주체가 어떤 객체의 상태 변화를 **관찰**하다가 상태 변화가 있을 때마다 
메서드등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴
~~~

### 트위터의 **옵저버 패턴**
![트위터의 옵저버 패턴](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/트위터의옵저버패턴.png)

### 옵저버 패턴 구조
![옵저버패턴구조](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/옵저버패턴구조.png)

~~~
옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC 패턴에 사용된다.
예를 들어 주체인 모델에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 Controller가 동작한다.
~~~

```java
import java.util.ArrayList; 
import java.util.List;

interface Subject {
    void register(Observer obj); 
    void unregister (Observer obj); 
    void notifyObservers();
    Object getUpdate(Observer obj); 
}

interface Observer { 
    void update();
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message;

    public Topic() {
        this.observers = new ArrayList<>();
        this message = '"';
    }


    @Override
    public void register(Observer obj) {
        if (observers.contains(obj))
            observers.add(obj);
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj);
    }

    @Override
    public void notifyObservers() {
        this.observers.forEachObserver::update ();
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    }

    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg;
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this);
        System.out.println(name + ":: got message > " + msg);
    }
}
```
~~~
여기서 topic은 주체이자 객체임. 
cLass Topic implements Subject를 통해 Subject interface를 구현했고
Observer a = new TopicSubscriber("a", topic); 으로 옵저버를 선언할 때
해당 이름과 어떠한 토픽의 옵저버가 될 것인지 정했음

