## 동적으로 dropdown 생성하기
> 선택하는 기준에 따라 dropDown list가 달라진다. 또한 행마다 다른 경우이다.

* <code>labelField</code> 에 보여질 컬럼을 지정한다.
```javascript
{name : "VERIFY_CODE", header : {text : '검증방법'}, width : 200, lookupDisplay: true, editor: gridJs.dropDown(), labelField: 'VERIFY_NAME'}
```

* 두 개의 이벤트 처리가 필요하다.
  * onCurrentChanging : 동적으로 dropDown values, labels 구성
  * onGetEditValue : labelField에 보여질 텍스트 값을 설정
```javascript
<%-- 그리드의 포커스 셀의 위치가 변경되기 직전에 호출 --%>
gridView01.onCurrentChanging =  function (grid, oldIndex, newIndex){
  var provider = grid.getDataSource();
  var fieldName = newIndex.fieldName;
  if(fieldName === 'VERIFY_CODE'){
    <%-- 환경기준서 선택시 데이터셋(dataProvider21)을 조회하여 해당 dropdown list를 가져온다 --%>
    var envSeq = provider.getValue(newIndex.dataRow, 'ENV_SEQ');
    var jsonRows = dataProvider21.getJsonRows(0, -1);
    var valuesList = new Array();
    var labelsList = new Array();

    $.each(jsonRows, function(idx, data) {
      if(envSeq === data.ENV_SEQ){
        valuesList.push(data.VERIFY_CODE);
        labelsList.push(data.VERIFY_NAME);
      }
    });
    var column = grid.columnByName('VERIFY_CODE');
    column.values = valuesList;
    column.labels = labelsList;
    grid.setColumn(column);
  }
};
<%-- 셀 편집이 완료되었을때 발생하는 이벤트, dropDown을 동적으로 생성한 후 사용자가 값을 선택하면 labelField가 보여진다(행마다 dropDown list가 다르기 때문에) --%>
gridView11.onGetEditValue = function (grid, index, editResult) {
  if (index.fieldName === "VERIFY_CODE") {
    grid.setValue(index.itemIndex, "VERIFY_NAME", editResult.text);
  }
};
```
* _kotiti project ModelPartEd.jsp 작업중_
