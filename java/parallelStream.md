## parallelStream 병렬처리
* 병렬 처리 스트림은 main 스레드를 포함해서 스레드풀의 작업 스레드들이 병렬적으로 요소를 처리한다.
* 예제 소스
```java
import java.util.Arrays;
import java.util.List;

public class ParallelExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("1", "2", "3", "4", "5");

        //순차 처리
        list.stream().forEach(s -> ParallelExample.print(s));
        System.out.println("==============================");

        //병렬 처리
        list.parallelStream().forEach(s -> ParallelExample.print(s));
    }

    public static void print(String str){
        System.out.println(str + ":" + Thread.currentThread().getName());
    }
}
```
* 결과
```
1:main
2:main
3:main
4:main
5:main
==============================
3:main
5:ForkJoinPool.commonPool-worker-2
2:ForkJoinPool.commonPool-worker-1
1:main
4:ForkJoinPool.commonPool-worker-3
```
