
# String은 왜 Immutable(불변)인가?
(모든 설명은 JAVA를 기준으로 합니다.)

<br/>

## Immutable이란,
> 뜻 : **불변의, 변경할 수 없는** <br/>

Java에서의 String객체는 한 번 생성되면 절대로 그 값이 변하지 않는다.<br/>
왜 그럴까?

<br/>

## String Pool
우선 자바는 Heap의 String Pool이라는 공간에 String을 포함시켜,
매번 String객체를 생성하지 않고 값이 같다면 String Pool에 있는 객체를 재사용 할 수 있도록 구현하였다.

즉, String Pool안에있는 String 데이터를 공유/참조 한다는 뜻이다. <br/>
그렇기에 String은 Immutable해야하는 것이다.

만약 mutable하다 해보자

``` java
String a1 = "Hello";
String a2 = "Hello";
```

a1과 a2는 변수 명은 다르지만 같은 String 객체를 가리킨다.
``` java
a2 = "Bye";
```
a2을 "Bye"로 바꿔보자
a1과 a2는 **같은 String 객체**를 가리키는데 값이 다르다. 
이거슨 존재 할 수 없다!!

그렇기에 String은 Immutable인 것이다.

그리고 
``` java
a2 = "Bye"
```
은 "Hello"라는 String 객체가 "Bye" 로 바뀐게 아니라 "Bye"라는 객체를 String Pool에 새로 생성한 것이다.

<br/>

## Immutable하게 쓰면서 생긴 장점들이 있다
<br/>

### 캐싱
사용자가 많이 방문하는 naver
이 "Naver"도 문자열 데이터인데 사용자들이 올 때마다 "Naver" 객체를 만들어서 준다고 해보자 <br/>
100만명이 방문하면 100만개의 "Naver"객체가 생길 것이다. 메모리 낭비다.
근데 이를 String Pool에있는 Naver객체를 재사용함으로써 캐시와 같은 효과를 볼 수있다. 당연히 성능과 메모리 효율면에서 이득을 얻을 수 있다.

<br/>

### 해싱
불변이라 함은 해시코드가 언제나 같다는것을 의미
한번 해시함수를 거쳐 계산해두면 값이 변할 일이 없어서 안심할 수 있다. 
java는 String객체 생성시 부터 hashcode를 캐싱해놓는다고 한다. 즉 해시맵이나 테이블에서 키로 쓰일 때 해시함수 과정을 안거치고 사용해도 된다는 뜻이다.


<br/>

### 보안
메모리 주소에 저장된 String 값은 불변이기 때문에 보안면에서 유리하다.
파라미터로 넘겨줄 때 String의 값이 저장된 주소값을 전달받게 된다.
그 주소값을 따라가 메모리 상의 값을 참조 할 수 있지만 그 값을 변경하는것은 불가능 하므로 안전하다고 볼 수 있다.

<br/>

### 스레드 안전성
멀티 스레드환경을 보면 프로세스의 자원을 스레드들이 공유한다
String의 불변은 멀티 쓰레드 환경에서 String 객체를 참조하더라도 안전하다는 뜻이다.


<br/>

### 단점도 존재한다.
Immutable이기 때문에 
문자열을 합치거나 조작 해야하는 경우에 매번 새로운 String 객체를 생성해야한다는 단점이 있다. 이를 막기 위해서 StringBuilder나 StringBuffer를 사용한다.
python에는 join이 있다.

<br/>

## StringBuilder, StringBuffer
둘 다 mutable한 객체로
주소값이 변하지 않고 객체 내부의 배열의 공간을 조절해서 문자열을 추가, 삭제 할 수 있다. 

<br/>

문자열의 추가, 수정, 삭제 등이 빈번히 발생하는 경우라면 해당 객체를 사용하는게 좋다.

## StringBuilder, StringBuffer차이
둘은 같은 클래스를 상속한다. 기능이 비슷한데, 동기화 여부에서 차이가 있다.<br/>
StringBuilder는 동기화 지원을 안하고
StringBuffer는 동기화 지원을 한다.
<br/>

즉 멀티스레드 환경에서는 StringBuffer가 더 사용하기 안전하며<br/>
단일 스레드 환경에서는 StringBuilder가 조금 더 빠를 것이다.





## Reference
https://steady-coding.tistory.com/569 <br/>
https://steady-coding.tistory.com/569 <br/>
https://starkying.tistory.com/entry/why-java-string-is-immutable <br/>
https://dololak.tistory.com/699 <br/>
https://twinstae.github.io/string-immutable/ <br/>
https://yeon-kr.tistory.com/157 <br/>
