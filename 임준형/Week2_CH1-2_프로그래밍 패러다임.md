# 1.2 프로그래밍 패러다임

프로그래머에게 프로그래밍의 고나점을 갖게 해주는 역할을 하는 개발 방법론

객체지향 프로그래밍: 플고르개럼들이 프로그램을 상호 작용하는 객체들의 집합으로 볼 수 있음

함수형 프로그래밍: 상태 값을 지니지 않는 함수 값들의 연속으로 생각할 수 있음

그로그래밍 패러다임은 크게 선언형, 명령형으로 나눔

선언형: `함수형` 하위 집합을 가짐

명령형: `객체지향`, `절치지향`으로 나뉨

![1.패러다임분류](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/1.패러다임분류.png)

## 1.2.1 선언형과 함수형 프로그래밍

선언형 프로그래밍은 `무엇을 풀어내는가`, `프로그램은 함수로 이루어짐`

함수형 프로그래밍은 선언형 패러다임의 일종

함수형 프로그래밍은 커링, 불변성 등 많은 특징이 있음

### 순수 함수

출력이 입력에만 의존하는 것

```javascript
const pure = (a, b) => {
    return a + b
}
```
pure 함수는 매개변수 a, b에만 영향을 받음. 만약 a,b 외의 다른 전역 변수가 출력에 영향을 주면 순수 함수가 아님


### 고차 함수

함수가 함수를 값으로 매개 변수로 받아 로직을 생성할 수 있는 것

### 일급 객체

이때 고차 함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야함

~~~ 
일급 객체 특징
- 변수나 메서드에 함수를 할당할 수 있음
- 함수 안에 함수를 매개변수로 담을 수 있음
- 함수가 함수를 반환할 수 있음
~~~

## 1.2 객체지향 프로그래밍

객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급, 객체 내부에 선언된 메서드를 활용하는 방식

설계에 많은 시간이 소요되며 처리 속도가 다른 패러다임에 비해 **상대적으로 느림**

추상화, 캡슐화, 상속성, 다형성

### 추상화

복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려 내는 것

### 캡슐화

객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것

### 상속성

상위 클래스의 특성을 하위 클래스가 이어받아서 재사용 및 추가, 확장하는 것

재사용, 계층적인 관계 생성, 유지 보수성 측면에서 중요

### 다형성

하나의 메서드나 클래스가 다양한 방법으로 동작하는 것

ex) overloading, overriding

### 오버로딩
같은 이름을 가진 메서드를 여러 개 두는 것, 

`정적 다형성`

```java
class Person {
    public void eat(String a) {
        System.out.println("I eat "
                + a);
    }

    public void eat(String a, String b) {
        System.out.println("I eat "
                + a + " and " + b);
    }
}
public class CalculateArea {
    public static void main(String[] args) {
        Person a = new Person();
        a.eat("apple");
        a.eat("tomato", "phodo");
    }
}

/* 
    I eat apple
    I eat tomato and phodo 
*/
```

### 오버라이

메서드 오버라이딩을 말함

상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것을 의미

`동적 다형성`

```java
class Animal {
    public void bark() {
        System.out.println("mumu! mumu!");
    }
}

class Dog extends Animal {
    @Override
    public void bark() {
        System.out.println("wal!!! wal!!!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.bark();
    }
}
/*
    wal!!! wal!!! 
*/
```

### 설계 원칙

SOLID 원칙
~~~
S: 단일 책임 원칙
O: 개방-폐쇄 원칙
L: 리스코프 치환 원칙
I: 인터페이스 분리 원칙
D: 의존 역전 원칙
~~~

### 단일 책임 원칙 (SRP, Responsibility Principle)

모든 클래스는 각각 하나의 책임만 가져야 하는 원칙

A라는 로직이 존재시 어떠한 클래스는 A에 관한 클래스여야하고 수정할 때도 A와 관련된 수정이여야함

### 개방 폐쇄 원칙 (OCP, Open Closed Principle)

유지 보수 사항이 생길시 코드를 쉽게 확장할 수 있어야하며 수정할 때는 닫혀있어야하는 원칙

기존 코드는 잘 변경하지 않으면서 확장은 쉽게 가능하도록 설계해야함

### 리스코프 치환 원칙 (LSP, Liskov Substitution Principle)

프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미

클래스는 상속이 이루어지며 부모, 자식이라는 계층 관계가 생성됨

이때 부모 객체에 자식 객체를 넣어도 시스템이 문제 없이 돌아가게 만드는 것을 말함

### 인터페이스 분리 원칙 (ISP, Interface Segregation Principle)

구체적인 여러 개의 인터페이스를 만들어야하는 원칙

### 의존 역전 원칙 (DIP, Dependency Inversion Principle)

자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않는 원칙

타이어를 갈아끼울 수 있는 틀을 만들어 놓은 후 다양한 타이어를 교체할 수 있어야함 -> interface 사용하여 개발

상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야하는 것

## 1.2.3 절차형 프로그래밍

로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있음

코드의 가독성이 좋으며 실행 속도가 빠름

계산이 많은 작업에 쓰임. 포트란을 이용한 대기 과학 관련 연산 작업, 머신 러닝의 배치작업 등이 있음

모듈화가 어렵고 유지 보수성이 떨어짐
