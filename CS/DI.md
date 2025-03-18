## IoC(제어의 역전)

- 객체 생성과 의존성 관리의 책임을 외부 컨테이너(스프링)에게 맡기는 개념
- 즉 객체 간의 의존 관계를 코드에서 직접 관리하는 것이 아니라 외부에서 관리하여 객체를 주입하는 방식
- `외부에서 관리하는 객체를 가져와 사용하는 것`

```java
public class A {
    B b = new B();
}
```

- IoC 적용 후: A는 객체는 B를 직접 생성하지 않고, 외부에서 제공하는 B 객체를 받아옵니다.

```java
public class A {
    private B b;
}
```

- 제어의 역전을 구현하기 위해 사용하는 방법이 DI입니다.

## DI 의존 관계

- 의존성 주입 혹은 의존 관계 주입이라고도 불린다.
- 외부에서 객체 간의 관계를 결정해 주는데 즉 객체를 직접 생성하는 것이 아니라
- 외부에서 생성 후 주입시켜 주는 방식입니다.

## DI 3가지 방법

- Construct Injection(생성자 주입)
- Field Injection(필드 주입)
- Setter Injection(Setter 주입)

### 1. Construct Injection(생성자 주입)

- @RequiredArgsConstructor
- 클래스의 생성자를 통해 의존성을 주입하는 방식입니다.
- 생성자 주입은 인스턴스가 생성될 때 1회 호출되는 것이 보장
- 생성자 주입시 필드에 final 키워드를 사용할 수 있다.

```java

@Controller
public class PetController {

    private final PetService petService;

    @Autowired
    public PetController(PetService petService) {
        this.petService = petService;
    }
}
```

### 2. Field Injection(필드 주입)

- 필드 주입은 클래스에 선언된 필드에 생성된 객체를 주입해주는 방식
- @Autowired 어노테이션을 주입할 필드위에 명시
- 과거 필드 주입을 이용하면 코드가 간결해지면서 과거에 많이 이용
-

```java

@Controller
public class OrderController {
    @Autowired
    private OrderService orderService;
}
```

### 3. Setter Injection(수정자 주입)

- 필드 값을 변경하는 Setter를 통해서 의존 관계를 주입하는 방법
- Setter 주입은 생성자 주입과 다르게 주입 받은 객체가 변경될 가능성이 있는 경우에 사용

```java
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

## 스프링에서는 생성자 주입을 권장한다.

- 인텔리제이에서 사용한다면 필드 주입을 사용할 때 경고줄을 나타낸다.

### 생성자 주입을 권장하는 이유

1. 객체의 불변성
    - 생성자 주입 방식을 사용할 경우 객체가 생성되는 시점에 생성자를 호출하여 최초 1회만 주입합니다.
    - 이와 같은 특성으로 불변 객체를 보장합니다.

2. 순환 참조 문제 방지 기능
    - 순한 참조란 A, B 두 객체가 각각 서로를 필드에 포함하여 참조하고 있는 상태를 말합니다.
    - 서로가 서로를 참조하기 때문에 A,B 두 클래스가 맞물려서 서로의 객체를 계속 생성하는 무한반복상태에 빠지는 문제
    - 필드 주입, 수정자 주입 모두 순한 참조 문제가 발생하지만 시점이 다르다.
    - 필드 주입, 스정자 주입은 메소드가 호출되었을 때 발생해 예측이 어렵다
    - 생성자는 어플리케이션이 구동되는 순간에 에러가 발생해 쉽게 추적이 가능
3. 테스트 용이
    - 단위 테스트를 진행할 대 순수 자바 코드로 테스트가 가능합니다. 필드 주입을 사용하는 경우
    - 순수 자바코드에서는 DI가 이루어지지 않기 때문에 필드가 null 상태가 되어 에러가 발생한다.

## AOP

- `AOP`는 핵심 비즈니스 로직과 부가적인 관심사를 분리하는 방법론입니다.
- 이를 통해 관점(aspect)이라는 기준으로 모듈화하여 코드의 유연성과 확장성을 높입니다.
- 핵심 관점: 비즈니스 로직(예: 계좌 이체, 고객 관리).
- 부가 관점: 로깅, 트랜잭션 관리, 보안 등 애플리케이션 전체에 걸쳐 적용되는 로직.
- 횡단 관심사(cross-cutting concerns): 여러 곳에서 공통적으로 발생하는 작업을 말하며 로깅, 보안, 트랜잭션 관리 등이 포함됩니다.  
  이를 `AOP`를 통해 핵심 비즈니스 로직에서 분리하여 코드의 간결성과 변경의 용이성을 높일 수 있습니다.

### 요약

- IoC: 객체 생성과 의존성을 외부에서 관리하고 객체가 자신을 생성하지 않고 외부에서 주입받는 방식
- DI: 객체 간의 의존성을 외부에서 주입하는 방식으로, 의존성 주입은 필드, 수정자, 생성자 3가지 방식이 있다.
    - 객체의 불변성 보장과 순환 참조 방지, 테스트 용이와 같은 이유 때문에 생성자 주입이 권장한다.
- AOP는 핵심 비즈니스 로직과 부가 기능을 분리하여 코드의 유연성과 확장성을 높이는 기법입니다.

1. [[Spring] 의존성 주입(Dependency Injection)에 대하여](http://velog.io/@think2wice/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
2. [스프링의 콘셉트(IoC, DI, AOP, PSA) 쉽게 이해하기](https://shinsunyoung.tistory.com/133)
3. [Spring AOP 개념](https://velog.io/@kai6666/Spring-Spring-AOP-%EA%B0%9C%EB%85%90)