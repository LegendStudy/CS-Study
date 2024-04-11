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

