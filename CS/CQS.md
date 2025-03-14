# CQS

- CQS(Command Query Separation, 명령 - 조회 분리 원칙)
- 객체의 메서드를 `명령`(Command)과 `조회`(Query)로 명확히 분리하는 원칙
- 1986년 버트란드 마이더가 처음 제안한 개념
- 객체의 상태를 변경하는 메서드와 객체의 상태를 조회하는 메서드를 구분함으로써 코드의 명확성을 높이는 설계 원칙

## CQS의 장점

1. 코드의 명확성 증가
    - 명령(변경)과 조회(읽기)가 분리되어 있어 기능별 역할이 명확해짐.
    - 메서드가 어떤 역할을 수행하는지 직관적으로 파악 가능.
2. 테스트 및 유지보수 용이
    - Query는 부작용(side Effect)이 없으므로, 단위 테스트가 쉬움.
    - Command와 Query가 섞이지 않아 디버깅이 용이함.
3. 병렬 처리 및 동시성 문제 감소
    - Command와 Query를 분리하면, Query는 여러 스레드가 동시 접근 가능하여 성능이 향상됨.
    - Command와 별도의 트랜잭션으로 처리하면 경합 조건(Race Condition)을 줄일수 있음.

## CQS의 단점

1. 코드 복잡성 증가
    - 단순한 시스템에서는 Command와 Query를 분리하는 것이 오히려 불필요한 복잡성을 초래 할수 있음.
2. 트랜잭션 관리가 어려울 수 있음
    - Command는 트랜잭션을 필요로 하지만 Query는 그렇지 않음.
    - 분리된 구조에서는 데이터 정합성을 맞추는 추가적인 로직이 필요할 수 있음.
3. 실행 성능 저하 가능
    - 한 번의 호출로 해결할 수 있는 작업을 여러번 나눠서 수행해야 하는 경우
    - 예를 들어, 상태 변경 후 상태 조회가 필요하면 두번의 호출이 필요하므로 성능이 저하될수 있음.

## CQS가 적용되지 않은 예제와 적용된 예제

```java
// CQS 적용 ❌ - Command와 Query가 섞여 있어 역할이 불분명함.
public class BankAccount {
    private double balance;

    // 상태 변경 + 상태 조회를 동시에 수행 (CQS 위반)
    public double depositAndGetBalance(double amount) {
        this.balance += amount;
        return this.balance;
    }
}
```

```java
// CQS 적용 ⭕️ - Command와 Query를 분리하여 코드 가독성이 향상됨.
public class BankAccount {
    private double balance;

    // Command (상태 변경)
    public void deposit(double amount) {
        this.balance += amount;
    }

    // Query (조회)
    public double getBalance() {
        return this.balance;
    }
}
```

## 결론

- CQS는 `데이터 변경`과 `데이터 조회`를 분리하는 원칙으로, 코드 명확성과 유지보수성을 높이는 데 기여함.
- 테스트 및 병렬 처리에 유리하지만, 트랜잭션 관리나 성능 저하 등의 단점도 존재함.
- CQRS 아키텍처에서도 사용되며, 대규모 시스템에서는 효과적인 설계 방식이 될 수 있음.
- CQS를 적용할지 여부는 시스템의 복잡도와 요구사항에 따라 결정하는 것이 중요합니다!

---
`김영환`님의 [인프런 질문 답변](https://www.inflearn.com/community/questions/27795/cqrs)
- 이게 웹 애플리케이션에서 얻는 이점이 있다기 보다는, 개발 전반에 기본개념으로 깔고 가시는 것이 좋습니다.
- 이 메서드를 호출 했을 때, 내부에서 변경(사이드 이펙트)가 일어나는 메서드인지, 아니면 내부에서 변경이 전혀 일어나지 않는 메서드인지 명확히 분리하는 것이지요.
- 그렇게 되면 데이터 변경 관련 이슈가 발생했을 때, 변경이 일어나는 메서드만 찾아보면 됩니다^^
- 정말 크리티컬한 이슈들은 대부분 데이터를 변경하는 곳에서 발생하지요.
- 변경 메서드도 변경에만 집중하면 되기 때문에 유지보수가 더 좋아집니다.
- 제가 권장하는 방법은 insert는 id만 반환하고(아무것도 없으면 조회가 안되니), update는 아무것도 반환하지 않고, 조회는 내부의 변경이 없는 메서드로 설계하면 좋습니다^^

## CQRS (Command Query Responsibility Segregation)와의 연결

- CQS는 CQRS 아키텍처의 기초 개념으로 사용됨.
- CQRS는 Command와 Query를 각각 다른 시스템으로 분리하는 아키텍처.