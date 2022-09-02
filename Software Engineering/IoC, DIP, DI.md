### Contents

- [IoC](#ioc)
- [DIP](#dip)
- [DI](#di)

# IoC, DIP, DI

IoC, DIP, DI에 대해 알아봅니다. 본문을 요약하자면 다음과 같습니다.

`객체를 사용할 때 IoC 달성을 위해 외부에서 생성자를 통해 객체를 DI한다. 이때 DIP 달성을 위해 주입받을 객체의 타입은 추상화된 타입이어야 한다.`

## IoC, Inversion of Control

제어의 역전을 뜻하는 IoC는 말 그대로 제어가 역전되는 것을 의미합니다. 여기서 제어란 코드가 실행되는 과정에서 사용되는 객체를 생성하는 것을 말합니다. 역전이란 객체 생성을 직접하는 것이 아닌 외부로부터 전달받는 것을 말합니다.

### IoC 위반 예시

```java
public class JpaMemberRepository implements MemberRepository {

    // ...
}

public class MemberService {

    public void register(Member member) {
        MemberRepository memberRepository = new JpaMemberRepository;
        memberRepository.save(member);
    }
}
```

위 코드에서 register() 메서드는 회원가입을 위해 직접 MemberRepository 객체를 생성하므로 IoC를 위반합니다.

### IoC 반영

```java
public class JpaMemberRepository implements MemberRepository {

    // ...
}

public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(JpaMemberRepository jpaMemberRepository) {
        this.memberRepository = jpaMemberRepository;
    }

    public void register(Member member) {
        memberRepository.save(member);
    }
}
```

위 코드에서는 MemberRepository 객체를 직접 생성하는 것이 아닌 생성자를 통해 외부로부터 주입받습니다. 객체 생성을 직접 하지 않고 외부로부터 전달받아 사용하므로 IoC를 달성했다고 할 수 있습니다.

IoC를 통해 객체 생성에 대한 책임을 외부에 위임하므로 비즈니스 로직에서 개발자가 직접 객체를 생성할 필요가 없게 됩니다.

## DIP, Dependency Inversion Principle

의존 역전 원칙을 뜻하는 DIP는 객체지향 설계 5대 원칙 중 하나로 다음과 같은 규칙을 제시합니다.

`고수준 모듈이 저수준 모듈의 구현에 의존해서는 안되며, 저수준 모듈이 고수준 모듈에 의존해야 한다`

그리고 일반적으로 IoC와 함께 사용됩니다.

### DIP 위반 예시

```java
public class JpaMemberRepository implements MemberRepository {

    // ...
}

public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(JpaMemberRepository jpaMemberRepository) {
        this.memberRepository = jpaMemberRepository;
    }

    public void register(Member member) {
        memberRepository.save(member);
    }
}
```

IoC를 반영한 코드를 다시 보겠습니다. MemberRepository 객체를 외부에서 주입받으므로 IoC는 잘 지키고 있습니다. 하지만 외부에서 주입받는 타입이 MemberRepository 타입의 하위 타입인 JpaMemberRepository입니다. 이는 DIP를 위반하는 것입니다.

### DIP 반영

```java
public class JpaMemberRepository implements MemberRepository {

    // ...
}

public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public void register(Member member) {
        memberRepository.save(member);
    }
}
```

DIP를 지키기 위해 객체를 주입받을 때 고수준 모듈인 MemberRepository로 주입받습니다. 즉, MemberService가 JpaMemberRepository 객체를 의존하지 않도록 합니다.

이를 통해 MemberRepository 타입의 하위 타입이 새로 추가되더라도 MemberService는 변경할 필요가 없게 됩니다.

## DI, Dependency Injection

DI는 의존성을 주입하는 것을 의미합니다. 의존성이란 객체 간에 의존 관계를 의미 하며 이러한 의존 관계를 외부에서 설정하는 것이라고 할 수 있습니다.

DI를 하는 방법은 세 가지가 있으며 생성자를 통한 DI가 가장 많이 사용됩니다.

### 생성자

```java
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

DIP를 지키고 있는 코드를 다시 보겠습니다. 위 코드에서 MemberService는 MemberRepository에 의존하고 있습니다. 그리고 생성자를 통해서 MemberRepository를 주입받는데, 이러한 방법을 생성자 DI라고 합니다.

### 수정자

```java
public class MemberService {

    private MemberRepository memberRepository;

    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

수정자 DI는 말 그대로 수정자를 통해 객체를 DI하는 방법입니다. 생성자 Di와 다르게 수정자를 통해 객체를 변경할 수 있습니다.

### 필드

```java
@Repository
public class JpaMemberRepository implements MemberRepository {

    // ...
}

@Service
public class MemberService {

    @Autowired
    private MemberRepository memberRepository;
}
```

스프링 프레임워크를 사용한다면 필드 DI를 사용할 수 있습니다. MemberRepository를 구현하는 JpaMemberRepository 클래스를 스프링 Bean을 등록하고 MemberService에서는 MemberRepository 필드에 @Autowired 애노테이션을 붙이면 됩니다.

### 결론

생성자 DI를 사용하는 것이 좋습니다.

수정자와 필드를 통해 DI하는 방법은 다음과 같은 단점이 있습니다.

1. 수정자 DI의 경우 수정자 메서드를 통해 객체가 언제든지 변경될 수 있습니다. 의존하는 객체를 변경할 필요가 없다면 수정자 메서드의 사용을 막는 것이 좋습니다.
2. 필드 DI의 경우 테스트 코드를 작성할 때 Mock 객체를 주입하는 것이 어렵습니다.

   - 생성자 DI를 통해 테스트 코드에서 Mock 객체를 주입하여 테스트 성능을 높일 수 있습니다.

     ```java
     class MemberServiceTest {

         private final MemberService memberService;

         @BeforeEach
         public void setUp() {
             memberService = new MemberService(new MemoryMemberRepository());
         }

         @DisplayName("회원가입 성공 테스트")
         @Test
         public void registerSuccess() {
             // given
             Member member = new Member("id", "password");

             // when
             memberService.register(member);

             // then
             Member findMember = memberRepository.findById("id");
             assertThat(findMember.getId()).equals("id");
         }

         private class MemoryMemberRepository implements MemberRepository {

             private Map<String, Member> store = new HashMap<>();

             @Override
             public Member save(Member member) {
                 store.put(member.getId(), member);
                 return member;
             }

             @Override
             public Member findById(String id) {
                 return store.get(id);
             }
         }
     }
     ```

위 코드에서는 JpaMemberRepository 대신 테스트용으로 MemoryMemberRepository를 구현하여 사용했습니다. 이를 통해 외부 데이터베이스와 연결하지 않고 MemberService의 register() 메서드의 핵심 비즈니스 로직을 빠르게 테스트할 수 있습니다.

또한 생성자 DI를 사용하면 다음과 같은 장점도 있습니다.

- 생성자를 통해 객체를 주입받으므로 final 키워드를 사용하여 객체의 불변성을 보장할 수 있습니다.
- 메인 객체가 생성될 때 모든 의존성을 주입받을 수 있습니다. 이를 통해 NullPointerException을 방지할 수 있습니다.
