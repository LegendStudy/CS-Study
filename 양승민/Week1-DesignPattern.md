# ⚜️ 디자인 패턴 ⚜️

---

## 📚 싱글톤 패턴


### ☝ 싱글톤 패턴이란?

> **하나의 클래스**에 오직 **하나의 인스턴스**만 가지는 패턴이다. 하나의 클래스를 기반으로 여러 개의 개별적인 인스턴스를 만들 수 있지만, 그렇게 하지 않고 하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는 데 쓰이며, 보통 DB 연결 모듈에 많이 쓰인다.

![싱글톤 패턴](참고자료/싱글톤_패턴/Untitled.png)

- 장점 → 인스턴스 생성 비용 감소
- 단점 → 의존성이 높아짐

---


### ☝ 자바스크립트의 싱글톤 패턴



자바스크립트에서는 리터럴 `{}` 또는 `new Object`로 객체를 생성하게 되면 다른 어떤 객체와도 다르기 때문에 이 자체만으로 싱글톤 패턴을 구현할 수 있다.

```jsx
const obj = { 
	a: 27
}
const obj2 = { 
	a: 27
}
console.log(obj === obj2)
// false
```

위 코드에서 `obj`와 `obj2`는 다른 인스턴스이다. 이 또한 `new Object` 클래스에서 나온 단 하나의 인스턴스이기 때문에 싱글톤 패턴이라 볼 수 있지만, 실제 싱글톤 패턴은 다음과 같이 구성된다.

```jsx
class Singleton {
	constructor () {
		if (!Singleton.instance) {
			Singleton. instance = this 
		}
		return Singleton. instance 
	}
	get Instance () { 
		return this
	} 
}
const a = new Singleton() 
const b = new Singleton()
console.log(a === b) // true
```

위 코드는 `Singleton.instance`라는 하나의 인스턴스를 가지는 `Singleton` 클래스를 구현한 모습이다.

---

### ☝ DB 연결 모듈


```jsx
const URL = 'mongodb://localhost: 27017/kundolapp'
const createConnection = url => ({"url" : url})
class DB { 
	constructor(url) {
		if (!DB.instance) {
			DB.instance = createConnection(url)
		}
		return DB.instance }
		connect(){
			return this.instance
		}
}
const a = new DB(URL)
const b = new DB(URL) 
console.log(a === b) // true
```

위처럼 `DB.instance`라는 하나의 인스턴스를 기반으로 a, b를 생성하여 DB 연결에 관한 인스턴스 생성 비용을 아낄 수 있다.

---


### ☝ 자바에서의 싱글톤 패턴



자바에서는 다음과 같이 중첩 클래스를 이용해서 만드는 방법이 가장 대중적이다.

```java
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld {
    public static void main(String[] args) {
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());
        if (a == b) {
            System.out.println(true);
        }
    }
}
/*
705927765 
705927765
true
*/
```

---


### ☝ mongoose의 싱글톤 패턴



실제로 `Node.js`에서 `MongoDB`를 연결할 때 쓰는 `mongoose` 모듈에서 볼 수 있다.

![MongoDB 로고](참고자료/싱글톤_패턴/Untitled%201.png)

`mongoose`의 DB를 연결할 때 쓰는 `connect()`라는 함수는 싱글톤 인스턴스를 반환합니다. 다음은 `connect()` 함수를 구현할 때 쓰인 실제 코드이다.

```jsx
Mongoose.prototype.connect = function(uri, options, callback) { 
	const _mongoose = this instanceof Mongoose ? this : mongoose;
	const conn = _mongoose.connection;
	return _mongoose._promiseOrCallback(callback, cb => { 
		conn. openUri (uri, options, err => {
			if (err != null) {
				return cb(err); 
			}
			return cb(null, _mongoose); 
		});
	});
};
```

---

### ☝ MySQL의 싱글톤 패턴



`Node.js`에서 `MySQL` DB를 연결할 때도 싱글톤 패턴이 쓰인다.

![MySQL 로고](참고자료/싱글톤_패턴/Untitled%202.png)

```jsx
const mysql = require('mysql');
const pool = mysql.createPool({
	connectionLimit: 10,
	host: 'example.org',
	user: 'root',
	password: 'secret',
	database: 'lukeDB'
});
pool.connect();

// 모듈 A
pool.query(query, function (error, results, fields) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
});

// 모듈 B
pool.query(query, function (error, results, fields) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
});
```

위 코드처럼 메인 모듈에서 DB 연결에 관한 인스턴스를 정의하고 다른 모듈인 A 또는 B에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식으로 쓰인다.

---


### ☝ 싱글톤 패턴의 단점



싱글톤 패턴은 `TDD`(Test Driven Development)를 할 때 걸림돌이 된다.

`TDD`를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.

하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 **‘독립적인’** 인스턴스를 만들기가 힘들다.

---


### ☝ DI (Dependency Injection)



싱글톤 패턴은 사용하기가 쉽고 굉장히 실용적이지만 모듈 간의 **결합을 강하게** 만든다는 단점이 있다.

