# JVM

- java 프로그램을 실행하기 위한 가상 머신
- 운영체제에 상관없이 java 프로그램이 실행될 수 있도록 해주는 역할

## JVM의 구조

- Class Loader(클래스 로더)
    - .class 파일을 로드하여 JVM 메모리에 적재
    - 클래스를 검증하고, 필요 시 링크 및 초기화 작업을 수행

- Runtime Data Area(런타임 데이터 영역)
    - JVM이 실행 중에 사용하는 메모리 영역으로, 크게 아래와 같이 나뉩니다.

      1️⃣ Method Area(메서드 영역, 클래스 메타데이터 저장소)

               - 클래스의 구조(메서드, 변수, 상수 풀, 인터페이스 정보 등)를 저장하는 영역
               - JVM이 시작될 때 클래스 로더가 클래스를 로드하면서 메서드 영역에 저장
               - 모든 스레드에서 공유하는 영역
      2️⃣ Heap Area(힙 영역)

               - 객체와 배열이 저장되는 공간으로 GC의 대상
               - GC가 사용하지 않은 객체를 자동으로 정리
               - 모든 스레드에서 공유하는 영역
      3️⃣ Stack Area(스택 영역)

               - 메서드 호출 시 생성되는 스택 프레임을 저장하는 공간
               - 각 스레드마다 독립적인 공간을 가짐
               - 메서드가 종료되면 해당 스택 프레임은 제거됨
      4️⃣ PC Register(PC 레지스터)

               - 현재 실행중인 바이트코드의 명령어 주소를 저장
               - 각 스레드마다 개별적으로 할당됨
               - 다음 실행할 명령어를 추적하는 역할
      5️⃣ Native Method Stack(네이티브 메서드 스택)

               - C, C++과 같은 네이티브 메서드 실행을 위한 스택
               - JNI를 통해 네이티브 코드를 호출할 때 사용

- Execution Engiine(실행 엔진)
    - 바이트코드를 기계어로 변환하여 실행
    - 인터프리터: 한 줄씩 바이트코드를 해석하여 실행하고 속도가 느리지만 빠르게 시작할 수 있다.
    - JIT 컴파일러: 반복적으로 실행되는 코드를 네이티브 코드로 변환하여 성능을 최적화
- Native Interface(네이티브 인터페이스, JNI)
    - java에서 C, C++ 같은 네이티브 코드를 호출할 때 사용

> JVM은 메모리를 효율적으로 관리하기 위해 GC를 사용하며, 스레드마다 독립적인 스택과 PC 레지스터를 가짐.

## JVM 주요 역할

1. 바이트 코드 실행
    - `.class` 파일(바이트코드)을 해석하고 실행
2. 메모리 관리(Garbage Collection)
    - 필요 없는 객체를 자동으로 정리하여 메모리를 효율적으로 사용
3. 플랫폼 독립성 제공
    - `JVM`이 설치된 환경이라면, `Windows`, `MAC`, `Linux` 등 어떤 `OS`에서도 실행 가능

## JVM이 실행하는 과정

1. 클래스 로더 (class Loader)
    - `.class` 파일을 메모리에 로드
2. 바이트코드 검증(Bytecode Verifier)
    - 악성 코드 여부, 바이트코드의 유효성 검사
3. 실행 엔진에서 실행
    - 인터프리터: 한 줄씩 바이트코드를 해석하여 실행
    - JIT(just-in-time) 컴파일러: 자주 실행되는 코드를 네이티브 코드로 변환하여 속도 향상

## JVM의 특징

- 플랫폼 독립성: 바이트코드는 운영체제와 무관하게 `JVM`이 설치된 환경에서 실행할 수 있다.
- 메모리 관리: `JVM`이 자동으로 GC(`가비지 컬렉션`)을 수행하여 `메모리 누수를 방지`합니다.
- 멀티스레딩 지원: `JVM`은 여러 개의 스레드를 실행할 수 있는 기능을 제공합니다.

# Java 컴파일 과정

- java의 컴파일 과정은 크게 소스 코드(`java`) → 바이트코드(`.class`) → 실행 과정으로 나뉩니다.

## 전체 과정

1. 소스 코드 작성(`.java` 파일)
2. 컴파일 (`javac` → `.class` 파일 생성)
3. 클래스 로딩(JVM이 `.class` 파일을 메모리에 로드)
4. 실행(JVM이 바이트코드를 해석 및 실행)

> java는 컴파일(`javac`) 후 `JVM`에서 추가적으로 실행(`JIT`컴파일)하여 최적화

## 추가적인 내용

### java가 플랫폼 독립적인 이유 무엇인가요?

> java는 소스 코드를 컴파일하면 바이트코드(.class)로 변환됩니다. 이 바이트코드는 OS에 종속되지 않으며,  
> JVM이 있는 환경에서는 어디서든 실행될 수 있기 때문에 플랫폼 독립성을 가진다.
