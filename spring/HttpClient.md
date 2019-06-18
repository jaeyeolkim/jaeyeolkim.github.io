## Spring 에서 jsp 얻기
> 결재 템플릿 jsp를 Service에서 호출하여 그 내용을 얻어와야 한다.
* 호출하는 Service
```java
private String getHtmlBody(HttpServletRequest request){
	String html = "";
	try{
		HttpClient client = HttpClientBuilder.create().build(); // HttpClient 생성
		String requestUrl = request.getRequestURL().toString();
		final String hostUrl = requestUrl.substring(0, requestUrl.indexOf(request.getRequestURI()));
		//GET 메소드 URL 생성하여 호출
		String pathVariable = "1";
		HttpGet httpGet = new HttpGet(hostUrl + "/approval/template/" + pathVariable);
		HttpResponse response = client.execute(httpGet);
		//정상 응답시 Response 출력
		if (response.getStatusLine().getStatusCode() == 200) {
			ResponseHandler<String> handler = new BasicResponseHandler();
			html = handler.handleResponse(response);
			logger.debug("html={}", html);
		} else {
			logger.error("response is error : {}", response.getStatusLine().getStatusCode());
		}
	}catch (Exception e){
		logger.error("error={}", e);
	}
	return html;
}
```
<br/>

* jsp를 리턴하는 Controller

```java
@RequestMapping(value = {"", "{param1}"}, method = {RequestMethod.GET})
public void page(ParamVO paramVO, Map<String, Object> model, @PathVariable String param1) {
	Map<String, Object> params = new HashMap<>();
	logger.debug("param1={}", param1);
	params.put("id", "oyejin");
	params.put("name", "다람쥐");
	model.put("params", params);
}
```

* jsp template
```html
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>
<!DOCTYPE html>
<html lang="ko">
  <body>
    <h2 id="title"><spring:message code="APPROVE"/></h2>
    ${params.id}
    ${params.name}
  </body>
</html>
```

* dispatcher-servelt.xml 설정
  - Interceptor 에 해당 url에 대한 exclude-mapping path 설정이 필요한다.