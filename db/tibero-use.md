# Tibero 이관 및 프로젝트 적용
> oracle -> tibero6, tbAdmin, TiberoStudio2

## oracle -> tibero6 DB 이관하기
* T-Up 이라는 툴을 이용한다.
  - 참고로 tibero5 버전에서는 tbMigrator 를 사용했었다.
  - tmax technet 에 로그인 후 다운로드 > 데이터베이스 > Tibero6 > 다운로드 > [tibero6-XXX.tar.gz](https://technet.tmaxsoft.com/ko/front/download/viewDownload.do?cmProductCode=0301&version_seq=PVER-20150504-000001&doc_type_cd=DN#binary)을 다운로드 한다.
  - 압축을 풀고 tibero6/client/bin/T-Up.zip 파일의 압축을 다시 푼다.
  - 오라클 접속을 위해 T-Up/lib 경로에 ojdbc14.jar 파일을 복사한다.
  - T-Up.x86_64.bat 파일을 실행하면 된다.
  - 각 유저에 DBA(with admin option) 권한을 부여해야 이관이 가능하다.
* 테이블스페이스, 유저(스키마 - tibero는 유저와 스키마가 동일한 개념이다)는 이관시 생성을 시도한다.
* tibero에 이미 생성한 경우에도 진행에는 문제 없다. 에러 로그만 남을 뿐.
* tibero는 temporary table에 index 생성이 불가하다.

## tbAdmin
* tibero5 버전에서도 사용했던 db 접속 관리 툴이다.
* 이클립스 기반으로 다소 무겁지만 테이블스페이스 생성등의 관리자 작업이 가능하다.
* tmax technet 다운로드 > 데이터베이스 > Tibero > [Tibero Admin Tool](https://technet.tmaxsoft.com/ko/front/download/viewDownload.do?cmProductCode=0301&version_seq=PVER-20170425-000001&doc_type_cd=DN) 에서 다운로드 할 수 있다.
* 압축을 풀고 tbAdmin6.exe 를 실행하여 사용하면 된다.

## TiberoStudio2
* tibero5 버전 사용했을때에는 없던 툴이다.
* 동일하게 이클립스 기반이지만 tbAdmin보다 좀 가볍고 개발자용으로 볼 수 있다.
* F6 키로 조회하면 데이터 편집까지 가능하다.(tbAdmin은 안됨)
* procedure 등의 작업도 가능하므로 tbAdmin 보다는 이 툴의 사용을 권장한다.
* tmax technet 다운로드 > 데이터베이스 > Tibero > [Tibero Studio](https://technet.tmaxsoft.com/ko/front/download/viewDownload.do?cmProductCode=0301&version_seq=PVER-20140109-000004&doc_type_cd=DN#binary) 에서 다운로드 할 수 있다.
* 압축을 풀고 TiberoStudio.exe 를 실행하여 사용하면 된다.

## 프로젝트 적용하기
* application.properties
```
db.driverClassName=com.tmax.tibero.jdbc.TbDriver
db.url=jdbc:tibero:thin:@ip:port:sid
```

* pom.xml
  - 먼저 tibero6-jdbc.jar 파일을 WEB-INF/lib 경로에 복사.
```xml
<dependency>
    <groupId>solutis</groupId>
    <artifactId>tibero6</artifactId>
    <version>6</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/src/main/webapp/WEB-INF/lib/tibero6-jdbc.jar</systemPath>
</dependency>
```
