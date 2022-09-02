### Contents

- [SRP, 단일 책임 원칙](#srp-단일-책임-원칙)
- [OCP, 개방 폐쇄 원칙](#ocp-개방-폐쇄-원칙)
- [LSP, 리스코프 치환 원칙](#lsp-리스코프-치환-원칙)
- [ISP, 인터페이스 분리 원칙](#isp-인터페이스-분리-원칙)
- [DIP, 의존 역전 원칙](#dip-의존-역전-원칙)

# SOLID 원칙

SOLID 원칙은 객체지향 설계 원칙으로 다섯 가지의 원칙을 가집니다. 아래의 다섯 가지 원칙을 준수함으로써 소프트웨어를 객체지향적으로 설계하고 구현할 수 있습니다.

- 단일 책임 원칙 (Single Responsibility Principle)
- 개방 폐쇄 원칙 (Open Closed Principle)
- 리스코프 치환 원칙 (Liskov Substitution Principle)
- 인터페이스 분리 원칙 (Interface Segregation Principle)
- 의존 역전 원칙 (Dependency Inversion Principle)

## SRP, 단일 책임 원칙

SRP 원칙은 `클래스는 단 하나의 책임을 가져야 한다`는 규칙을 제시합니다.

여기서 책임이란 클래스가 변경되는 이유를 말하며, 클래스가 변경되는 이유가 단 하나여야 한다는 의미입니다.

### SRP 원칙 위반 예시

```java
public class Viewer {

    // ...

    private FileDocument fileDocument;

    public void view() {
        String data = fileDocument.getContent();
        System.out.println(data);
    }
}
```

위 코드에서 Viewer 클래스는 데이터를 화면에 출력하는 책임을 가집니다. view() 메서드는 fileDocument 객체로부터 content를 가져와 화면에 출력합니다. 만약 fileDocument 객체가 반환하는 content의 타입이 String에서 List<String>으로 변경된다면 view() 메서드는 아래와 같이 변경될 것입니다.

```java
public void view() {
    // String -> List<String>
    List<String> data = fileDocument.getContent();
    for (String d : data) {
        System.out.println(d);
    }
}
```

Viewer 클래스는 데이터를 화면에 출력하는 책임만 가져야 하므로 Viewer 클래스가 변경될 수 있는 때는 화면에 출력하는 방식을 바꿀 때가 될 것입니다. 그런데, 앞선 이유가 아닌 fileDocument 객체로부터 데이터를 가져오는 부분 때문에 Viewer 클래스에 변경이 발생했습니다. 이는 SRP 원칙을 위반하는 것입니다.

SRP 원칙을 지키기 위해 fileDocument 객체가 반환하는 데이터를 추상화할 수 있습니다. String이나 List\<String\>와 같은 저수준의 타입이 아닌 별도의 추상화된 타입을 만들고 해당 타입을 사용하는 것입니다.

### SRP 원칙 반영

```java
public class Data {

    // 데이터를 담을 타입 정의

    public void show() {
        // 타입에 따른 데이터 출력
    }
}

public class ListData extends Data {

    @Override
    public void show() {
        // 메서드 오버라이딩
    }
}

public class Viewer {

    // ...

    private FileDocument fileDocument;

    public void view() {
        Data data = fileDocument.getContent();
        data.show();
    }
}
```

위의 코드에서 Data 클래스를 만들고 내부에 데이터를 출력하는 show() 메서드를 작성했습니다. Viewer 클래스의 view() 메서드는 fileDocument 객체가 반환하는 data 객체의 show() 메서드를 호출하도록 했습니다.

이때 fileDocument 객체가 getContent() 메서드의 반환 타입을 Data 타입이 아닌 ListData 타입으로 변경하더라도 Viewer 클래스는 변경되지 않습니다. 왜냐하면 ListData 타입은 Data 타입을 상속하고 있고 Viewer 클래스의 view() 메서드는 추상화된 타입인 Data 타입으로 하위 타입인 ListData 타입을 전달받아 사용하기 때문입니다.

따라서 fileDocument 객체가 반환하는 타입이 Data 타입이거나 Data 타입을 상속한 하위 타입(MapData, SetData 등)이라면 Viewer 클래스는 변경할 필요가 없게 됩니다.

이와 같이 SRP 원칙을 지킴으로써 fileDocument 객체의 변화가 Viewer 클래스에 끼치는 영향을 낮출 수 있습니다.

## OCP, 개방 폐쇄 원칙

OCP 원칙은 `확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다`는 규칙을 제시합니다.

기능은 추가될 수 있어야 하고, 해당 기능를 사용하는 코드는 수정하지 않아야 한다는 의미입니다.

### OCP 원칙 위반 예시

```java
public class Viewer {

    // ...

    private FileDocument fileDocument;

    private ElectronicDocument electronicDocument;

    public void view(boolean isFile) {
        Data data = null;
        if (isFile) {
            data = fileDocument.getContent();
        } else {
            data = electronicDocument.getContent();
        }
        data.show();
    }
}
```

fileDocument 객체가 파일 문서 데이터를 제공하던 상황에서 전자 문서의 데이터를 제공하는 기능을 추가한다고 가정해봅시다. 그럼 위와 같이 Viewer 클래스를 변경할 수 있을 것입니다. electronicDocument 기능이 추가됨에 따라 해당 기능을 사용하는 view() 메서드의 코드가 변경된 것을 확인할 수 있습니다. 이는 OCP 원칙을 위반하는 것입니다.

OCP 원칙을 지키기 위해 fileDocument 객체와 electronicDocument 객체를 추상화하여 상위 객체를 만들고 Viewer 클래스의 view() 메서드가 상위 객체를 사용하도록 할 수 있습니다.

### OCP 원칙 반영

```java
public class Document {

    private Data data;

    public getContent() {

    }
}

public class FileDocument extends Document {

    // ...
}

public class ElectronicDocument extends Document {

    // ...
}

public class Viewer {

    // ...

    private Document document;

    public void view() {
        Data data = document.getContent();
        data.show();
    }
}
```

위와 같이 fileDocument 객체와 electronicDocument 객체를 추상화한 Document 타입을 클래스를 정의하고 Viewer 클래스가 Document 클래스를 사용하도록 합니다. Document 클래스가 FileDocument 클래스와 ElectronicDocument 클래스의 상위 클래스이므로 Document 클래스를 통해 FileDocument, ElectronicDocument 클래스의 기능을 모두 사용할 수 있게 됩니다.

이후에 새로운 형태의 문서 데이터를 지원하는 기능이 추가되더라도 해당 기능의 클래스를 Document 클래스를 상속받아 구현한다면 Viewer 클래스는 변경할 필요가 없게 됩니다.

즉, 새로운 기능이 추가되더라도 해당 기능을 사용하는 View 클래스는 변경하지 않아도 됩니다.

이와 같이 OCP 원칙을 지킴으로써 기능 확장은 자유롭게 하고 확장에 따른 영향은 낮출 수 있습니다.

## LSP, 리스코프 치환 원칙

LSP 원칙은 `상위 타입의 객체를 하위 타입의 객체로 바꿔도 상위 타입을 사용하는 객체는 정상적으로 동작해야 한다`는 규칙을 제시합니다.

상위 객체의 메서드를 하위 객체에서 오버라이딩할 때 적절하게 구현해야 한다는 의미입니다.

### LSP 원칙 위반 예시

```java
public class Food {

    public void bad() {
        System.out.println("음식이 상했습니다.");
    }
}

public class IceCream extends Food {

    public void melt() {
        System.out.println("아이스크림이 녹았습니다.");
    }
}

public class FoodUtil {

    public void badALl(List<Food> foods) {
        for (Food food : foods) {
            if (food instanceof IceCream) {
                ((IceCream) food).melt();
            } else {
                food.bad();
            }
        }
    }
}
```

위 코드를 보면 Food 클래스에 `음식이 상하다`의 기능을 가지는 bad() 메서드가 있습니다. IceCream 클래스는 Food 클래스를 상속하는데, `음식이 상하다`의 의미보다 더 직관적인 `아이스크림이 녹다`라는 의미를 가지는 melt() 메서드를 추가했습니다.

FoodUtil 클래스의 badAll() 메서드는 Food 리스트를 매개변수로 받아서 `음식이 상하는` 처리를 합니다. 하위 타입인 IceCream 객체가 전달될 수 있으므로 타입을 확인하고 IceCream 객체일 경우 형변환을 하여 melt() 메서드를 호출합니다. Food 객체일 경우 bad() 메서드를 호출합니다. 이는 LSP 원칙을 위반하는 것입니다. 상위 타입인 Food 객체가 하위 타입인 IceCream 객체로 바뀌어도 같은 기능을 사용해야 하는데 IceCream 객체일 경우 bad() 메서드가 아닌 melt() 메서드를 사용하기 때문입니다.

### LSP 원칙 반영

```java
public class Food {

    public void bad() {
        System.out.println("음식이 상했습니다.");
    }
}

public class IceCream extends Food {

    @Override
    public void bad() {
        melt();
    }

    public void melt() {
        System.out.println("아이스크림이 녹았습니다.");
    }
}

public class FoodUtil {

    public void badALl(List<Food> foods) {
        foods.forEach(Food::bad);
    }
}
```

IceCream 클래스에서 melt() 메서드를 호출하도록 오버라이딩했습니다. 이제 FoodUtil 클래스의 badAll 메서드에서는 Food 객체의 실제 타입이 무엇인지 확인하지 않고 bad() 메서드를 호출할 수 있습니다.

이와 같이 LSP 원칙을 지킴으로써 상위 타입을 사용하는 코드에 하위 타입을 전달하더라도 상위 타입의 메서드만을 사용하여 기능이 정상적으로 동작하는 것을 보장할 수 있습니다.

## ISP, 인터페이스 분리 원칙

ISP 원칙은 `인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다`는 규칙을 제시합니다.

클래스에서 사용하는 기능만 제공하는 인터페이스를 상속해야 한다는 의미입니다.

### ISP 원칙 위반 예시

```java
public interface Ship {

    void forward();
}

public class PassengerShip implements Ship {

    @Override
    public void forward() {
        // 전진 기능 구현
    }
}
```

Ship 인터페이스는 배의 기능을 정의하는 인터페이스입니다. 현재 앞으로 전진하는 기능을 하는 메서드가 선언되어 있습니다. PassengerShip 클래스는 Ship 인터페이스를 상속하고 forward() 메서드를 적절하게 구현합니다. 이때 잠수함을 의미하는 TourSubmarine 클래스를 추가했을 때 코드는 아래와 같이 변경될 수 있습니다.

```java
public interface Ship {

    void forward();

    void subside();
}

public class PassengerShip implements Ship {

    @Override
    public void forward() {
        // 전진 기능 구현
    }

    @Override
    public void subside() {
        throw new Exception("사용할 수 없는 기능입니다.");
    }
}

public class TourSubmarine implements Ship {

    @Override
    public void forward() {
        // 전진 기능 구현
    }

    @Override
    public void subside() {
        // 잠수 기능 구현
    }
}
```

Ship 인터페이스에 잠수하는 기능을 의미하는 subside() 메서드가 정의되었고, PassengerShip 클래스와 TourSubmarine 클래스에 forward(), subside() 메서드가 구현되었습니다. 그런데 PassengerShip 클래스의 subside() 메서드는 예외를 발생시킵니다. 잠수 기능을 사용해서는 안되는 클래스이기 때문입니다. 이는 ISP 원칙을 위반하는 것입니다.

ISP 원칙을 지키기 위해 인터페이스를 적절하게 분리할 필요가 있습니다.

### ISP 원칙 반영

```java
public interface Ship {

    void forward();
}

public interface Submarine extends Ship {

    void subside();
}

public class PassengerShip implements Ship {

    @Override
    public void forward() {
        // 전진 기능 구현
    }
}

public class TourSubmarine implements Submarine {

    @Override
    public void forward() {
        // 전진 기능 구현
    }

    @Override
    public void subside() {
        // 잠수 기능 구현
    }
}
```

위와 같이 Ship 인터페이스를 상속하는 Submarine 인터페이스를 추가합니다. 그리고 PassengerShip 클래스는 Ship 인터페이스를 상속받으며, TourSubmarine 클래스는 Submarine 인터페이스를 상속받습니다.

이를 통해 PassengerShip 클래스는 자신이 사용하는 전진 기능만 구현하면 되며, TourSubmarine 클래스는 전진 기능과 잠수 기능을 모두 구현할 수 있습니다.

이와 같이 ISP 원칙을 지킴으로써 불필요한 기능을 구현하는 리소스를 줄일 수 있습니다. 또한 인터페이스의 크기가 과하게 커지는 것을 방지할 수 있습니다.

## DIP, 의존 역전 원칙

DIP 원칙은 `고수준 모듈이 저수준 모듈의 구현에 의존해서는 안 되며, 저수준 모듈이 고수준 모듈에 의존해야 한다`는 규칙을 제시합니다.

객체는 저수준 모듈인 구현 클래스보다 추상화된 고수준 모듈인 인터페이스나 추상 클래스에 의존해야 한다는 의미입니다.

### DIP 원칙 위반 예시

```java
public class Chicken {

    private int price;

    private boolean fresh;
}

public class Pizza {

    private int price;

    private boolean fresh;
}

public class Menu {

    private Chicken chicken;

    private Pizza pizza;
}
```

현재 Menu 클래스는 Chicken, Pizza 클래스를 의존하고 있습니다. 만약 햄버거, 스테이크 메뉴가 추가된다면 Menu 클래스는 다음과 같이 변경될 것입니다.

```java
public class Menu {

    private Chicken chicken;

    private Pizza pizza;

    private Hamburger hamburger;

    private Steak steak;
}
```

위의 코드처럼 새로운 메뉴가 추가될 때마다 Menu 클래스는 수정해야만 합니다.

이는 DIP 원칙을 위반했기 때문에 발생하는 문제입니다. 현재 Menu 클래스는 저수준 모듈인 Chicken, Pizza 클래스를 의존하고 있습니다. DIP 원칙을 반영하여 의존 관계를 역전시켜야 합니다.

### DIP 원칙 반영

```java
public class Food {

    private int price;

    private boolean fresh;
}

public class Chicken extends Food {

}

public class Pizza extends Food {

}

public class Menu {

    private List<Food> foods;
}
```

위와 같이 Chicken, Pizza 타입을 추상화한 Food 클래스를 만들고 Menu 클래스는 고수준 모듈인 Food 클래스만을 의존하도록 했습니다. 이제 새로운 메뉴가 추가되더라도 Food 클래스를 상속한다면 Menu 클래스는 수정할 필요가 없게 되었습니다.

이제 Menu 클래스는 고수준 모듈인 Food 클래스만을 의존하게 되었고, Chicken, Pizza 같은 저수준 모듈도 Food 클래스를 의존하게 되었습니다.

이처럼 코드 상에서 의존을 역전시킴으로써 DIP 원칙을 지킬 수 있습니다.
