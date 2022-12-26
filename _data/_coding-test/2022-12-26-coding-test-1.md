---
title: "알고리즘 문제 - 구현"
categories: "coding-test"
tags:
  - 알고리즘
  - 백준
---

# 

## 개요

구현 문제의 중점은, 생각하기는 쉬우나 이를 코드로 풀어냈을 때 어떻게 간결하게 해결할 수 있는가를 중점으로 판단합니다.

따라서 최대한 간결한 방법으로 코드를 표현하는 것이 중요하다고 생각합니다.

## 학습 문제

### (1) 단어 공부 (백준 1157번)

[1157번: 단어 공부](https://www.acmicpc.net/problem/1157)

- 나의 풀이
    
    Java 11 / 메모리 35540 KB / 시간 348 ms
    
    ```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.util.List;
    import java.util.stream.Collectors;
    
    public class Main {
        public static void main(String[] args) throws Exception {
            BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
            List<Character> data = bf.readLine().toUpperCase()  //  입력받은 단어 전부 대문자로 변경
            .chars().mapToObj(i -> (char)i)                     // String -> Character Stream 으로 변경
            .collect(Collectors.toList());                      // List<Character>로 변경
    
            int[] count = new int[26];                          // 각 알파벳 별 사용 횟수
            int maxCount = 0;                                   // 최대 알파벳 사용 횟수
            Character result = '?';                             // 가장 많이 사용된 알파벳 (중복시 ?)
    
            for (Character c : data) {
                int target = (int)(c.charValue() - 'A');        // 알파벳 인덱스 계산
                if (++count[target] > maxCount) {               // 사용 횟수 증가 이후 최대 사용 횟수와 비교
                    // 가장 많은 횟수로 나타났을 경우
                    result = c;                 // 가장 많이 사용된 알파벳 갱신
                    maxCount = count[target];   // 최대 알파벳 사용 횟수 갱신
                } else if (count[target] == maxCount) {
                    result = '?';           // 중복일 경우 ?로 변경
                }
            }
    
            System.out.println(result);
    
            return;
        }
    }
    ```
    
- 남의 풀이
    
    Java 11 / 메모리 21072 KB / 시간 284 ms
    
    ```java
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStreamReader;
    
    public class Main {
        public static void main(String[] args) throws IOException {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            String str = br.readLine().toUpperCase();
    
            // 아스키코드-> a - 65 = 0 0 -> index?
            int[] abc = new int[26];
    
            for(int i = 0; i < str.length(); i++) {
                // str.charAt(i) 의 값 - 65 -> abc의 인덱스 값
                int index = str.charAt(i) - 65;
                abc[index]++;
            }
    
            int max = 0;
            char ch = '?';
    
            for(int i = 0; i < abc.length; i++) {
                if(abc[i] > max) {
                    max = abc[i];
                    ch = (char) (i + 65);
                } else if (abc[i] == max) {
                    ch = '?';
                }
            }
            System.out.println(ch);
    
        }
    }
    ```
    

### (2) 크로스 컨트리 (백준 9017번)

[9017번: 크로스 컨트리](https://www.acmicpc.net/problem/9017)

- 나의 풀이
    
    Java 11 / 메모리 19156 KB / 시간 320 ms
    
    ```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
    import java.util.stream.Collectors;
    
    public class BJ9017 {
        public static void main(String[] args) throws Exception {
            BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
            
            Integer T = Integer.parseInt(bf.readLine());
    
            final int N_SIZE = 201;
    
            for (int testCase = 0; testCase < T; testCase++) {
                bf.readLine();  // N 입력 (무시)
                List<String> data = Arrays.stream(bf.readLine().split(" ")).collect(Collectors.toList());   // 참가한 모든 선수 처리
                List<Integer> clearData = data.stream()
                    .filter(e -> Collections.frequency(data, e) == 6)
                    .map(e -> Integer.parseInt(e)).collect(Collectors.toList());       // 6인이 참가하지 않은 인원 제거
    
                // System.out.println("clearData = " + clearData);
    
                int[] scoreTeam = new int[N_SIZE];     // 4등까지의 팀별 점수 (1 <= M <= 200)
                int[] memberCount = new int[N_SIZE];   // 들어온 인원 계산..
                int[] score5th = new int[N_SIZE];      // 5등 점수
    
                int score = 1;
                for (Integer i : clearData) {
                    if (memberCount[i] == 4) {
                        // 5번째 인원일 경우
                        score5th[i] = score;
                    } else if (memberCount[i] < 4) {
                        // 1 ~ 4번째 인원일 경우
                        scoreTeam[i] += score;    // 팀 점수 합산
                    } 
                    memberCount[i]++;           // 들어온 인원 카운트
                    score++;
                }
    
                // System.out.print("scoreTeam = ");
                // for (int i = 1; i < N_SIZE; i++) {
                //     if (scoreTeam[i] != 0) {
                //         System.out.print("(" + i + " = " + scoreTeam[i] + ")");
                //     }
                // }
                // System.out.println();
    
                // System.out.print("score5th = ");
                // for (int i = 1; i < N_SIZE; i++) {
                //     if (scoreTeam[i] != 0) {
                //         System.out.print("(" + i + " = " + score5th[i] + ")");
                //     }
                // }
                // System.out.println();
    
                Integer minScore = Integer.MAX_VALUE; // 팀 최소 점수
                Integer winnerTeam = 0;               // 우승 팀 번호
    
                for (int i = 1; i < N_SIZE; i++) {
                    if ((scoreTeam[i] != 0) && (scoreTeam[i] < minScore)) {
                        // 최소 점수 팀을 찾을 경우
                        minScore = scoreTeam[i];
                        winnerTeam = i;
                    } else if ((scoreTeam[i] != 0) && (scoreTeam[i] == minScore) && (score5th[i] < score5th[winnerTeam])) {
                        // 최소 점수와 같고, 5th 멤버가 더 빨리 들어왔을 경우
                        winnerTeam = i;
                    }
                }
    
                System.out.println(winnerTeam);
            }
    
            return;
        }
    }
    ```
    
- 남의 풀이
    
    Java 8 / 메모리 13640 KB / 시간 112 ms
    
    ```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.StringTokenizer;
    
    public class Main {
    	
    	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	static StringTokenizer st;
    	static StringBuilder sb = new StringBuilder();
    	
    	static int T,N,rank,max,ans,minSum,val;
    	static int[] score;
    	static int[] info;
    	static int[] cnt;
    	static boolean[] accepted;
    	static int[] five; //다섯번째
    	
    	public static void main(String[] args) throws Exception {
    		T = Integer.parseInt(br.readLine());
    		
    		while(T-- > 0) {
    			N = Integer.parseInt(br.readLine());
    			
    			st = new StringTokenizer(br.readLine());
    			accepted = new boolean[201];
    			score = new int[201];
    			info = new int[N+1];
    			cnt = new int[201];
    			five = new int[201];
    			
    			rank =1;
    			minSum = Integer.MAX_VALUE;
    			ans=1;
    			Arrays.fill(accepted, true);
    			
    			for(int i=1;i<=N;++i) {
    				int num = Integer.parseInt(st.nextToken());
    				info[i] = num;
    				cnt[num]++;
    				max = Math.max(max, num);
    			}
    			
    			for(int i=1;i<=max;++i) {
    				if(cnt[i] < 6) accepted[i] =false;
    			}
    			
    			Arrays.fill(cnt, 0);
    			//등수정보 갱신해서 점수 계산
    			for(int i=1;i<=N;++i) {
    				if(!accepted[info[i]]) continue;
    				
    				if(cnt[info[i]] < 4) {
    					score[info[i]] += rank;
    					cnt[info[i]]++;
    				} else if(cnt[info[i]] == 4) {
    					five[info[i]] = rank;
    					cnt[info[i]]++;
    				}
    				rank++;
    			}
    
    			for(int i=1;i<=max;++i) {
    				if(!accepted[i]) continue;
    				if(score[i] < minSum) {
    					minSum = score[i];
    					ans = i;
    				} else if(score[i] == minSum) {
    					if(five[i] < five[ans]) {
    						ans = i;
    					}
    				}
    			}
    			
    			sb.append(ans+"\n");	
    		}
    		
    		System.out.println(sb);
    	}
    }
    ```

## 알게된 점

### 1. `stream()`의 무분별한 사용

알고리즘 문제에서는 한 줄에 여러 값을 입력받는 경우가 많았는데, `stream()`으로 불필요하게 연산하여 시간과 메모리를 과하게 사용한다는 느낌이 있었습니다.

다음 문제 풀이 때는 직접 컨트롤하여 시간의 이점을 가져보도록 하겠습니다.

### 2. `Collections.frequency(Collection<?> c, Object o)`의 활용

컬렉션 내에서 원소의 갯수를 파악할 때 사용할 수 있는 `frequency()`함수를 알고난 덕분에, 원소의 갯수를 세는데 더욱 수월했습니다.  

### 3. `char[]`는 `Arrays.stream()`으로 사용할 수 없다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39c2b2dc-7bf4-407d-b747-12d3cb29486c/Untitled.png)

`Arrays.stream()`을 사용해서 Stream으로 반환시켜서 많이 사용하는데, 원시 숫자 타입과 클래스 타입은 되는데 `char[]`는 바로 Stream으로 변환할 수 없었습니다.

그래서 컬렉션 타입으로 변환하기 위래 아래와 같은 코드를 사용했습니다.

```java
List<Character> data = bf.readLine().toUpperCase()  //  입력받은 단어 전부 대문자로 변경
        .chars().mapToObj(i -> (char)i)                     // String -> Character Stream 으로 변경
        .collect(Collectors.toList());                      // List<Character>로 변경
```