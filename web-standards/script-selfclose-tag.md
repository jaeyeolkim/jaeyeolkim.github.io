## script self close tag
* bootstrap4 navbar dropdown이 정상 동작하지 않았다.(클릭하면 바로 펼쳐지지 않음)
* 소스의 boostrap 관련 부분만 남기고 튜토리얼을 그대로 붙여넣기 했는데도 해결되지 않았다.
```
<script src="/static/js/jquery-3.3.1.slim.min.js"></script>
<script src="/static/js/bootstrap.bundle.min.js"></script>
<script src="/static/js/bootstrap.min.js"></script>
```
 
* 이제 script, css 선언 부분을 튜토리얼에 있는 cdn 을 사용하니 정상적으로 돌았다.
```
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
```

* https://getbootstrap.com/docs/4.3/getting-started/introduction/ 에 보면
<code>bootstrap.bundle.min.js include Popper</code> 라고 씌워져 있는데, 그렇다면 동일하게 동작해야 하는 것 아닐까 싶어서
스크립트를 셀프 클로즈로 변경 하였더니 정상 동작하였다.
```
<script src="/static/js/bootstrap.bundle.min.js"/>
```
