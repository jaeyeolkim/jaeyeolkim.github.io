## 중간 처리와 최종 처리
* 스트림은 중간 처리에서는 매핑, 필터링, 정렬을 수행하고
* 최종 처리에서는 반복, 카운팅, 평균, 총합 등의 집계 처리를 수행한다.
* 예제 소스
```java
import java.util.Arrays;
import java.util.List;

public class MapAndReduceExample {
    public static void main(String[] args) {
        List<Student> studentList = Arrays.asList(
                new Student("kim", 10),
                new Student("lee", 20),
                new Student("park", 30)
        );

        double avg = studentList.stream()
                .mapToInt(Student::getScore) //중간처리(학생 객체를 점수로 매핑)
                .average() //최종처리(평균 점수)
                .getAsDouble();

        System.out.println("평균 점수: " + avg);
    }
}
```
* 결과
```
평균 점수: 20.0
```
