## favicon.ico


### favicon
![favicon](favicon_chrome.PNG)

```java
http://localhost:8090/favicon.ico 404 (Not Found)
```
- 개발자 도구를 보면 위와 같은 오류가 발생하는 경우가 있습니다.
- favicon은 브라우저 주소창에 표시되는 사이트의 대표 아이콘입니다.
- 브라우저가 이를 표현하기 위해 /favicon.ico 를 호출하는데 파일을 못 찾아서 나는 오류이죠

### spring mvc favicon 설정
* favicon.ico 파일을 만들어서 프로젝트 webapp 위치에 추가합니다.
  * 이미지를 ico 파일로 생성해주는 사이트들이 있습니다
* dispatcher-servlet.xml 수정
```xml
<mvc:resources mapping="/favicon.ico" location="/favicon.ico"/>

...
//불필요한 인터셉터를 타지 않도록 기존에 <mvc:exclude-mapping path="/static/**"/> 이 선언된 인터셉터마다 추가
<mvc:interceptor>
  <mvc:exclude-mapping path="/favicon.ico"/>
  <mvc:exclude-mapping path="/static/**"/>
</mvc:interceptor>
```
