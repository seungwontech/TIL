# HashMap

- `null` key 와 `null` value 허용 ⭕️
- 동기화 없음 (멀티스레드 환경에서 안전하지 않음)
- key-value 기반의 데이터 저장(해시 기반)
- 단일 스레드 환경에서 빠르게 작동

    ```java
    public class HashMapExample {
        public static void main(String[] args) {
            Map<Integer, String> map = new HashMap<>();
            map.put(1, "Apple");
            map.put(2, "Banana");
            map.put(null, "Null Key");
            System.out.println(map); // {1=Apple, 2=Banana, null=Null Key}
        }
    }
    ```

# HashTable

- 동기화 있음 (멀티스레드 환경에서 사용 가능)
- `null` key 와 `null` value 허용 ❌
- 동기화 처리가 많아 성능이 상대적으로 느림
    ```java
    public class HashtableExample {
        public static void main(String[] args) {
            Map<Integer, String> table = new Hashtable<>();
            table.put(1, "Dog");
            table.put(2, "Cat");
            // table.put(null, "Null Key"); // NullPointerException 발생!
            System.out.println(table); // {1=Dog, 2=Cat}
        }
    }
    ```

# ConcurrentHashMap

- 멀티스레드 환경에서 안전
- `null`key와 `null` value 허용 ❌
- 성능 향상을 위해 락을 세분화 (Segmented Locking) 사용
- `Hashtable`보다 빠르고, `HashMap`보다 안전

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.Map;

public class ConcurrentHashMapExample {
    public static void main(String[] args) {
        Map<Integer, String> concurrentMap = new ConcurrentHashMap<>();
        concurrentMap.put(1, "Red");
        concurrentMap.put(2, "Blue");
        // concurrentMap.put(null, "Null Key"); // ❌ NullPointerException 발생!
        System.out.println(concurrentMap); // {1=Red, 2=Blue}
    }
}
```

# `HashMap`과 `ConcurrentHashMap` 차이점

| **특징**       | **HashMap**                         | **ConcurrentHashMap**                   |
|--------------|-------------------------------------|-----------------------------------------|
| **동기화**      | 동기화되지 않음 (Thread-safe 아님)           | 동기화됨 (Thread-safe 지원)                   |
| **성능**       | 동기화가 없으므로 더 빠름                      | 동기화로 인해 성능이 다소 떨어짐                      |
| **Null 키/값** | 하나의 Null 키와 여러 Null 값을 허용           | Null 키/값을 허용하지 않음                       |
| **멀티스레드 환경** | 멀티스레드 환경에서 안전하지 않음                  | 멀티스레드 환경에서도 안전하게 동작                     |
| **작동 원리**    | 내부적으로 하나의 락을 사용하여 전체 맵을 잠금          | 여러 세그먼트를 사용하여 동시 접근을 제어 (세그먼트 수준에서 동기화) |
| **사용 용도**    | 단일 스레드 환경에서 빠르게 동작하거나 동기화가 필요 없는 경우 | 멀티스레드 환경에서 효율적인 병렬 처리가 필요한 경우           |

```java
package CS;

import java.util.HashMap;
import java.util.concurrent.ConcurrentHashMap;

public class HashMapVsConcurrentHashMap {
    public static void main(String[] args) throws InterruptedException {
        HashMap<Integer, String> hashMap = new HashMap<>();
        ConcurrentHashMap<Integer, String> concurrentMap = new ConcurrentHashMap<>();

        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                hashMap.put(i, "Value" + i);
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                hashMap.put(i, "Value" + i);
            }
        });

        Thread thread3 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                concurrentMap.put(i, "Value" + i);
            }
        });

        Thread thread4 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                concurrentMap.put(i, "Value" + i);
            }
        });

        thread1.start();
        thread2.start();
        thread3.start();
        thread4.start();

        thread1.join();
        thread2.join();
        thread3.join();
        thread4.join();

        System.out.println("HashMap Size: " + hashMap.size()); // 데이터 손실 가능성 있음
        System.out.println("ConcurrentHashMap Size: " + concurrentMap.size()); // 정확한 크기 보장
    }
}
```

> 999 :Value999  
> 1000 :null  
> 1001 :null  
> 1002 :null  
> 1003 :null  
> HashMap Size: 1004  
> ConcurrentHashMap Size: 1000

- 멀티스레드 환경에서는 `ConcurrentHashMap`을 사용해야 안전하다