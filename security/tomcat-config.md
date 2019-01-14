## Tomcat 보안 환경 설정

### 1. %TOMCAT_HOME% 하위에 사용하지 않는 폴더 삭제
- /examples/
- /docs/
- /manager/
  - manager 는 사용할 경우 경로를 변경할 것
  
### 2. 메소드 허용
- 허용 메소드 확인
  - https://curl.haxx.se/download.html 에서 curl 다운로드하여 압축을 풀면 된다.
```
C:\curl-7.63.0-win64-mingw\bin>curl -I -X OPTIONS http://localhost:8080/app/

HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Allow: GET, HEAD, POST, PUT, DELETE, OPTIONS
Content-Length: 0
Date: Mon, 14 Jan 2019 06:24:28 GMT
```

- 수정
  - %TOMCAT_HOME%/conf/web.xml
  - 제일 하단에 아래 내용 추가
```
<!-- GET, POST, HEAD 외 불필요한 메소드 제거 -->
<security-constraint>
    <web-resource-collection>
        <web-resource-name>Forbidden</web-resource-name>
        <url-pattern>/*</url-pattern>
        <http-method>PUT</http-method>
        <http-method>DELETE</http-method>
        <http-method>PATCH</http-method>
        <http-method>TRACE</http-method>
        <http-method>OPTIONS</http-method>
    </web-resource-collection>
    <auth-constraint />
</security-constraint>
```

- 허용 메소드 재확인(재부팅후)
```
HTTP/1.1 403 Forbidden
Server: Apache-Coyote/1.1
Pragma: No-cache
Cache-Control: no-cache
Expires: Thu, 01 Jan 1970 09:00:00 KST
Set-Cookie: JSESSIONID=80C64B9F89E0202304ADBEF5C5684B15; Path=/; HttpOnly
Content-Type: text/html;charset=utf-8
Transfer-Encoding: chunked
Date: Mon, 14 Jan 2019 06:26:50 GMT
```

### 3. 톰캣 HttpOnly 설정하기
- %TOMCAT_HOME%/conf/context.xml
```
<Context useHttpOnly="true">
```
