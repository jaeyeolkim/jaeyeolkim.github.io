# fieldset 속성을 사용하여 disabled 일괄 처리하기
* 폼 안에 \<fieldset\> 태그를 사용하여 그룹화 할 수 있다.
* 속성으로 disabled 를 갖기 때문에 폼 안에 여러 요소들을 묶어서 컨트롤할 수 있다.
* 또한 \<fieldset\> 은 라벨인 \<legend\> 와 같이 사용된다.
```html
<form>
  <fieldset disabled>
    <legend>Disabled input list</legend>
    <div class="form-group">
      <label for="disabledTextInput">Disabled input</label>
      <input type="text" id="disabledTextInput" class="form-control" placeholder="Disabled input">
    </div>
    <div class="form-group">
      <label for="disabledSelect">Disabled select menu</label>
      <select id="disabledSelect" class="form-control">
        <option>Disabled select</option>
      </select>
    </div>
    <div class="form-group">
      <div class="form-check">
        <input class="form-check-input" type="checkbox" id="disabledFieldsetCheck" disabled>
        <label class="form-check-label" for="disabledFieldsetCheck">
          Can't check this
        </label>
      </div>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </fieldset>
</form>
```
* 참고자료
1. https://getbootstrap.com/docs/4.3/components/forms/#disabled-forms
2. https://developer.mozilla.org/ko/docs/Web/HTML/Element/fieldset
