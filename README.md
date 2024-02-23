# 8. Two Pointers

# 1. Two Pointers

## 1-1. 이론

말 그대로, 두 개의 포인터를 사용한다는 뜻이며, C/C++ 에서의 포인터 문법은 아니다.

N 개의 수로 된 배열이 있다. 이 배열의 i 부터 j 까지의 수의 합이 5가 되는 경우를 구하라.

단순하게 이중루프로 구현할수도 있지만, 좀 더 지혜롭게 풀어보자.

`start` = 0, `end` = 0 으로 둔다.

tar = 5

| idx | S = E = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

`start == end` 일 경우, 합은 0 이라고 본다.

이 경우엔, `end++`

| idx | S = 0 | E = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

여기서 `sum` 은, `nums[start] + nums[end - 1]` 이다.

`sum` : 1

`sum < tar` 일 경우에도, `end++`

| idx | S = 0 | 1 | E = 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

`sum` : 3

`sum < tar` ⇒ `end++`

| idx | S = 0 | 1 | 2 | E = 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

`sum` : 6

`sum > tar` 일 경우엔, `start++`

| idx | 0 | S = 1 | 2 | E = 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

 `sum` : 5 ⇒ 정답!

이 때, `cnt++` 해주고,

`sum == tar` 일 경우에도, `start++`

이런식으로 진행하다보면…

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | S = E = 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |

이 상황이 오게 되는데, 맨 마지막 값이 5일수도 있으므로 `end` 는 한번 더 가줘야 한다.

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | S = 9 | E = 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |  |

`sum` : 2

`sum < tar` ⇒ `end++` 이어야 하지만, 더 이상 가면 안되므로 `start++`

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | S = E = 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 1 | 2 | 3 | 4 | 2 | 5 | 3 | 1 | 1 | 2 |  |

이렇게, `start == N` 의 상황이 오게 된다.

이때, 루프를 탈출한다.

`cnt` : 3

- 지금은 두 개의 포인터가 오른쪽으로만 이동한다.
- 유형에 따라서, 포인터 2개가 같은 방향으로 진행할수도, 양끝에서 시작해 반대로 진행할수도, 하나는 한쪽으로만 진행하고, 다른 포인터는 양쪽으로 진행할수도 있다. 문제 상황에 따라서, 머리를 잘 써야한다.

## 1-2. 구현

```cpp
#include <iostream>
using namespace std;

int main()
{
    int nums[10] = { 1, 2, 3, 4, 2, 5, 3, 1, 1, 2 };
    int size = 10;
    int tar = 5;

    int cnt = 0;
    int sum = 0;
    int start = 0;
    int end = 0;
    while (start < size)
    {
        if (sum >= tar || end == size)
        {
            sum -= nums[start];
            start++;
        }
        else
        {
            sum += nums[end];
            end++;
        }
        if (sum == tar)
            cnt++;
    }

    cout << cnt;
}
```

## 1-3. BOJ 2470 - 두 용액

[https://www.acmicpc.net/problem/2470](https://www.acmicpc.net/problem/2470)

배열에서 두 개의 수를 합해 0 에 근접한 값 두 개를 "아무거나" 리턴하면 됨.

즉, 제일 빨리 찾는 거 리턴하면 된다. 

투포인터 문제이지만, 두 개의 포인터가 양쪽 끝에서 모아진다.

일단, 정렬부터 시킨다.

왜 양쪽일까? 당연한데,

왼쪽 끝 + 왼쪽 끝 ⇒ 너무 작음.

오른쪽 끝 + 오른쪽 끝 ⇒ 너무 큼.

즉, 둘 다 0 에 근접하려면 한참 걸린다.

가장 합리적인 생각은, 양쪽 끝에 포인터를 두고, 상황에 따라 옮겨가는 것.

1. 합쳤을 때 0 보다 작다 ⇒ 값이 커져야 하니깐 `start++`
2. 합쳤을 때 0 보다 크다 ⇒ 값이 작아져 야하니깐 `end++`

두 포인터를 옮기기 전에, `min` 갱신하되, 근접하는 걸 구해야 하니깐 `abs()` 사용

탈출 조건

- 포인터가 같은 인덱스를 가리킬 때 (두 용액이 아니라 하나의 용액이 되어버리니깐)
- 합이 `0` 이 되었을 때

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <algorithm>
using namespace std;

int arr[100000];
int ans[2];
int main()
{
    // freopen("sample_input.txt", "r", stdin);

    int N;
    cin >> N;
    for (int i = 0; i < N; i++)
    {
        cin >> arr[i];
    }
    sort(arr, arr + N); // 정렬 필수!

    int start = 0;        // start 은 왼쪽 끝에서
    int end = N - 1;      // end 은 오른쪽 끝에서 시작
    int min = 2000000000; // 현재까지 0 에 가장 가까웠던 합
    while (start < end)
    {
        int sum = arr[start] + arr[end];

        if (min > abs(sum))
        {
            min = abs(sum);
            ans[0] = arr[start];
            ans[1] = arr[end];

            if (sum == 0)
                break;
        }

        if (sum < 0)
            start++;
        else
            end--;
    }

    cout << ans[0] << " " << ans[1] << '\n';
    return 0;
}
```

# 2. Sliding Window

## 2-1. 이론

크기가 고정된 창문을 왼쪽에서 오른쪽으로 민다고 생각하자. 창문의 길이는 고정이다.

![Untitled](8%20Two%20Pointers%20e931bd71526a4bafa960966d7829e474/Untitled.png)

![Untitled](8%20Two%20Pointers%20e931bd71526a4bafa960966d7829e474/Untitled%201.png)

![Untitled](8%20Two%20Pointers%20e931bd71526a4bafa960966d7829e474/Untitled%202.png)

투포인터와 다른 점은, 원포인터만 있어도 된다는 것이다. 어차피 창문의 길이는 고정이기 때문이다.

만약 배열이 주어지고, 특정한 구간의 합을 구한다면, 옮겨가면서 부분합을 구할 수 있을 것이다. 즉, 이중루프를 사용하면 된다.

그런데, 이중루프는 사용할 필요 없다. 다음 그림을 보자.

![Untitled](8%20Two%20Pointers%20e931bd71526a4bafa960966d7829e474/Untitled%203.png)

![Untitled](8%20Two%20Pointers%20e931bd71526a4bafa960966d7829e474/Untitled%204.png)

이 부분은 겹치는 부분이다. 따라서, 앞의 걸 삭제하고, 그 다음 걸 추가하면 된다.

문제: N 개의 수로 된 배열이 있다. 이 배열에서, 연속된 총 K 개의 수를 더했을 때 최대합을 구하라.

## 2-2. 구현

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <algorithm>

using namespace std;

int main()
{
    int arr[10] = { 1, 2, 3, 4, 2, 5, 3, 1, 1, 2 };
    int size = 10;
    int K = 3;

    int maxVal = 0;
    int sum = 0;

    // 최초엔 다 더함
    for (int i = 0; i < K; i++)
    {
        sum += arr[i];
    }

    maxVal = sum;

    for (int i = K; i < size; i++)
    {
        // 새로운 끝 인덱스
        sum += arr[i];
        // 기존의 첫 인덱스
        sum -= arr[i - K];
        maxVal = max(maxVal, sum);
    }

    cout << maxVal << '\n';
    return 0;
}
```