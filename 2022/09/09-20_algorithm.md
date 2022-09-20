# 2022-09-20 algorithm

## boj_2667 단지번호붙이기 - silver 1

Link : [단지번호붙이기](https://www.acmicpc.net/problem/2667)

### 정보
![](https://i.imgur.com/n440uI6.png)


### 문제
<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.

![](https://i.imgur.com/Jw8VpEa.png)

### 입력
첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.

### 출력
첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.

### 예제 입력1
![](https://i.imgur.com/vohDKck.png)


### 예제 출력1
![](https://i.imgur.com/Xxde3jj.png)


### 풀이
1. 영역을 방문하면 방문체크를 해주면서 영역의 크기를 카운트해준다.
2. 영역 방문이 끝나면 영역의 개수를 늘려주고 영역의 크기를 리스트에 저장해준다.
3. 다 방문했으면 크기들을 정렬하고 개수와 크기를 차례대로 출력해준다.

### 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    static int n;
    static int cnt = 0;
    static int[][] map;
    static boolean[][] visited;
    static int[] dx = {1, -1, 0, 0};
    static int[] dy = {0, 0, 1 ,-1};
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());

        map = new int[n][n];
        visited = new boolean[n][n];

        for (int i = 0; i < n; i++) {
            String input = br.readLine();
            for (int j = 0; j < n; j++) {
                map[i][j] = input.charAt(j)-'0';
            }
        }
        int result = 0;
        ArrayList<Integer> apart = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if(map[i][j]==1 && !visited[i][j]) {
                    dfs(i, j);
                    result++;
                    apart.add(cnt);
                    cnt=0;
                }
            }
        }
        Collections.sort(apart);

        System.out.println(result);
        for (int num : apart) {
            System.out.println(num);
        }
    }
    public static void dfs(int x, int y){
        visited[x][y] = true;
        cnt++;
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            if(nx<0 || ny<0 || nx >= n || ny >= n){ continue; }
            if(map[nx][ny]==1 && !visited[nx][ny]){
                dfs(nx, ny);
            }
        }
    }
}
```

### 결과
![](https://i.imgur.com/MDZY4Pk.png)



###### tags: `algorithm`, `Boj`, `dfs`