이때 의존성 주입(DI, Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다.


#### 🗒️ **참고**


- 의존성이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미한다.


![의존성 주입](참고자료/싱글톤_패턴/Untitled%203.png)

위 그림처럼 메인 모듈이 **직접** 다른 하위 모듈에 대한 의존성을 주는게 아닌, 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 **간접적**으로 의존성을 주입하는 방식이다.

이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어진다.
(디커플링 된다)

- 장점

  → 모듈들을 쉽게 교체할 수 있는구조가 되어 테스팅하기 쉽고 마이그레이션하기 수월하다.
  → 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며, 모듈 간의 관계들이 조금 더 명확해진다.

- 단점

  → 모듈이 분리되어 클래스 수가 늘어나서 복잡성이 증가될 수 있고, 약간의 런타임 패널티가 생기기도 한다.

- **의존성 주입 원칙**

  → 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.

  → 두 모듈 전부 추상화에 의존해야 한다.

  → 추상화 시에 세부사항에 의존하지 말아햐 한다.

---

---


## 📚 팩토리 패턴

### ☝ **팩토리 패턴이란?**


> 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 **중요한 뼈대**를 결정하고, 하위 클래스에서 객체 생성에 관한 **구체적인 내용**을 결정하는 패턴이다.

> 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며, 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖는다.
> 또한 객체 생성 로직이 분리되어 있어 코드를 리팩토링하더라도 한 곳만 고칠 수 있게 되어 유지 보수성이 증가한다.

![팩토리 패턴 예시](참고자료/팩토리_패턴/Untitled.png)

---

### ☝ **자바스크립트의 팩토리 패턴**


자바스크립트에서 팩토리 패턴을 구현한다면 간단하게 `new0bject()`로 구현할 수 있다.

```jsx
const num = new Object(42) 
const str = new Object('abc')
num.constructor.name; // Number 
str.constructor.name; // String
```

파라미터에 따라 다른 타입의 객체를 생성하는 것을 볼 수 있다.

```jsx
class CoffeeFactory {
	static createCoffee(type) {
		const factory = factoryList[type]
		return factory.createCoffee() 
	}
}
class Latte {
	constructor() {
		this. name = "latte"
	}
}
class Espresso { 
	constructor(){
		this. name = "Espresso" 
	}
}
class LatteFactory extends CoffeeFactory{
	static createCoffee() {
		return new Latte() 
	}
}
class EspressoFactory extends CoffeeFactory{
	static createCoffee() { 
		return new Espresso()
	} 
}
const factoryList = { LatteFactory, EspressoFactory }

const main = () => {
	// 라떼 커피를 주문한다.
	const coffee = CoffeeFactory.createCoffee("LatteFactory")
	console.log(coffee.name) // latte 
}
main()
```

위 코드에서 `CoffeeFactory`라는 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스인 `LatteFactory`가 구체적인 내용을 결정하고 있다.

이는 DI라고도 볼수 있다. (CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌 LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문)

위처럼 정적 메서드로 정의하면 클래스를 기반으로 객체를 만들지 않고 호출이 가능하며, 해당 메서드에 대한 메모리 할당을 한 번만 할 수 있는 장점이 있다.

---

### ☝ **자바의 팩토리 패턴**


```java
enum CoffeeType {
    LATTE,
    ESPRESSO
}

abstract class Coffee {
    protected String name;

    public String getName() {
        return name;
    }
}

class Latte extends Coffee {
    public Latte() {
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
            case LATTE -> {
                return new Latte();
            }
            case ESPRESSO -> {
                return new Espresso();
            }
            default -> throw new IllegalArgumentException("Invalid Coffee Type: " + type);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE);
        System.out.println(coffee.getName());   // latte
    }
}
```

#### 🗒️ **Enum**


- 상수의 집합을 정의할 때 사용되는 타입이다. 자바에서는 Enum이 다른 언어보다 더 활발히 활용되며, 상수뿐만 아니라 메서드를 집어넣어 관리할 수도 있다.
- Enum을 기반으로 상수 집합을 관리한다면 코드를 리팩토링할 때 상수 집합에 대한 로직 수정 시 이 부분만 수정하면 된다는 장점이 있고, 본질적으로 스레드세이프(Thread Safe)하기 때문에 싱글톤 패턴을 만들 때 도움이 된다.

---

---

## 📚 전략 패턴

### ☝ **전략 패턴이란?**


> 정책 패턴(Policy Pattern)이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 **직접** 수정하지 않고 전략이라고 부르는 **캡슐화한 알고리즘**을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.

![전략 패턴](참고자료/전략_패턴/Untitled.png)

---

### ☝ **자바의 전략 패턴**


```java
interface PaymentStrategy{
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCardStrategy(String name, String cardNumber, String cvv, String dateOfExpiry) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.dateOfExpiry = dateOfExpiry;
    }

    @java.lang.Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard");
    }
}

class LUNACardStratgy implements PaymentStrategy {
    private String emailId;
    private String password;

    public LUNACardStratgy(String emailId, String password) {
        this.emailId = emailId;
        this.password = password;
    }

    @java.lang.Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard");
    }
}

class Item{
    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
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
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }

        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}

public class HelloWorld {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("LukeA", 100);
        Item B = new Item("LukeB", 300);

        cart.addItem(A);
        cart.addItem(B);

        // pay by LUNACard
        cart.pay(new LUNACardStratgy("yjsmk0902@gmail.com", "asdfqwer1234"));

        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Luke", "0104939304949394", "142", "04/20"));
    }
}
```

---

### ☝ **Passport의 전략 패턴**


전략 패턴을 활용한 라이브러리로는 `passport`가 있다.

![passport 홈페이지](참고자료/전략_패턴/Untitled%201.png)

> Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로, 여러 가지 **전략을 기반으로 인증할 수 있게 한다.**
> 서비스 내의 회원가입된 아이디와 비밀번호를 기반으로 인증하는 LocalStrategy 전략과 여러 다른 서비스를 기반으로 인증하는 OAuth 전략 등을 지원한다.

```jsx
var passport = require('passport'),
        LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
        function(username, password, done) {
          User.findOne({ username: username }, function(err, user) {
            if (err) {
              return done(err);
            }
            if (!user) {
              return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
              return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
          });
        }
));
```

`passport.use(new LocalStratgy( …` 처럼 `passport.use()`라는 메서드에 전략을 매개변수로 넣어서 로직을 수행하는 것을 볼 수 있다.

---

---
