# MyNotes

## View Content 
> 就2步驟：製作內容頁面(HTML)、呼叫關閉(JS)

### include/xxx_view_content
`id="productContent"` : xxxContent，xxx請改對應的系統代號名稱‼️ 非常重要。  
公版HTML：  

```html=
<!-- productModal -->
<div class="modal fade" id="productContent" data-backdrop="static" tabindex="-1" aria-labelledby="exampleModalLabel"
    aria-hidden="true">
    <div class="modal-dialog modal-xl modal-dialog-scrollable">
        <div class="modal-content">
            <div class="modal-body">
                <div class="card card-info">
                    <div class="card-header bg-light">
                        <h3 class="card-title text-muted" th:text="${appName}"></h3>
                        <!-- 要替換th:text -->
                    </div>
                    <div class="card-body">
                        <form name="editForm">
                            <!-- view product(appName) -->
                            <!-- 這邊放對應的html -->
                        </form>
                    </div>
                </div>

                <!-- 審核歷程 -->
                <div class="accordion" id="accordionExample">
                    <div class="card">
                        <div class="card-header bg-light text-muted" id="headingOne">
                            <a href="#" class="px-0 text-muted" data-toggle="collapse" data-target="#collapseOne"
                                aria-expanded="true" aria-controls="collapseOne">
                                <i class="fa fa-angle-down mr-2 text-muted float-left pt-1"></i>
                            </a>
                            <h3 class="card-title text-muted" th:text="#{we01SignHistory}"></h3>
                        </div>

                        <div id="collapseOne" class="collapse show" aria-labelledby="headingOne"
                            data-parent="#accordionExample">
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-bordered">
                                        <thead>
                                            <tr class="bg-light text-center">
                                                <th th:text="#{we01CategoriesOfApplication}">
                                                </th>
                                                <th th:text="#{we01VerifiedStatus}"></th>
                                                <th th:text="#{we01Releasablestate}"></th>
                                                <th th:text="#{globalProcessLogFrom}"></th>
                                                <th th:text="#{we01Receiver}"></th>
                                                <th th:text="#{we01VerifiedUpdateStatus}"></th>
                                                <th th:text="#{we01Feedback}"></th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr class="text-center" ng-repeat="alllog in alllogs">
                                                <td class="align-middle">
                                                    <div>
                                                        <span ng-if="alllog.ApplyType=='0'"
                                                            th:text="#{we01ActiveApply}"></span>
                                                        <span ng-if="alllog.ApplyType=='1'"
                                                            th:text="#{we01InActiveApply}"></span>
                                                    </div>
                                                </td>
                                                <td class="align-middle">
                                                    <div>
                                                        <span ng-if="alllog.SignStatus=='0'"
                                                            th:text="#{we01Pending}"></span>
                                                        <span ng-if="alllog.SignStatus=='1'"
                                                            th:text="#{we01Processing}"></span>
                                                        <span ng-if="alllog.SignStatus=='2'"
                                                            th:text="#{we01Approval}"></span>
                                                        <span ng-if="alllog.SignStatus=='3'"
                                                            th:text="#{we01DisApproval}"></span>
                                                    </div>
                                                </td>
                                                <td class="align-middle">
                                                    <div>
                                                        <span ng-if="alllog.IsActive=='0'">-</span>
                                                        <span ng-if="alllog.IsActive=='1'"
                                                            th:text="#{we01Active}"></span>
                                                        <span class="bg-secondary" ng-if="alllog.IsActive=='2'"
                                                            th:text="#{we01InActive}"></span>
                                                    </div>
                                                </td>
                                                <td class="align-middle">{{alllog.Editor}}</td>
                                                <td class="align-middle">{{alllog.Receiver}}
                                                </td>
                                                <td class="align-middle">{{alllog.ModifyTime}}
                                                </td>
                                                <td class="align-middle">
                                                    <div>
                                                        <span ng-if="alllog.IsActive == ''">-</span>
                                                        <span ng-if="alllog.IsActive !=''">{{alllog.Note}}</span>
                                                    </div>
                                                </td>
                                            </tr>
                                            <!-- If data is Null -->
                                            <tr class="text-center" ng-show="alllogsLength == Null">
                                                <td colspan="7" th:text="#{tableRowNoRecord}">
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
            <div class="py-3 px-3">
                <div class="row">
                    <div class="col-lg-6 col-md-12">
                        <!-- <button id="previewBtn" type="button" class="btn btn-tangerine px-3">
                        <th:block th:text="#{we01btnPreview}">
                    </button> -->
                    </div>
                    <div class="col-lg-6 col-md-12 mt-lg-0 mt-sm-2">
                        <button type="button"
                            class="btn bg-secondary text-white px-5 float-lg-right float-md-left mr-md-1"
                            ng-click="closeContent()" data-bs-dismiss="modal" aria-label="Close">
                            <th:block th:text="#{btnReturn}">
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

```
![image](https://user-images.githubusercontent.com/99180553/175255323-a29c3510-bef4-4e35-ac7c-256cdc780768.png)



### Show Modal
> `ng-click` 寫在內容的button
`#productContent`: xxxContent，xxx請改對應的系統代號名稱‼️ 非常重要。

```html=
// View Content Start
$scope.viewContent = function (id) {
	$('#productContent').modal('show'); //->>括號裡填上剛剛說很重要的id,show就是顯示懸浮視窗
	//這會呼叫query Modal內容的function
}
// View Content End
```

### Close Modal
> `ng-click` 寫在返回的button
`#productContent`: xxxContent，xxx請改對應的系統代號名稱‼️ 非常重要。
```html=
// Close View Content Start
$scope.closeContent = function () {
	$('#productContent').modal('hide'); //->>括號裡填上剛剛說很重要的id,hide就是關閉懸浮視窗
};
// Close View Content End
```
### 安插Modal
```
<!-- View Content Modal -->
<span th:replace="/include/web_info_view_content"></span>
```

### 注意事項 
```
<label for="DataTime" class="font-weight-normal" th:text="#{we01DataDate}"
	ng-required="true"></label>
<input type="datetime-local" class="form-control" readonly id="DataTime"
	name="DataTime" ng-model="DataTime" ng-required="true">
```
1. `label`:  `for="xxx"`,xxx請改成等同你的`name="xxx"`的值，必須改！！！！！！  
2. `th:text="#{pleaseEnter}"` : 自行替換

