# 1.2 프로그래밍 패러다임

> 프로그래밍 패러다임은 프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론
> 
- 자바는 jdk 1.8부터 함수형 프로그래밍 패러다임을 지원하기 위해 람다식, 생성자 레퍼런스, 메서드 레퍼런스를 도입했고 선언형 프로그래밍을 위해 스트림같은 표준 API등도 추가했다.
- 프로그래밍 패러다임은 크게 선언형, 명령형으로 나누며, 선언형은 함수형이라는 하위 집합을 갖는다.
명령형은 다시 객체지향, 절차지향으로 나눈다.
    
    

## 1.2.1 선언형과 함수형 프로그래밍

> “프로그램은 함수로 이루어진 것이다.”
> 
- 순수 함수: 출력이 입력에만 의존하는 것, 전역변수가 영향을 주면 안됨
- 고차 함수: 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것
    - 고차 함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야한다.

## 1.2.2 객체지향 프로그래밍

> 객체들의 집합으로 프로그램의 상호 작용을 표현한다.
> 
- 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용
- 설계에 많은 시간이 소요, 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느림

### 객체 지향 프로그래밍의 특징

- **추상화**
    - 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것
- **캡슐화**
    - 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것
- **상속성**
    - 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
    - 코드의 재사용 측면, 계층적인 관계 생성, 유지 보수성 측면에서 중요
- **다형성**
    - 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것. ex) 오버로딩, 오버라이딩
    - 오버로딩: 같은 이름을 가진 메서드 여러 개, 타입/유형/개수로 구별함. 
                   컴파일 중에 발생하는 ‘정적’ 다형성
    - 오버라이딩: 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것
                       런타임 중에 발생하는 ‘동적’ 다형성

### 설계 원칙 (SOLID)

### **S - 단일 책임 원칙 (Single Responsibility Principle)**

한 클래스는 **하나의 책임**만 가져야 한다. 즉, 클래스를 변경하는 이유는 하나뿐이어야 한다.

### **O - 개방 폐쇄 원칙 (Open/Closed Principle)**

유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야하는 원칙. 즉, 기존의 코드는 잘 변경하지 않으면서도 확장은 쉽게 할 수 있어야 한다.

- 예시
    
    > 예제 시나리오
    > 
    
    간단한 결제 시스템을 구현하되, 여러 결제 방식을 지원할 수 있도록 설계합니다. 기존 시스템을 수정하지 않고도 새로운 결제 방식을 쉽게 추가할 수 있어야 합니다.
    
    > 코드
    > 
    
    ```java
    // 결제 방식에 대한 인터페이스
    interface PaymentMethod {
        void pay(int amount);
    }
    
    // 신용카드 결제 구현
    class CreditCardPayment implements PaymentMethod {
        @Override
        public void pay(int amount) {
            System.out.println("Paid " + amount + " using Credit Card");
        }
    }
    
    // PayPal 결제 구현
    class PayPalPayment implements PaymentMethod {
        @Override
        public void pay(int amount) {
            System.out.println("Paid " + amount + " using PayPal");
        }
    }
    
    // 결제 처리기
    class PaymentProcessor {
        private PaymentMethod paymentMethod;
    
        public PaymentProcessor(PaymentMethod paymentMethod) {
            this.paymentMethod = paymentMethod;
        }
    
        public void processPayment(int amount) {
            paymentMethod.pay(amount);
        }
    }
    
    public class OCPExample {
        public static void main(String[] args) {
            PaymentProcessor creditCardProcessor = new PaymentProcessor(new CreditCardPayment());
            creditCardProcessor.processPayment(1000);
    
            PaymentProcessor payPalProcessor = new PaymentProcessor(new PayPalPayment());
            payPalProcessor.processPayment(2000);
        }
    }
    ```
    
    위의 예제에서 `PaymentMethod` 인터페이스는 다양한 결제 방식의 추상화를 제공합니다. `CreditCardPayment`와 `PayPalPayment` 클래스는 이 인터페이스를 구현하여 구체적인 결제 방식을 정의합니다. `PaymentProcessor` 클래스는 `PaymentMethod` 인터페이스에 의존하므로, 새로운 결제 방식을 추가하고 싶을 때 기존 코드를 변경할 필요 없이 새로운 결제 방식 클래스만 추가하면 됩니다. 이는 개방-폐쇄 원칙을 따르는 아주 좋은 예시입니다.
    

