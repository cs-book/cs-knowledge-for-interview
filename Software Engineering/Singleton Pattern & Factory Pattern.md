# Singleton Pattern & Factory Pattern

Created: 2022년 6월 8일 오후 7:08
Tags: CS, Design Pattern, SW Engineering

## 1. 싱글톤 패턴

하나의 `클래스`가 오직 하나의 `인스턴스`만 가지는 패턴
**하나의 메모리만을 인스턴스로 할당하는 패턴**으로 봐도 무방

1. 프로세스 내에서 객체가 하나만 만들어져야 할 때
2. 굳이 중복된 객체 생성 비용을 감당할 필요가 없을 때

![Untitled](Singleton%20Pattern%20&%20Factory%20Pattern%207d83bb9240da475d90a81f584c308758/Untitled.png)

### 1.1 특징

<aside>
⭐ 소프트웨어 디자인 패턴에서 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, **생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴**한다. 이와 같은 디자인 유형을 싱글턴 패턴이라고 한다.

</aside>

하나의 인스턴스를 만들어 놓고 **해당 인스턴스를 여러 모듈에서 공유하며 사용**하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어든다. 하지만 하나의 인스턴스에 의존하기 때문에 **의존성이 높아진다**는 단점도 있다.

### 1.2 JavaScript의 싱글톤 패턴

- 리터럴 `{}` 또는 `new Object`로 객체 생성 시 다른 어떤 객체와도 같지 않기 때문에 이 자체만으로도 싱글톤 패턴 구현 가능

```tsx
const obj1 = {
	algo: "cs-book"
}

const obj2 = {
	algo: "cs-book"
}

console.log(obj1 === obj2)
// false
```

각 오브젝트는 겉보기엔 같아보여도 다른 메모리를 참조하는 서로 다른 인스턴스

### 1.3 JS에서의 주된 싱글톤 구현 패턴

```tsx
class Singleton {
	constructor() {
		if (!Singleton.instance) {
			Singleton.instance = this
		}
		return Singleton.instance
	}
	getInstance() {
		return this.instance
	}
}

const a = new Singleton()
const b = new Singleton()

console.log(a === b) // true
```

`Singleton.instance`라는 하나의 인스턴스를 가지는 `Singleton` 클래스를 구현한 모습. `a`와 `b`는 하나의 인스턴스를 공유한다.

### 1.4 데이터베이스 연결 모듈

싱글톤 패턴은 특히 DB와 연결하는 부분에서 많이 사용된다고…

```tsx
const URL = "mongodb://cs-book.com:271017"

const createConnection = (URL) => {
	veryExpensiveConnectionFunc(URL)
}

class DB() {
	constructor(url) {
		if (!DB.instance) {
			DB.instance = createConnection(url)
		}
		return DB.instance
	}
	connect() {
		return this.instance
	}
}

const a = new DB(URL)
const b = new DB(URL)

console.log(a === b) // true
```

`DB.instance`라는 하나의 인스턴스를 기반으로 `a`, `b`를 생성. 이를 통해 매번 연결을 시도하지 않고 연결에 관한 인스턴스를 받아옴으로써 인스턴스 생성 비용을 아낄 수 있다.

### 1.5 Java에서의 싱글톤 패턴

 여러 명의 사원이 하나의 프린터기를 사용하는 경우의 예제

```java
package csbook.softwareEngineering.signleton
    
public class Printer {
	private static Printer printer = null;

	private Printer(){}

	public static Printer getInstance() {
		if(printer == null) {
			printer = new Printer();
		}
		return printer;
	}

	public void print(String input) {
		System.out.println(input);
	}
}
```

- 기본생성자를 `private`를 사용하여 생성을 불가능하게 하고, `getInstance`를 통해서만 생성
- `getInstance`는 내부적으로 생성되지 않았다면 생성하고, 기존에 생성된 값이 존재한다면 생성된 인스턴스를 리턴하는 형태로 프로그램 전반에 걸쳐 하나의 인스턴스를 유지
- 메서드와 인스턴스 변수 모두 **`static`으로 선언**된 정적 변수 및 메서드. 당연히 기본생성자를 통해 생성할 수 없기 때문에 외부에서 인스턴스에 접근하려면 클래스 변수 및 메서드에 접근을 허용해야하기 때문에 두 메서드는 정적타입으로 선언

### 1.7 단점

1. 멀티쓰레드 환경에서 안전하지 않다.
    1. 생성자가 여러 쓰레드에서 동시에 실행된다면?
    2. 여러 쓰레드에서 작업 중이기 때문에 일관되게 유지하고 싶은 상태가 있다면, 이를 관리하기가 어렵다.
2. 싱글톤 인스턴스를 참조하는 모든 모듈에 의존성이 생긴다
    1. TDD하기 어려움 - 단위 기능 테스트를 하는 데, 독립적인 테스트를 하기 어렵다. (예를 들어, 모든 테스트에서 독립적으로 테스트할 때 DB를 생성해야 함)
    2. 재사용성이 낮아짐

### 1.8 해결 방법

