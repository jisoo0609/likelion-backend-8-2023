# Brute Force

- 무작정 가능한 모든 경우를 다 검사하는 알고리즘
- 경우의 수가 상대적으로 작을 때 유용
- 경우의 수가 늘어나기 시작하면 수행 속도 느려지기 시작함

---

# **Permutation (**순열)

- 서로 다른 n개의 것들 중 r개를 골라 나열하는 방법 → 순서 유의미함
- $nPr$
- 주어진 요소들의 순서를 정하고 해당 순서의 비용이 최적인지 판단하는 알고리즘
- 문제점
    - n!은 n이 커지면 그 값이 폭발적으로 증가하게 됨

### for문을 이용한 순열 만들기

```java
public static void permSimple(int n) {
        int first;
        int second;
        int third;

        // 0 ~ n 사이의 숫자를 차례대로 골라본다
        for (int i = 0; i < n; i++) {
            // 첫번째 숫자를 골랐음
            first = i;
            // 0에서 n 사이의 숫자 중, 아직 고르지 않은 숫자를 골라본다
            for (int j = 0; j < n; j++) {
                // 이미 고른 숫자라면 나머지는 실행하지 않는다
                if (j == first) {
                    continue;
                }
                second = j;
                // 0에서 n 사이 숫자 중, 아직 고르지 않은 숫자를 골라본다
                for (int k = 0; k < n; k++) {
                    if (k == first || k == second) {
                        continue;
                    }
                    third = k;
                    System.out.println(first + " " + second + " "+ third);
                }
            }
        }
    }
```

### 재귀함수를 이용한 순열 만들기

```java
    // int n, r : 순열을 구할때 필요한 것. 고르는 대상, 고르는 갯수
    // int depth : 지금 몇번째 원소를 고르고 있는지
    // boolen[] used : 어떤 요소들을 사용했는지 저장하는 배열
    // int[] perm : 결과를 저장하기 위한 배열
    public static void permRecur(int n, int r, int depth, boolean[] used, int[] perm) {
        // 내가 고른것의 개수가 고를것의 개수와 같아지면 중단
        if (depth == r) {

        }
        // n개의 원소 중 하나를 선택하는 for
        for (int i = 0; i < n; i++) {
            // 이미 선택했다면 스킵
            if (used[i]) continue;
            // 선택을 할때 first = i의 형태로 작성했던 부분
            perm[depth] = 1;
            // 내가 이번에 i를 선택했다는 것을 기록
            used[i] = true;
            // 중첩된 for 대신 재귀 호출
            permRecur(n, r, depth + 1, used, perm);
            // 이 i에서 출발하는 순열을 다 찾으면, 다음 i를 쓰기 위해 기록 변경
            used[i] = false;
        }

    }
```

```java
// 재귀함수로 더 많은 원소를 선택하는 순열을 만들어보자.
    public static void permRecurHelper(
            // 순열을 구할때 필요한거: 고르는 대상, 고르는 갯수
            int n, int r,
            // 내가 지금 몇번째 원소를 고르고 있는지
            int depth,
            // 어떤 요소들을 사용했는지 저장하는 배열
            boolean[] used,
            // 결과를 저장하기 위한 배열
            int[] perm
    ) {
        // 내가 고른것의 갯수가 고를것의 갯수와 같아지면 중단.
        if (depth == r) {
            System.out.println(Arrays.toString(perm));
        }
        else {
            // n개의 원소중 하나를 선택하는 for
            for (int i = 0; i < n; i++) {
                // 이미 선택했다면 스킵
                if (used[i]) continue;

                // 선택을 할때 first = i;의 형태로 작성했던 부분
                perm[depth] = i;
                // 내가 이번에 i를 선택했다는걸 기록
                used[i] = true;
                // 중첩된 for 대신 재귀 호출
                permRecurHelper(n, r, depth + 1, used, perm);
                // 이 i에서 출발하는 순열을 다 찾으면, 다음 i를 쓰기위해 기록 변경
                used[i] = false;
            }
        }
    }

    // 사용성을 위해 실제 메서드를 분리한다. (n과 r만 있어도 실행이 되게끔)
    public static void permRecursive(int n, int r) {
        permRecurHelper(n, r, 0, new boolean[n], new int[r]);
    }
```

---

# Combination (조합)

- 서로 다른 n개의 것들 중 r개를 순서 상관없이 고르는 방법
- $nCr$
- $nCr = n-1Cr + n-1Cr-1$

### for문을 이용한 조합 만들기

```java
public static void combSimple(int n) {
        int first;
        int second;
        int third;
        // i는 0 부터 n - 2까지 반복하고,
        for (int i = 0; i < n - 2; i++) {
            first = i;
            // j는 i + 1 부터 n - 1까지 반복하고,
            for (int j = first; j < n -1; j++) {
                second = j;
                // k는 j + 1 부터 n - 1까지 반복한다.
                for (int k = 0; k < n; k++) {
                    third = k;
                    System.out.println(first + " "+ second + " "+ third);
                }
            }
        }
    }
```

### 재귀함수를 이용한 조합 만들기

```java
// 재귀함수로 nCr 구하는 메서드
    // n개 중에서 r개 고름
    // int count: 지금까지 몇개의 원소를 골랐는지
    // int next: 이번에 고를지 말지를 판단중인 숫자
    // int[] comb: 조합 결과를 담을 배열
    public static void combRecurHelper(int n, int r, int count, int next, int[] comb) {
        // r개 골라야 하는데 다 골랐다
        if (count == r) {
            System.out.println(Arrays.toString(comb));
        }
        // 만약 내가 고르려고 고려할 숫자가 범위를 벗어나려 한다면
        else if (next == n) {
            return;
        }else {
            // 만약 내가 이번에 next를 골랐으면
            comb[count] = next;
            // count 번째 원소를 골랐으니, count + 1번째 원소를 고르러 감
            combRecurHelper(n, r, count + 1, next + 1, comb);

            // count 번째 원소를 고르지 않고 다음 원소를 확인한다
            combRecurHelper(n, r, count, next + 1, comb);
        }
    }

    public static void combRecur(int n, int r) {
        combRecurHelper(n, r, 0, 0, new int[r]);
    }
```