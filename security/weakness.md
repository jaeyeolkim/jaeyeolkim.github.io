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
* LOGGER.error(e) 를 찍어준다.

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
```java
} catch (Exception e) {
    LOGGER.error(getSolutisErrorMsg(e));
}

//SolutisStringUtil.java 에 아래 메소드 추가
public static String getSolutisErrorMsg(Exception exception){
    return null2string(exception);
}
```



### 9. Private 배열에 Public 데이터 할당 & Public 메소드로부터 반환된 Private 배열
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
