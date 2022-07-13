# Strategy Pattern

Created: 2022년 7월 6일 오후 3:08
Tags: CS, Design Pattern, SW Engineering

## 디자인 패턴이란?

- SW를 개발하다보니, 어떤 경우에는 어떤 방식이 적합하더라. 라는 경험이 만든 패턴
- 따라서 절대적인 우열은 없고, 맥락에 따라 적합한 패턴이 존재

## 전략 패턴이란?

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2ad59de8-c6eb-493d-82cf-e0325ece0d4c%2FUntitled.png?table=block&id=a905e42e-d825-4180-b4cc-a54e7624968b&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 맥락에 따라서 `setStrategy`로 전략을 고르면,
- 코드는 무슨 전략인지 알 필요 없이 `AlgorithmInterface()`에 따라 실행하기만 하면 되는 전략

## 예시

- 민들렛의 주니어 백엔드 개발자 백 아무개는 민들레니버스를 위협하는 용을 개발하는 막중한 임무를 맡았다. 다행히도, 모든 민들레를 불어서 없애버리는 용을 이미 시니어들이 객체지향 기법에 따라 개발을 해 놓았다.

### 수퍼클래스 `Dragon`

```java
class Dragon {
    breath() { "모든 민들레를 날려버립니다!" } // 모든 용은 브레스를 날릴 줄 안다.
    describe() // 용의 생김새를 묘사하는 추상 메서드
}
```

### 서브클래스 `RedDragon`, `IceDragon`

```tsx
class RedDragon extends Dragon {
  describe(); // 뜨거운 불을 뿜는 용입니다!
}

class IceDragon extends Dragon {
  describe(); // 모든 것을 얼려버리는 용입니다!
}
```

그런데, 민들렛 서비스가 커지게 되면서, 고인물들이 늘고, 용을 유저들이 두려워하지 않게 되었다. 그래서 용이 더 두려운 존재가 될 수 있도록 회사는 아래와 같은 요구사항을 만들었다.

> 앞으로는 용이 날아다니면서 더 많은 민들레들을 불어버리고, 대응하기 어렵도록 더 다양한 용을 만든다.

### 상속을 통한 해결

새로운 요구사항을 본 백아무개는 객체지향을 떠올리며 수퍼클래스에 `fly`를 추가하면 모든 용이 날 수 있을 거라 생각했다.

```java
class Dragon {
    breath() { "모든 민들레를 날려버립니다!" }
    describe()
    fly() { "용이 날아오릅니다!" } // 추가
}
```

훌륭한 아이디어 였다고 생각하고 개발을 마무리하고 퇴근한 뒤, 다음 날 아침 운영팀에서 전화를 받게 된다.

> 백아무개님 도마뱀용이 날아다니는데요?

`fly` 기능이 필요 없는 도마뱀 용까지 날아다니게 된 것. 상속을 사용했다고 생각했지만, 실제로는 버그를 만들어버린 것이다.

### 그러면 예외만 오버라이드 하지 뭐

이에 백아무개는 도마뱀용만 날지 못하도록 오버라이드를 해버린다.

```java
class LizardDragon extends Dragon {
		describe()
		fly() // do nothing
}
```

하지만 그 다음날 병아리용도 날고 있다는 전화를 받자, 일부 용만 날 수 있도록 좀 더 깔끔한 방법을 찾아야 겠다는 생각을 한다. 수퍼클래스 `Dragon`에서 `fly`를 정의하는 것을 포기하고, `Dragon`을 위한 인터페이스를 만든다.

### interface for Dragon

```java
interface Winged {
		// 이 용은 날개가 있는 용입니다!
		fly()
}
```

### 서브 클래스들 재정의

```java
class RedDragon extends Dragon implements Winged{
    describe()
    fly()
}

class IceDragon extends Dragon implements Winged {
    describe()
    fly()
}

class LizardDragon extends Dragon {
		describe()
}
```

처음에는 이 방식이 괜찮아 보였지만, 결국 날 수 있는 용마다 `fly` 메서드를 구현해줘야 했고, 용마다 다른 `fly` 기능을 구현할 수 없다는 점을 깨닳았다. 괴로워하는 백 아무개를 본 시니어 개발자는 `Strategy Pattern`을 참고해서 개발해보라고 조언한다.

## 전략 패턴의 원칙

### 1. 어떻게 분리해야 할까? - 캡슐화

> 달라지는 부분을 분리해라 (encapsulation, 캡슐화)

용은 `fly` 외에는 잘 작동하고 있기 때문에, 각 용마다 다른 동작이 필요한 부분은 `fly` 이다. 그렇다면 `fly`를 분리해서 동적으로 세팅하면 좋은 결과가 따를 수 있을 것 같다.

### 2. 다형성은 어떻게 확보? - `interface`

> 인터페이스를 통해 동적으로 변경이 가능하도록 설계한다.

인터페이스는 `super type`에 맞춰서 다형성을 확보한다는 의미이기도 하다.

```java
interface FlyBehavior {
		fly()
 }

class FlyWithWings implements FlyBehavior {
		fly() { "용이 날아오릅니다!" } // 나는 모습을 구현
 }

class FlyNoWay implements FlyBehavior {
		fly() { "stay.." } // 날 수 없음
 }
```

### 3. Dragon 클래스에 서브 용들의 개별 행동을 인터페이스로 위임하기

이는 서브 클래스들의 행위를 상위 클래스로 통합한다는 의미이기도 하다.

```java
public class Dragon {
    FlyBehavior flyBehavior;

    public void performFly() {
        flyBehavior.fly();
    }
}
```

### 4. 서브클래스를 정의할 때, `flyBehavior` 인스턴스 변수를 설정

```java
public class LizardDragon extends Dragon {
    public LizardDragon() {
        flyBehavior = new FlyNoWay();
    }
		public void describe() {

    }
}
```

### 5. 더 나아가 동적인 `Dragon` 설계하기

```java
public class Dragon {
    FlyBehavior flyBehavior;
    ...

    public void setFlyBehavior(FlyBehavior fb) {
        flyBehavior = fb;
    }
    ...
}
```

`Red Dragon`이 날개를 다쳐서 못 날게 되었다고 가정하고, `setter` 사용

```java
...

Dragon red = new RedDragon();

red.performFly(); // fly!

red.setFlayBehavior(new FlyNoWay());

red.perfomFly(); // stay...

...
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2ad59de8-c6eb-493d-82cf-e0325ece0d4c%2FUntitled.png?table=block&id=a905e42e-d825-4180-b4cc-a54e7624968b&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

> "A는 B이다"보다 "A에는 B가 있다"가 나을 수 있습니다. A와 B의 관계에 대해 생각해보면, `flyBehavior`가 있으며, 각각의 행위를 위임 받습니다.

두 클래스를 이런 식으로 합치는 것을 조합(composition)을 이용한다 합니다.

용 클래스에서는 행동을 상속받는 대신, 올바른 행동 객체로 조합됨으로써 행동을 부여받게 됩니다.

> 이처럼 동적으로 조합을 이용하여 시스템을 만들면 유연성을 크게 향상시킬 수 있습니다.

## 정리

- 전략 패턴이란, 객체 지향적 프로그래밍 방법 중 하나로서 기능의 차이가 있는 부분을 캡슐화하고, 다형적 메서드를 공통된 인터페이스 부모로 통합해 사용하는 방법
