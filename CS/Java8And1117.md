# Java8, Java11, Java17

## 플랫폼 호환

2022년 11월 출시한 Spring Boot3.0부터 JDK17이상을 지원합니다.

## 주요 기능 업데이트

- Java8 → Java17 사이에 많은 내용들이 추가 되었지만 주요 기능들만 요약

### Java8 → Java11

1. Collection Factory Method 강화
    - Set, List, Map 인터페이스에 Immutable 생성할 수 있는 새로운 메서드가 추가.
    - 기본보다 깔끔한 코드스타일을 가져갈 수 있고, Collection 초기화 값을 설정하여 사용 가능.
    - 기본적으로 Immutable 속성을 가지므로 데이터 변경이 어렵지만 추가, 수정, 삭제가 필요할 경우 가능
   ### 변경 전

    ```java
    public class CollectionOfExample { 
         public static void main(String[] args) {
             Set<String> set = new HashSet<>();
             set.add("KBS");
             set.add("MBC");
            
             List<String> list = new ArrayList<>();
             list.add("유재석");
             list.add("미주");
             list.add("이이경");
            
             Map<String, String> map = new HashMap<>();
             map.put("H","홍길동");
             map.put("L","이순신");
         }
    }
   
    ```

   ### 변경 후

    ```java
    public class CollectionOfExample { 
         public static void main(String[] args) {
             Set<String> set = Set.of("SBS", "MBC");
            
             List<String> list = List.of("유재석", "미주", "이이경");
            
             Map<String, String> map = Map.of(
                   "H", "홍길동"
                   , "L", "이순신"
             );
         }
    }
    ```
   ### 추가 작업
    ```java
    public class CollectionOfExample { 
         public static void main(String[] args) {
             // 불변 Set 생성
             Set<String> set = Set.of("SBS", "MBC");
             // 수정 가능한 Set으로 변환 후 요소 추가
             Set<String> cusSet = new HashSet<>(set);
             cusSet.add("KBS");
             System.out.println("수정된 Set: " + cusSet); // [SBS, MBC, KBS]
   
             // 불변 List 생성
             List<String> list = List.of("유재석", "미주", "이이경");
             // 수정 가능한 List로 변환 후 요소 추가
             List<String> cusList = new ArrayList<>(list);
             cusList.add("놀러와");
             System.out.println("수정된 List: " + cusList); // [유재석, 미주, 이이경, 놀러와]
   
             // 불변 Map 생성
             Map<String, String> map = Map.of(
                 "H", "홍길동",
                 "L", "이순신"
             );
             // 수정 가능한 Map으로 변환 후 요소 추가
             Map<String, String> cusMap = new HashMap<>(map);
             cusMap.put("K", "김수로");
             System.out.println("수정된 Map: " + cusMap); // {H=홍길동, L=이순신, K=김수로}
         }
    }
    ```
2. 로컬 변수 타입 추론 `var`
    - 명시적 타입 선언 없이 변수 선언 가능
    - 기존 Lombok 기능이었으나 JDK10부터 공식 지원
    - LTS 버전인 JDK11부터 람다 표현식에서도 사용 가능
    - 컴파일 시 타입이 자동 추론되므로 성능에 영향 없음
    - 가독성을 위해 무분별한 사용은 지양

3. JDK 11부터 추가된 문자열 메서드
    - `isBlank()` → 문자열이 비어있거나 공백이면 true 반환
    - `lines()` → 줄 단위로 문자열을 배열로 반환
    - `strip()` → 유니코드 공백까지 제거 (trim()과 차이점)
    - `stripLeading()` → 문자열 앞 공백 제거
    - `stripTrailing()` → 문자열 뒤 공백 제거
    - `repeat(n)` → 문자열을 n번 반복

   ```java
   public class StringMethodsExample {
     public static void main(String[] args) {
         // isBlank() - 문자열이 비어있거나 공백인지 확인
         System.out.println("".isBlank());        // true
         System.out.println("   ".isBlank());    // true
         System.out.println("hello".isBlank());  // false
   
         // lines() - 문자열을 줄 단위로 나누어 Stream으로 반환
         String multiline = "Hello\nWorld\nJava 11";
         multiline.lines().forEach(System.out::println);
         // 출력:
         // Hello
         // World
         // Java 11
   
         // strip(), stripLeading(), stripTrailing()
         String spaced = "   hello   ";
         System.out.println("[" + spaced.strip() + "]");         // [hello]
         System.out.println("[" + spaced.stripLeading() + "]");  // [hello   ]
         System.out.println("[" + spaced.stripTrailing() + "]"); // [   hello]
   
         // repeat(n) - 문자열을 n번 반복
         String word = "Java ";
         System.out.println(word.repeat(3)); // Java Java Java 
     }
   }
   ```

### Java12 → Java17

1. 텍스트 블록
    - JDK15부터 지원한 기능
    - """ """ 형식을 사용하여 여러 줄 문자열을 쉽게 작성 가능
    - %s 와 format()을 활용해 동적 데이터 삽입 가능
   ```
   String json = """
     {
         "name": "홍길동",
         "age": 25,
         "city": "서울"
     }
     """;
   System.out.println(json);
   ```
2. Switch 표현식 향상
    - JDK14부터 지원한 기능
    - 값 직접 반환 가능
    - yield 예약어를 이용해 값 리턴 가능
    - case 문에서 람다식 지원
   ```java
   public class Example { 
      public static void main(String[] args) {
         int day = 3;
         String result = switch (day) {
            case 1, 2, 3 -> "초반";
            case 4, 5, 6 -> "중반";
            case 7 -> "후반";
            default -> throw new IllegalArgumentException("잘못된 값");
         };
         System.out.println(result); // 초반
       }
   }
   ```

3. Record Data Class 요약
    - JDK 14부터 추가된 Immutable 객체를 위한 새로운 클래스 유형
    - toString(), equals(), hashCode() 자동 생성
    - 불변 객체이므로 모든 필드는 생성자를 통해 초기화
    - DTO와 같은 데이터 객체로 활용 시 간결한 코드 작성 가능
   ```
   public record Person(String name, int age) {}
   
   Person person = new Person("홍길동", 25);
   System.out.println(person.name()); // 홍길동
   System.out.println(person.age());  // 25
   System.out.println(person);        // Person[name=홍길동, age=25]
   ```

### 참고자료

[여기어때 기술블로그 - 우리팀이 JDK 도입한 이유](https://techblog.gccompany.co.kr/%EC%9A%B0%EB%A6%AC%ED%8C%80%EC%9D%B4-jdk-17%EC%9D%84-%EB%8F%84%EC%9E%85%ED%95%9C-%EC%9D%B4%EC%9C%A0-ced2b754cd7)