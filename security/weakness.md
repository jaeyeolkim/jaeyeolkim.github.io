# 보안 취약점 대응
> EPD 취약점 처리 과정 중 공유가 필요한 내용들만 요약 정리 하였습니다.
<br/>

## A. 위험도 높음
### 1.부적절한 자원 해제 (IO)
* FileInputStream, FileOutputStream 등의 자원을 사용하는 경우 반드시 finally 에서 close 되도록 한다.
* try 에서 해제하는 경우 검출됨.

### 2. 경로 조작 및 자원 삽입
* 검증되지 않은 외부 입력값을 통해 파일 및 서버 등 시스템 자원에 접근하면 안됨
* doXssFilter 메소드를 적용하여준다.
```java
String fileName = request.getParameter("append_file_nm");
//필터 적용
fileName = doXssFilter(fileName);
```   
<br/>

## B. 위험도 보통
### 1. 오류 상황 대응 부재
* catch 구문이 비어있을 경우 검출된다.
```java
// 1) SolutisStringUtil.java 에 아래 메소드 추가
public static String getSolutisErrorMsg(Exception exception){
    return null2string(exception);
}

// 2) getSolutisErrorMsg(e) 사용하여 로그를 찍어준다.
} catch (Exception e) {
    LOGGER.error(getSolutisErrorMsg(e));
}
```

### 2. 오류 상황 미수신
* 예외를 발생시키면 반드시 처리해야 합니다.
* 아래의 경우 BizException 이 throw 되는 경우에 대한 대응 부재
```java
public void saveResult(Map<String, Object> paramMap) {
    ...
	if("ERROR".equals(paramMap.get("MESSAGE"))){
		//Exception 처리.
		throw new BizException(messageManager.getMessage("MSG_ERR_ADMIN_CONTACT", new Object[]{messageManager.getMessage("26")})); //{0} 처리 중 오류가 발생하였습니다.관리자에게 문의 하시기 바랍니다.
	}
}
```
* throws BizException 을 추가하여준다.
```java
public void saveResult(Map<String, Object> paramMap) throws BizException{
    ...
	if("ERROR".equals(paramMap.get("MESSAGE"))){
		//Exception 처리.
		throw new BizException(messageManager.getMessage("MSG_ERR_ADMIN_CONTACT", new Object[]{messageManager.getMessage("26")})); //{0} 처리 중 오류가 발생하였습니다.관리자에게 문의 하시기 바랍니다.
	}
}
```

### 3. 오류메시지를 통한 정보노출
* catch 구문안에서 e.getMessage(), e.toString() 사용하면 검출 됨
* getSolutisErrorMsg(e) 메소드를 사용한다.

### 4. 중요한 자원에 대한 잘못된 권한 설정 (File)
* 디렉토리/파일 생성시 접근 권한에 대한 설정이 없거나 잘못되어 있을 경우 탐지합니다.
```java
// 1) SolutisStringUtil.java 에 아래 메소드 추가
public static void permissionMkdirs(File directory){
	if(!directory.exists()){
		//폴더 권한 설정 - 보안 취약점 대응[2019.01.29, kimjaeyeol]
		directory.setExecutable(false, true);
		directory.setReadable(true);
		directory.setWritable(false, true);
		directory.mkdirs();
	}
}

// 2) permissionMkdirs(folder) 사용하여 디렉토리 생성시 권한설정을 하도록 한다.
permissionMkdirs(folder);
```

### 5. Private 배열에 Public 데이터 할당 & Public 메소드로부터 반환된 Private 배열
* Public으로 선언된 데이터 또는 메소드의 파라미터가 Private 배열에 저장되면, 외부에서 Private 배열에 접근할 수 있습니다.
* Private 배열을 Public 메소드가 반환하면, 배열 주소값이 외부 공개됨으로써 외부에서 배열 수정이 가능해집니다.
```java
private List<Map<String, Object>> authViewMenuList;
public List<Map<String, Object>> getAuthViewMenuList() {
	return authViewMenuList;
}
public void setAuthViewMenuList(List<Map<String, Object>> authViewMenuList) {
	this.authViewMenuList = authViewMenuList;
}
```
* 수정
  * getter : 새로운 배열을 만들어 담는다.
  * setter : 파라미터값을 그대로 담지 않고 루프를 돌며 하나씩 꺼내어 담는다.
```java
private List<Map<String, Object>> authViewMenuList;
public List<Map<String, Object>> getAuthViewMenuList() {
	List<Map<String, Object>> authViewMenuList = new ArrayList<>();
	authViewMenuList.addAll(this.authViewMenuList);
	return authViewMenuList;
}
public void setAuthViewMenuList(List<Map<String, Object>> authViewMenuList) {
	this.authViewMenuList = new ArrayList<>();
	for (Map<String, Object> map : authViewMenuList) {
		this.authViewMenuList.add(map);
	}
}
```

<br/>

## C. 위험도 낮음

### 1. 적절하지 않은 난수값 사용
* java.lang.Math.random() 사용 금지, java.util.Random 으로 대체한다.
* SolutisStringUtil.getRandomPassword() 메소드 변경함.

### 2. 이름 짓기 규칙 위반 (변수명)
* 변수명, 메서드명 등 이름을 짓는데 반드시 지켜야할 규칙
1. 대소문자 구별
2. 키워드 사용 불가
3. 첫문자는 숫자 불가
4. 특수문자는 _와 $만 허용
* 권장 규칙
1. 패키지명 첫글자는 소문자
2. 클래스명 첫글자는 대문자
3. 변수명 첫글자는 소문자
4. 메서드명 첫글자는 소문자
5. 상수명은 모두 대문자
```java
// 상수이므로 모두 대문자이 'LOGGER' 를 표준으로 사용한다.
private static final LOGGER = LogManager.getLogger()

// serialVersionUID 선언하지 않고 자동 생성되도록 한다.
```

### 3. switch 구문 내의 default 부재
* switch 구문은 반드시 default 구문을 작성해야 한다.

### 4. Final 변경자 없는 상수
* 변경되지 않는 멤버 상수는 final 을 선언해야 한다.
```java
// 멤버 상수는 final 이 없고 모두 대문자이어야 한다.
private static int readableAuth = 1;
// 권장 선언
private static final int READABLE_AUTH = 1;
```

### 5. 불필요한 코드 (비어있는 함수) & 불필요한 코드 (비어있는 IF문) & 불필요한 코드 (비어있는 case문)
* 메소드나 분기문의 내용이 비어있는 경우 검출된다.
* 비어있는 함수는 @Override 메소드들이 주 대상이다.
* 로깅 처리라도 해주어야 검출되지 않는다.
* 구문 내에 주석문만 있어도 검출 대상이다.
