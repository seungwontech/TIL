# 깊이 우선 탐색(DFS)
- 그래프나 트리 자료구조를 탐색하는 방법 중 하나로, 가능한 한 깊이 내려가면서 탐색을 진행한 뒤, 더 이상 갈 수 없을 때 이전 단계로 되돌아가는 방식이다.
- 시작 노드에서 출발해서 탐색할 한쪽 분기를 정하여 최대까지 탐색을 마친 후 다른 쪽 분기를 이동하여 다시 탐색을 수행하는 알고리즘이다.
- 특징으로 `재귀함수`로 구현하고 `스택`자료구조를 이용합니다. 재귀함수를 이용해서 스택 오버플로에 유의한다.

## 핵심 이론
- 한번 방문한 노드를 다시 방문하면 않고 노드 방문 여부를 체크할 배열 필요
- 그래프는 인접 리스트로 표현
- DFS의 탐색 방식은 후입선출 특성

# 너비 우선 탐색(BFS)
- 그래프를 완전 탐색하는 방법 중 하나로, 시작 노드에서 출발해 시작 노드를 기준으로 가까운 노드를 먼저 방문하면서 탐색하는 알고리즘이다.
- 선입선출 방식으로 탐색해 큐를 이용해 구현한다.
- 시작 노드와 가까운 노드를 우선하여 탐색해 목표 노드에 도착하는 경로가 여러 개일 때 최단 경로를 보장합니다.

## 핵심 이론
- 방문했던 노드는 다시 방문하지 않고 방문한 노드를 체크하기 위한 배열이 필요
- 그래프는 DFS와 동일
- `차이점` 탐색을 위해 `스택`이 아닌 `큐`를 사용.

## DFS와 BFS시간 복잡도
- O(V+E): 노트(V)와 간선(E)을 모두 탐색한다.
- V는 노드의 수, E는 간선의 수이다.

## 백준 예제
https://www.acmicpc.net/problem/1260
```java
package BaekJoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class ex1260 {
    static int N, M, V;
    static int[][] arr;
    static boolean[] visit;
    static StringBuilder text = new StringBuilder();
    static Queue<Integer> q = new LinkedList<>();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        V = Integer.parseInt(st.nextToken());

        arr = new int[N + 1][N + 1];
        visit = new boolean[N + 1];
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            arr[a][b] = 1;
            arr[b][a] = 1;
        }
        DFS(V);
        text.append("\n");
        visit = new boolean[N + 1];
        BFS(V);
        System.out.println(text);
    }

    private static void DFS(int v) {
        visit[v] = true;
        text.append(v + " ");
        for (int i = 0; i <= N; i++) {
            if (arr[v][i] == 1 && !visit[i]) {
                DFS(i);
            }
        }
    }

    private static void BFS(int v) {
        q.add(v);
        visit[v] = true;
        while (!q.isEmpty()) {
            v = q.poll();
            text.append(v + " ");
            for (int i = 1; i <= N; i++) {
                if (arr[v][i] == 1 && !visit[i]) {
                    q.add(i);
                    visit[i] = true;
                }
            }
        }
    }
}
```

## 참고자료
https://www.youtube.com/watch?v=Cs8DaupoSPM  
https://www.youtube.com/watch?v=lFtQnOhKnr0