# 2022-09-21 algorithm

## programmers k진수에서 소수 개수 구하기 - level 2

Link : [k진수에서 소수 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

### 문제 설명

양의 정수 **n**이 주어집니다. 이 숫자를 **k**진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

* 0P0처럼 소수 양쪽에 0이 있는 경우
* P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
* 0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
* P처럼 소수 양쪽에 아무것도 없는 경우
* 단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
    * 예를 들어, 101은 P가 될 수 없습니다.

예를 들어, 437674을 3진수로 바꾸면 **211**0**2**01010**11**입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 **k**진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 **P0** 형태에서 찾을 수 있으며, 2는 **0P0**에서, 11은 **0P**에서 찾을 수 있습니다.

정수 **n**과 **k**가 매개변수로 주어집니다. **n**을 **k**진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.

### 제한사항

* 1 ≤ n ≤ 1,000,000
* 3 ≤ k ≤ 10

### 입출력 예

| n | k | result |
| -------- | -------- | -------- |
| 437674 | 3 | 3 |
| 110011 | 10 | 2 |

### 풀이

1. Integer.toString() 메소드를 이용하여 k진법으로 바꾼다.
2. split() 메소드로 0을 기준으로 나눠준다.
3. 소수 판별 메소드를 만들어서 숫자들을 소수인지 판별하고 개수를 세준다.

### 코드
```java
class Solution {
    public int solution(int n, int k) {
        int answer = 0;
        String notation = Integer.toString(n, k);
        String[] nums = notation.split("0+");    
        // 0을 기준으로 나누는데 +를 붙여서 1개 이상만 분리해준다.

        for (String num : nums) {
            if(checkprime(Long.parseLong(num))){
                answer++;
            }
        }
        return answer;
    }
    public static boolean checkprime(long num){
        if (num == 2) { return true; }    // 2일때는 소수이므로 true
        if (num == 1 || num % 2 == 0) { return false; }    // 1과 짝수는 false

        for (long i = 3; i <= (long)Math.sqrt(num); i += 2) {
            if (num % i == 0) { return false;}    // 나눠지면 false
        }
        return true;
    }
}
```

### 결과

![](https://i.imgur.com/v1T8EjR.png)

###### tags: `programmers`
