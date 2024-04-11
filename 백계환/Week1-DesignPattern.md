- 라이브러리와 프레임워크의 차이
    - 개발자의 자율성(컨트롤) 정도의 차이라고 생각

## 1️⃣ 싱글톤 패턴

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴, 해당 인스턴스를 다른 모듈들이 공유
- 데이터베이스 연결 모듈에 많이 사용
- 인스턴스를 생성할 때 드는 비용이 줄어드는 장점
- 의존성이 높아진다는 단점

![싱글톤 패턴](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/201f8806-a2d0-4cb5-886f-4519d88422a0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.51.32.png)

### 자바에서의 싱글톤 패턴

- 아래 코드와 같이 중첩 클래스를 이용해서 만드는 방법이 가장 대중적

```java
class Singleton {
		private static class singleInstanceHolder {
				private static final Singleton NISTANCE = new Singleton(); 
		}
		public static Singleton getInstance() {
				return singleInstanceHolder. INSTANCE; }
		}
		
public class HelloWorld {
		public static void main(String[] args) {
				Singleton a = Singleton.getInstance(); 
				Singleton b = Singleton.getInstance();
				System.out.println(a.hashCode()); // 705927765
				System.out.println(b.hashCode()); // 705927765
				if (a == b) {
						System.out.printIn(true);  // true
				}
		}
}
```

### 싱글톤 패턴의 단점

- TDD(Test Driven Development)를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며, 테스트를 어떤 수서로든 실행할 수 있어야 한다. 하지만 싱글톤 패턴은 미리 생성된 하나의 인스ㅓㄴ스를 기반으로 구현하는 패턴이므로 각 테스트마다 ‘독립적인’ 인스턴스를 만들기가 어렵다.

### 의존성 주입

- 싱글톤 패턴은 사용하기가 쉽고 굉장히 실용적이지만 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다. 이때, 의존성 주입(DI, Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다.
- 메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식이다. 이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다. ( == 디커플링)
    
    ![의존성 주입](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/e35e058e-59bf-4a48-ba07-8341eaf081e9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.17.35.png)
    
- 장점
    - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기 수월
    - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관, 모듈간의 관계가 명확
- 단점
    - 모듈들이 더욱 분리되므로 복잡성이 증가될 수 있으며 약간의 런타임 페널티가 생기기도 한다.
- 의존성 주입 원칙
    - “상위 모듈을 하위 모듈에서 어떠한 것도 가져오지 않아야 합니다. 또한 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 합니다.”

## 2️⃣ 팩토리 패턴

- 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 된다.
- 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있어 유지보수성이 증가

### 자바에서의 팩토리 패턴

```java
enum CoffeeType {
		LATTE, ESPRESSO
}
abstract class Coffee{
		protected String name;
		public String getName()}
				return name;
		}
}
class Latte extends Coffee{
		public Latte(){
			name = "latte";
		}
}
class Espresso extends Coffee{
		public Espresso(){
				name = "Espresso"
		}
}
class CoffeeFactory{
		public static Coffee createCoffee(CoffeeType type){
				switch(type){
						case LATTE:
								return new Latte();
						case ESPRESSO:
								return new Espresso();
						default:
								throw new IllegalArgumentException("Invalid coffee type: " +type);
				}
		}
}
public class Main{
		public static void main(String[] args){
				Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE);
				System.out.print(coffee.getName()); // latte
		}
}
```

## 3️⃣ 전략패턴

- 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전략이라고 부르는 ‘캡슐화한 알고리즘’을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

### 자바에서의 전략 패턴

```java
interface PaymentStrategy{
		public void pay(int amount);
}
class KAKAOCardStrategy implements PaymentStrategy{
		private String name;
		private String cardNumber;
		private String cvv;private String dateOfExpiry;
		// 생성자 ..
		@Override
		public void pay(int amount){
				System.out.print(amount + " paid using KAKAOCard.");
		}
}
class LUNACardStrategy implements PaymentStrategy{
		private String emailId;
		private String password;
		// 생성자 ..
		@Override
		public void pay(int amount){
				System.out.print(amount + " paid using LUNACard.");
		}
}
class ShoppingCart{
		List<Item> items;
		int amount = calculateTotal();
		public void pay(PaymentStrategy paymentMethod){
				paymentMethod.pay(amount);
		}
}
public class HelloWorld{
		public static void main(String[] args){
				ShoppingCart cart = new ShoppingCart();
				// .. 중략
				cart.pay(new LUNACardStrategy ("kundol@example.com", "pukubababo"));
				cart.pay(new KAKAOCardStrategy ("Ju hongchul", "123456789", "123", "12/01"));
		}
}
```

## 4️⃣ 옵저버 패턴

- 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴
- 주로 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용됨

### 자바에서의 옵저버패턴

```java
내일 다시보기
```

## 5️⃣ 프록시 패턴과 프록시 서버

### 프록시 패턴

- 대상 객체에 접근하지 전 그 접근에 대한 흐름을 가로채 해당 접근을 필터링하거나 수정하는 등의 역할을 하는 계층이 있는 디자인 패턴
- 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용. 프록시 서버로도 활용된다.

### 프록시 서버

- 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킴.

![프록시 서버](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/e10d1438-5521-47f8-9300-320e442876a0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.20.32.png)

- DDOS 공격 방어
    - 짧은 시간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격
- CDN (Content Delivery Network)
    - 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크
- CORS (Cross-Origin Resource Sharing)
    - 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘
    - 오리진
        - 프로토콜과 호스트 이름, 포트의 조합

## 6️⃣ 이터레이터 패턴

- 이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소에 접근하는 디자인 패턴
- 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회 가능

## 7️⃣ 노출모듈 패턴

- 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴
- 즉시 실행 함수
    - 함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용

## 8️⃣ MVC 패턴

- 모델(Model), 뷰(View), 컨트롤러(Controller)
- 애플리케이션의 구성 요소를 세 가지 역할로 구분
- 재사용성과 확장성이 용이
- 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점
    
    ![MVC 패턴](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/0ec1a17c-7f16-4be9-80e8-3ca6eedf4e7b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.14.32.png)
    

### Model (모델)

- 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻함

### View (뷰)

- 사용자 인터페이스 요소. 모델을 기반으로 사용자가 볼 수 있는 화면을 뜻한다.
- 변경이 일어나면 컨트롤러에 이를 전달한다.

### Controller (컨트롤러)

- 모델과 뷰를 잇는 다리 역할을 하며 메인 로직을 담당한다.
- 모델과 뷰의 생명주기를 관리한다.
- 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려준다.

## 9️⃣ MVP 패턴

- MVC 패턴으로부터 파생됨. Controller → Presenter로 교체된 패턴
    
    ![MVP 패턴](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/55e923ff-ac9c-4394-ba00-9fd0de86c632/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.16.20.png)
    
- 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴(?)

## 🔟 MVVM 패턴

- MVC 패턴의 Controller → View Model로 바뀐 패턴
    
    ![MVVM 패턴](https://prod-files-secure.s3.us-west-2.amazonaws.com/3b9b9628-d44d-4922-bfa2-1af07c27a62c/91c12eb0-2e37-473e-bc3b-115b5084e8c2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.19.06.png)
    
- MVC 패턴과는 다르게 커맨드와 데이터 바인딩을 가지는 것이 특징