### **L - 리스코프 치환 원칙 (Liskov Substitution Principle)**

프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미한다. 부모 객체에 자식 객체를 넣어도 시스템이 문제없이 돌아가게 만드는 것

- 예시
    
    > **리스코프 치환 원칙을 위반한 예시 코드**
    > 
    
    ```java
    class Rectangle {
        protected int width;
        protected int height;
    
        public void setWidth(int width) {
            this.width = width;
        }
    
        public void setHeight(int height) {
            this.height = height;
        }
    
        public int getArea() {
            return width * height;
        }
    }
    
    class Square extends Rectangle {
        @Override
        public void setWidth(int width) {
            super.setWidth(width);
            super.setHeight(width);
        }
    
        @Override
        public void setHeight(int height) {
            super.setWidth(height);
            super.setHeight(height);
        }
    }
    ```
    
    ```java
    public class LSPExample {
        private static void printArea(Rectangle r) {
            r.setWidth(5);
            r.setHeight(4);
            System.out.println("Expected area of 5 * 4 = " + r.getArea());
        }
    
        public static void main(String[] args) {
            Rectangle rectangle = new Rectangle();
            printArea(rectangle);
    
            Rectangle square = new Square();
            printArea(square);
        }
    }
    ```
    
    이 예제에서 `Square` 클래스는 `Rectangle` 클래스를 상속받지만, 리스코프 치환 원칙을 위반합니다. 정사각형은 모든 면이 같으므로 `setWidth` 또는 `setHeight` 중 하나만 호출되어도 두 매개변수 모두 같은 값으로 설정됩니다. 이로 인해, `Square` 인스턴스를 사용할 때 `Rectangle`로 예상한 동작과 다르게 동작하므로 `Square`는 `Rectangle`의 올바른 하위형(subtype)이 아닙니다.
    
    > **리스코프 치환 원칙을 준수한 예시 코드**
    > 
    
    ```java
    interface Shape {
        void setWidth(int width);
        void setHeight(int height);
        int getArea();
    }
    class Rectangle implements Shape {
        protected int width;
        protected int height;
    
        @Override
        public void setWidth(int width) {
            this.width = width;
        }
        @Override
        public void setHeight(int height) {
            this.height = height;
        }
        @Override
        public int getArea() {
            return width * height;
        }
    }
    
    class Square implements Shape {
        private int size;
    
        @Override
        public void setWidth(int width) {
            this.size = width;
        }
        @Override
        public void setHeight(int height) {
            this.size = height;
        }
        @Override
        public int getArea() {
            return size * size;
        }
    }
    ```
    
    ```java
    public class LSPExample {
        private static void printArea(Shape shape) {
            shape.setWidth(5);
            shape.setHeight(4);
            System.out.println("Area: " + shape.getArea());
        }
    
        public static void main(String[] args) {
            Shape rectangle = new Rectangle();
            printArea(rectangle);
    
            Shape square = new Square();
            printArea(square); // 이제 각 Shape 구현체는 자신의 로직에 따라 올바르게 작동합니다.
        }
    }
    ```
    
    이제 `Rectangle`과 `Square`는 `Shape` ****인터페이스를 구현합니다. 각 클래스는 자신만의 구현을 가지며, 서로 다른 동작을 수행할 수 있습니다. `Square`는 입력된 값으로 모든 면의 크기를 같게 설정하고, `Rectangle`은 너비와 높이를 독립적으로 설정할 수 있습니다. 이 구조는 리스코프 치환 원칙을 준수하며, 서로 다른 `Shape` 구현체들이 예상대로 동작하도록 보장합니다.
    

### **I - 인터페이스 분리 원칙 (Interface Segregation Principle)**

하나의 일반적인 인터페이스보다 더 작고 구체적인 여러 개 의 인터페이스를 만들어야 하는 원칙