1. 멀티 쓰레드 환경
    1. 최초 인스턴스가 클래스를 호출하기 전에, 
    선제적으로 클래스가 메모리에 올라올 때 부터 최초 인스턴스를 만들어 버리기
        
        `private static Printer printer = null;` 
        → `private static Printer printer = new Printer();`
        
    2. 상태는 `synchronized` 를 통해 관리하기 (`java`)
        - 각 언어별로 동시에 접근하는 것을 막는 방법들이 있다고 함
        - 예를 들어, golang 같은 경우에도 `sync.Once`와 같이 최초 한 번만 실행되게 하는 기능들이 있음
2. 의존성
    
    **의존성 주입**을 통해 해결
    
    **의존성 주입이란?**
    
    <aside>
    ⭐ 클래스 모델이나 코드에는 **런타임 시점의 의존관계가 드러나지 않는다**. 그러기 위해서는 인터페이스만 의존하고 있어야 한다.
    
    - 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 **제3의 존재가 결정**한다.
    - *의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로써 만들어진다.*
        - *이일민, 토비의 스프링 3.1, 에이콘(2012), p114*
    </aside>
    
    예를 들어, 무슨 프린터인지는 클래스에서 관심 없게 만들어 버리고, 클래스 바깥에서 프린터의 종류를 관리
    
    ```java
    class Printer {
    		...
        public void setPrinterDevice(Printer printer) {
            this.Printer = printer;
        }
    }
    
    class Employee {
        private Printer printer = new Printer();
    
        public void changePrinter() {
            printer.setPrinterDevice(new OfficePrinterA());
        }
    }
    ```
    

### 1.9 Reference

- 싱글톤 패턴
    - [https://velog.io/@kyle/자바-싱글톤-패턴-Singleton-Pattern](https://velog.io/@kyle/%EC%9E%90%EB%B0%94-%EC%8B%B1%EA%B8%80%ED%86%A4-%ED%8C%A8%ED%84%B4-Singleton-Pattern)
- 의존성 주입
    - [https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/](https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/)

## 2. 팩토리 패턴

여러 객체들을 생성해 가져오는 공장 역할을 하는 패턴

### 2.1 특징

- 상위 클래스인 팩토리 클래스는 어떤 인스턴스를 어떻게 가져올 지 계획
- 하위 클래스는 상위 클래스에서 지정한 일의 종류에 따라 할당 받은 일 (=객체 리턴)을 수행

![Untitled](Singleton%20Pattern%20&%20Factory%20Pattern%207d83bb9240da475d90a81f584c308758/Untitled%201.png)

### 2.2 장점

<aside>
⭐ 그냥 조그만 의자, 중간 의자, 큰 의자 인스턴스 호출하면 되지, **굳이 이걸 왜 하나요?**

</aside>

1. 작은 의자만 만드는 공장, 큰 의자만 만드는 공장이 어딘지 관심 없고 그냥 의자 공장에다 주문
2. 의자 주문서의 변경(**객체 생성 방식 변경**)을 해당 의자 부서 직원이 623 군데의 거래처(**코드**)를 돌며 처리하지 않고 단 하나의 직영 대리점(**팩토리**)에서 처리할 수 있음

> 대부분의 라이브러리를 사용할 때 우리는 팩토리 패턴의 효과를 보고 있음
> 

### 2.3 팩토리 패턴 in TypeScript

**원하는 결과**

고객은 관심있는 종류의 의자 인스턴스를 바로 호출

```tsx
import ChairFactory from './chair-factory'

const CHAIR = ChairFactory.getChair('small')
console.log(CHAIR.getDimensions())
```

```tsx
export type dimension = {
    height: number
    width: number
    depth: number
}
```

```tsx
import { dimension } from './dimension'

// A Chair Interface
interface IChair {
    height: number
    width: number
    depth: number
    getDimensions(): dimension
}

// The Chair Base Class
export default class Chair implements IChair {
    height = 0
    width = 0
    depth = 0

    getDimensions(): dimension {
        return {
            width: this.width,
            depth: this.depth,
            height: this.height,
        }
    }
}
```

```tsx
// smallChair.ts
import Chair from './chair'

export default class SmallChair extends Chair {
    constructor() {
        super()
        this.height = 40
        this.width = 40
        this.depth = 40
    }
}
```

```tsx
// mediumChair.ts
import Chair from './chair'

export default class MediumChair extends Chair {
    constructor() {
        super()
        this.height = 60
        this.width = 60
        this.depth = 60
    }
}
```

```tsx
// bigChair.ts
import Chair from './chair'

export default class BigChair extends Chair {
    constructor() {
        super()
        this.height = 80
        this.width = 80
        this.depth = 80
    }
}
```

```tsx
import SmallChair from './smallChair'
import MediumChair from './mediumChair'
import BigChair from './bigChair'
import IChair from './chair'

type chairList = "small" | "medium" | "big"

export default class ChairFactory {
    static getChair(chair: chairList): IChair {
        if (chair == 'big') {
            return new BigChair()
        } else if (chair == 'medium') {
            return new MediumChair()
        } else {
            return new SmallChair()
        }
    }
```

### 2.4 Reference

- 팩토리 패턴은 언제 사용하나요?
[https://stackoverflow.com/questions/69849/factory-pattern-when-to-use-factory-methods](https://stackoverflow.com/questions/69849/factory-pattern-when-to-use-factory-methods)
- TS에서의 팩토리 패턴 
[https://sbcode.net/typescript/factory/](https://sbcode.net/typescript/factory/)