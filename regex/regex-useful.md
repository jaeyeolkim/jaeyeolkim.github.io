## 유용한 정규표현식 모음

### 1. img 태그 안에 alt 가 없는 경우 찾아서 alt="" 넣기
* 표현식
```javascript
(?!<img([^<]*|[^>]*<%=[^>]*%>[^>]*)\salt[^=>]*=[^>]*[/]*>)<img([^<]*|[^>]*<%=[^>]*%>[^>]*)\/>
```
* 대상 텍스트
```javascript
(O) <img src="<%=request.getContextPath()%>/common/images/common/dot.gif" align="absmiddle" />
(X) <img src="<%=request.getContextPath()%>/common/images/common/dot.gif" alt="" />
(O) <img src="/common/images/common/dot.gif" align="absmiddle" />
(X) <img src="/common/images/common/dot.gif" alt="" />
```
* replace
```javascript
$1<img$2alt="" />
```
* 최종 결과
  * 첫번째, 세번쨰 img 태그 안에 alt="" 가 추가 되었다
```javascript
<img src="<%=request.getContextPath()%>/common/images/common/dot.gif" align="absmiddle" alt="" />
<img src="<%=request.getContextPath()%>/common/images/common/dot.gif" alt="" />
<img src="/common/images/common/dot.gif" align="absmiddle" alt="" />
<img src="/common/images/common/dot.gif" alt="" />
```
