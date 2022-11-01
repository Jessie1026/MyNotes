# MOC開發筆記
## 同UUID相鄰同底色
### Frontend
#### JS
1. 需要一個隨機產生顏色的function
- 考量部分功能頁面，未來文章數量不可預測   
- 每一篇文章需以不同顏色方便進行辨識   
- 須設定透明度，因為隨機色也可能是深色會覆蓋內容（已測試過0.1是最佳配置）  

範例code:  
```
// Random Color
function random_bg_color() {
  var x = Math.floor(Math.random() * 256);
  var y = Math.floor(Math.random() * 256);
  var z = Math.floor(Math.random() * 256);
  var bgColor = "rgba(" + x + "," + y + "," + z + ",0.1" + ")";
  return bgColor;
}
```
2. 搭配
```javascript=
// Setting same uuid with same bg color
let temp_uuid;
let temp_color;
let bg_colors = [];
$scope.alldatas.forEach(function (alldata) {
  let uuid = alldata.UUID;
  if (uuid == temp_uuid) {
    //跟前者為同uuid，配置同色
    bg_colors.push(temp_color);
    temp_uuid = uuid;
  } else {
      //跟前者為不同uuid，重新產生顏色
      temp_color = random_bg_color();
      bg_colors.push(temp_color);
      temp_uuid = uuid;
  }
});
$scope.rowColors = bg_colors;
```


#### HTML
需配合angularjs語法`ng-style`控制底色配置！  
範例code（簡化後的code,）:  
```html=
<table class="table table-bordered">
    <tr>
      <th>Name</th>
      <th>Fcount</th>
    </tr>
    <tr ng-repeat="x in details" ng-style="{ background: rowColors[$index] }">
        <td>{{ x.name }}</td>
        <td>{{ x.fcount }}</td>
    </tr>
</table>
```
### Backend
排序方式：同uuid相鄰。  