- 예시
    
    큰 인터페이스를 더 작고 구체적인 인터페이스로 분리하는 방법
    
    > 기존의 큰 인터페이스
    > 
    
    ```java
    interface Device {
        void print();
        void scan();
        void fax();
    }
    ```
    
    > 인터페이스 분리
    > 
    
    ```java
    interface Printer {
        void print();
    }
    interface Scanner {
        void scan();
    }
    interface Fax {
        void fax();
    }
    ```
    
    ```java
    class SimplePrinter implements Printer {
        @Override
        public void print() {
            System.out.println("Printing document");
        }
    }
    
    class MultiFunctionPrinter implements Printer, Scanner, Fax {
        @Override
        public void print() {
            System.out.println("Printing document");
        }
    
        @Override
        public void scan() {
            System.out.println("Scanning document");
        }
    
        @Override
        public void fax() {
            System.out.println("Sending fax");
        }
    }
    ```
    
    이 예제에서 각 클래스는 필요한 인터페이스만 구현하므로, 클라이언트는 자신이 사용하지 않는 기능에 의존하지 않게 됩니다. 이것이 인터페이스 분리 원칙의 핵심이며, 클라이언트에 불필요한 의존성을 강요하지 않는 유연하고 확장 가능한 설계를 가능하게 합니다.
    

### **D - 의존 역전 원칙 (Dependency Inversion Principle)**

상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야 한다.

이 원칙은 소프트웨어의 재사용성을 높이고, 모듈 간의 결합도를 낮추어 유지보수성을 향상시킵니다.

- 예시
    
    다음 예제는 데이터 저장 메커니즘의 추상화를 통해 이 원칙을 보여줍니다. 데이터 저장 기능(고수준 모듈)이 특정 데이터베이스 구현(저수준 모듈)에 직접 의존하지 않도록 설계되었습니다.
    
    ```java
    interface DataRepository {
        void save(Object data);
    }
    class SqlDatabase implements DataRepository {
        @Override
        public void save(Object data) {
            System.out.println("Saving data to SQL Database: " + data);
        }
    }
    
    class NoSqlDatabase implements DataRepository {
        @Override
        public void save(Object data) {
            System.out.println("Saving data to NoSQL Database: " + data);
        }
    }
    ```
    
    ```java
    class DataManager {
        private DataRepository dataRepository;
    
        public DataManager(DataRepository dataRepository) {
            this.dataRepository = dataRepository;
        }
    
        public void saveData(Object data) {
            dataRepository.save(data);
        }
    }
    ```
    
    ```java
    public class DIPExample {
        public static void main(String[] args) {
            DataRepository sqlDb = new SqlDatabase();
            DataManager dataManager = new DataManager(sqlDb);
            dataManager.saveData("Some data");
    
            DataRepository noSqlDb = new NoSqlDatabase();
            DataManager dataManager2 = new DataManager(noSqlDb);
            dataManager2.saveData("Some other data");
        }
    }
    ```
    
    이 코드에서 `DataManager`는 `DataRepository` 인터페이스에 의존하며, 이는 구체적인 데이터베이스 구현(SQL 또는 NoSQL)의 세부 사항을 모릅니다. 이로 인해 `DataManager`는 구체적인 데이터베이스 구현이 변경되어도 영향을 받지 않습니다. 데이터베이스가 SQL에서 NoSQL로 변경되거나 그 반대로 변경되더라도, `DataManager` 클래스는 수정할 필요가 없습니다. 이 예제는 의존성 역전 원칙을 적용하여 소프트웨어의 유연성과 확장성을 향상시키는 방법을 보여줍니다.
    

## 1.2.3 절차형 프로그래밍

> 로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있다.
> 
- 코드의 가독성이 좋으며 실행 속도가 빠르다.
- 모듈화하기가 어렵고 유지 보수성이 떨어진다.

## 1.2.4 패러다임의 혼합

- 비즈니스 로직이나 서비스의 특징을 고려해서 패러다임을 정하는 것이 좋다.
- 여러 패러다임을 조합하여 상황과 맥락에 따라 패러다임 간의 장점만 취해 개발하는 것이 좋다.
