## 웹표준/웹접근성 대응
### 자동 검사
* 웹표준(html) : https://validator.w3.org/
* 웹표준(css) : http://jigsaw.w3.org/css-validator/
  * 웹표준 검사는 error 가 발견되지 않아야 한다.
* 웹접근성 : openWAX(크롬 확장 프로그램)
  * 100점이 되어야 한다.
* 웹접근성 항목에 대한 정리가 잘 된 사이트
  * http://darum.daum.net/accessibility/pc
  * 수동 검사 내용까지 맞추기에는 번거로움이 많다.

### 웹표준 주 변경점
* 표준 속성이 아닌 속성 사용
  * <code>validLabel, validParentId -> data-validLabel, data-validParentId</code> 로 변경해야 한다.
* label 제공
  * input, select 대상
  * <code>\<label for="요소의 id"></code>

### 웹접근성 주 변경점
> openWAX에서 사용하는 항목 기준으로 작성
* (1) 적절한 대체 텍스트 제공
  * <code>img alt="필수"</code>
  
* (12) 건너뛰기 링크
  * 순수하게 javascript 사용시에는 button으로 변경
  ```html
  <a href="javascript:alert('네이버');">네이버</a>
  -> <button type="button" onclick="alert('네이버')">네이버</button>
  ```
  * a 태그를 사용해야 하는 경우 'role' 을 사용하여 버튼임을 명시한다.
  ```html
  <a onclick="alert('네이버')" role="button">네이버</a>
  ```
  
* (13) 제목 제공
  * <code>iframe title="필수"</code>
  * <code>head 내의 title 존재</code>

* _본 문서는 mfa 웹표준/접근성 조치 내용을 기반으로 작성하였습니다.(일부내용 추가)_
