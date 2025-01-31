# 정적 메서드와 정적 팩토리 메서드

## 정적 메서드란 
- 객체를 생성하지 않고 클래스명으로 직접 호출할 수 있는 메서드
- static 키워드가 붙은 메서드
- 인스턴스 변수를 사용할 수 없음
- 주로 유틸리티 메서드를 만들 때 사용
### 예제: 일반적인 정적 메서드
`square()`는 단순한 기능을 제공하는 정적 메서드로, 객체 생성 없이 바로 사용할 수 있음.
```java
class MathUtil {
    // 정적 메서드 (유틸성 메서드)
    public static int square(int number) {
        return number * number;
    }
}

// 객체 없이 호출 가능
int result = MathUtil.square(5);  // 25
```

## 정적 팩토리 메서드란
- 객체를 생성하는 역할을 하는 정적 메서드
- 일반적인 `new` 키워드 대신 사용
- 보통 of(), from(), valueOf(), create() 같은 네이밍을 가짐
- 객체 생성을 캡슐화할 수 있어 유연한 객체 생성 로직을 적용할 수 잇음
### 예제: 정적 팩토리 메서드
`User` 클래스의 생성자는 private이므로 `new User("Alice",25)`로 직접 객체를 만들 수 없고 대신 `User.of("Alice",25)`를 호출하면 객체를 생성할 수 있음.
```java
class User {
    private String name;
    private int age;

    // private 생성자로 직접 인스턴스화 방지
    private User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 정적 팩토리 메서드
    public static User of(String name, int age) {
        return new User(name, age);
    }
}

// 정적 팩토리 메서드를 이용해 객체 생성
User user = User.of("Alice", 25);
```
| 비교항목       | 정적 메서드    | 정적 팩토리 메서드             |
|------------|-----------|------------------------|
| 목적         | 특정 기능을 수행 | 객체를 생성하는 역할            |
| 인스턴스 생성 여부 | X         | O                      |
| 사용 예       | 테스트2      | 객체 생성 (List.of(), User.of() |
| 생성자 호출 여부  | 테스트2      | 내부적으로 생성자를 호출하여 객체 생성  |
| 유연성        | 단순 기능 제공  | 캐싱, 서브클래스 반환, 다형성 적용 가능 |

## 정적 팩토리 메서드의 장점
- 이름을 가질수 있어 가독성이 좋음
  - 생성자보다 다양한 객체 반환이 가능
- 같은 메서드에서 다른 타입의 객체를 반환할 수도 있음
  - 캐싱을 활용하여 성능을 최적화할 수 있다.
- 동일한 객체를 재사용할 수 있음(싱글턴 패턴)
  - 불변 객체를 쉽게 만들수 있다.
