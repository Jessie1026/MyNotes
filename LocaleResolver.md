# Replace CookieLocaleResolver To SessionLocaleResolver

## WebMVCConfig.java
請記得先把`CookieLocaleResolver`的設定註解掉或刪除，並新增以下程式碼：
```
/**
* 默認解析器 其中locale表示默認語言,當請求中未包含語種信息，則設置默認語種
* 當前默認為TAIWAN, zh_TW
*/   
@Bean
public LocaleResolver localeResolver() {
    SessionLocaleResolver slr = new SessionLocaleResolver();
    slr.setDefaultLocale(Locale.TAIWAN);
    return slr;
}
    
/**
* 默認攔截器 其中lang表示切換語言的參數名
*/
@Bean
public LocaleChangeInterceptor localeChangeInterceptor() {
    LocaleChangeInterceptor lci = new LocaleChangeInterceptor();
    lci.setParamName("lang");
    return lci;
}
```

## DirectIndexController.java
**調整**`index`方法，異動結果如下：
```
/**
* 網站首頁
* 
* @param locale
*            Locale
* @param model
*            Model
* @return Url
*/
@GetMapping(value = { "/", "/index" })
public String index(HttpServletRequest request, HttpServletResponse response, Model model, HttpSession session) {
String lang = (String) session.getAttribute("lang");
model.addAttribute("lang", lang);
return isUserLoggedIn() ? response.isCommitted() ? null : "redirect:/pub/index" : "index";
}
```

**新增**`changeLanguage`方法，如下：
```
@GetMapping("/setLanguage")
public String changeLanguage(@RequestParam("lang") String lang, HttpSession session) {
    session.setAttribute("lang", lang);

    return "redirect:/";
}
```

## i18n.js
新增`switchLanguage(lang)`function：
```
function switchLanguage(lang) {
    fetch('/setLanguage?lang=' + lang)
        .then(function(response) {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            // 請求成功，重新加載頁面
            window.location.reload();
        })
        .catch(function(error) {
            // 請求失敗，處理錯誤
            console.error('Error setting language:', error);
        });
}
```

至於先前cookie的部分已用不上，建議註解掉`$(document).ready(function() {...}`    裏面的code～


## index.js
> 溫馨提醒：專案裡有兩個index檔案，這邊的index路徑是：`vans-web/src/main/resources/static/js/index.js`!!!

**調整**`$scope.login();`實際調整結果如下：
```
$scope.login = function() {
		var grecaptcha = $("#g-recaptcha-response").val();
		if (grecaptcha == '' && $scope.showTwoFactor) return;
		$('#loginErrorBtn').attr('disabled', true);

		// 从隐藏的元素中获取lang值
		var lang = document.getElementById("lang").textContent;

//		bootbox.dialog({
//            closeButton: false,
//            message: '<i class="fas fa-fw fa-circle-notch fa-spin"></i>' + loginLogin
//        });
		var request = 'account=' + $scope.userName + '&code=' + btoa($scope.userCode) + '&otp=' + $scope.otpCode + '&gtp=' + grecaptcha +'&lang=' + lang;
		$http.post('./public/api/login', request, csrf_config_form).then(function(response) {
//			console.log("response="+JSON.stringify(response));
			if (response.data.success == true) {
				if (response.data.url != "") {
					// $window.location = response.data.url;
					var url = response.data.url;
					// 跳轉頁面前先夾帶語系資料
					$scope.switchLanguage(lang, url);
				} else {
					bootbox.hideAll();
					// $scope.otpCode = "";
					$scope.showTwoFactor = true;
				}
				$timeout(function() {
					// $window.document.getElementById("otpCode").focus();
				}, 0); 
			} else {
				bootbox.hideAll();
				$scope.userName = "";
				$scope.userCode = "";
				// $scope.otpCode = "";
				$scope.showTwoFactor = false;
				$('#loginErrorBtn').attr('disabled', false);
				bootbox.alert({
					message : response.data.msg,
					buttons : {
						ok : {
							label : btnClose,
							className : 'btn_primary'
						}
					},
					callback: function() {
						$timeout(function() {
							$window.document.getElementById("userName").focus();
						}, 0); 
					}
				});
			}
		}).catch(function() {

			bootbox.hideAll();
			$scope.userCode = "";
			// $scope.otpCode = "";
			$scope.showTwoFactor = false;
			bootbox.alert({
				message : loginFail,
				buttons : {
					ok : {
						label : btnClose,
						className : 'btn_primary'
					}
				}
			});
		});
	};

```
怕不清楚，這邊附上異動區塊提示圖例！
![alt text](image.png)


## index.html
> 溫馨提醒：專案裡有兩個index檔案，這邊的index路徑是：`vans-web/src/main/resources/templates/index.html`!!!

在合適處安插以下語法：
```
<p id="lang" class="d-none" th:text="${lang}"></p>
```

## Modify Language Dropdown Menu
請搜尋全專案使用關鍵字`data-lang=`，針對這些含有語言菜單的html進行調整！請補上`onclick="switchLanguage('語系代碼')"`onclick function.
實際調整結果如下：
```html
<div class="dropdown-menu dropdown-menu-right">
    <a class="dropdown-item" href="#" data-lang="zh_TW" th:text="#{globalLanguageZhTw}" onclick="switchLanguage('zh_TW')"></a>
    <a class="dropdown-item" href="#" data-lang="en_US" th:text="#{globalLanguageEnUs}" onclick="switchLanguage('en_US')"></a>
    <a class="dropdown-item" href="#" data-lang="ja_JP" th:text="#{globalLanguageJaJp}" onclick="switchLanguage('ja_JP')"></a>
</div>
```



## 解決Login後Session Invalid Problem
> 這邊只是補解釋一下為何login page要埋隱形element的原因！！！（可略過）

由於現在改用較安全**SessionLocaleResolver**，但是Session有一個特性，user登入後session會刷新（invalid），造成登入前如果有更改語系，登入後又會跳成default設置的語系。

所以這邊我做了調整，讓user login前如果有更改語系，我會把lang存在java model attribute中，但因為JavaScript無法直接取用java model attribute 的參數，所以我將語言設定儲存在一個隱藏的`<p>`元素中，並使用JavaScript從該元素中取得語言值。

並調整`login()`function，判斷是登入成功後，跳轉頁面前夾帶語系代碼！這樣就可以帶著之前選的語系一起登入了！突破session invalid的窘境。

![alt text](image-1.png)



整理by Jessie






