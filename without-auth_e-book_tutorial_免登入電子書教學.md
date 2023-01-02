<img src="https://img.shields.io/badge/dynamic/json?label=Visits%20Count&query=value&url=https%3A%2F%2Fapi.countapi.xyz%2Fhit%2Fbaa4f015258d0312f627b5659d93d72e%2Fvisits&maxAge=1"></img>

# 教學用電子書與相關工具免登入教學

**使用前請務必閱讀 [免責聲明](https://gist.github.com/SiongSng/baa4f015258d0312f627b5659d93d72e#%E5%85%8D%E8%B2%AC%E8%81%B2%E6%98%8E)**

## 免責聲明
### 請勿將本腳本作為抄答案、侵權等惡意用途，使用本腳本者，請*自行承擔*所有後果與風險。

## 簡介
本腳本用於繞過台灣電子書與教學工具的前端身份驗證，達成不需要教師帳號即可使用。

## 開發緣由
原本是因為開發者忘記帶課本，但又想要查閱課本的資料，心血來潮研究看看電子書的驗證設計。  
開發這個並不希望被拿去抄答案或侵權，這對學習一點幫助都沒有。
是希望讓真正需要用的人可以用到，也希望各家出版社能提供一種學生與家長的版本，專為學習者設計，就可以完美解決這些問題。

## 如何使用
這邊示範翰林版的電子書如何使用，其他出版社以此類推
首先先前往要使用的電子書或相關工具網站 (下方有連結)，接著在瀏覽器的開發者頁面 (F12) 中的主控台 (Console) 輸入以下腳本，最後重新載入網頁即可迴避登入。  
> 這邊是以 Google Chrome 的環境作為示範，Firefox/Edge/Safari 大同小異。  

![image](https://user-images.githubusercontent.com/48402225/188836801-3688329c-fe73-4c1b-b762-ba0d7d4a50c4.png)

## 腳本

### ✅ 康軒電子書
連結：  
[康軒網頁版電子書 (國民小學)](https://webetextbook.knsh.com.tw/2/index.html?code_degree=1&openExternalBrowser=1)  
[康軒網頁版電子書 (國民小學/英語/閩南語/客家話)](https://webetextbook.knsh.com.tw/2/index.html?code_degree=3&openExternalBrowser=1)  
[康軒網頁版電子書 (國民中學)](https://webetextbook.knsh.com.tw/2/index.html?code_degree=2&openExternalBrowser=1)  
```js
localStorage.setItem("loginAccount", "mockAccount"); // 設定一個假的帳號
localStorage.setItem("uuid", "mockUUID"); // 設定假的 UUID
```
> 最後測試時間：2022/12/14

### ❌ 南一電子書
連結：[OneBook 南一電子書](https://reader.oneclass.com.tw/bookshelf)  
```js
let mockToken = JSON.stringify({
    "code": "SUCCESS",
    "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbXlhY2NvdW50Lm5hbmkuY29vbC8iLCJzdWIiOiJ1c2Vycy9qYWNreWNoaXUwMSIsImZyb20iOiJOYW5pIiwidXNlcm5hbWUiOiJqYWNreWNoaXUwMSIsImVtYWlsdmFsaWQiOnRydWUsIm1vYmlsZXZhbGlkIjpmYWxzZSwiZW1haWwiOiJraW5tYTE1OTg3NTMyQGdtYWlsLmNvbSIsInVpZCI6ImI1ZjE3MGYwLTI5ZmMtMTFlZC04NDJjLTQ5OTAxMGVhODI0MCIsImp0aSI6IjliOGI5OTE1LWYyMGQtNGNlMS04ZmJjLTA0OWFhYjkzZTY4ZiIsImlhdCI6MTY2NzIzMTA2NiwiZXhwIjoxNjcyNDE1MDY2fQ.R9cjUUSocKL9CiPTa2Tf8zPNiZLSJLRqH9eQAniMsJw"});

let fieldName = "nani_oneclass_login_token";
var d = new Date();
d.setTime(d.getTime() + (1 * 24 * 60 * 60 * 1000));
var expires = "expires=" + d.toUTCString();
var hostname = window.location.hostname;
if (hostname.indexOf("oneclass.com.tw") > 0) {
  document.cookie = fieldName + "=" + mockToken + ";" + expires + ";path=/;domain=oneclass.com.tw";
} else {
  document.cookie = fieldName + "=" + mockToken + ";" + expires + ";path=/";
}
    
localStorage.setItem("nani_tokenInfo", mockToken); // 設定身分驗證用的 toekn
```
> 最後測試時間：2022/12/14  
> token 由 @jackychiu0207 提供

### ✅ 翰林電子書
連結：[翰林行動大師電子書](https://edisc3.hle.com.tw/edisc_v3/ebook_v2023.html)  

```js
let time = new Date().getTime().toString();
localStorage.setItem("last_signinX_v2023", time); // 將帳號登入日期設定為現在，避免被判定為過期
localStorage.setItem("roleX_v2023", "老師"); // 設定身份為老師
localStorage.setItem("emailX_v2023", "test@test.com"); // 由於翰林電子書會驗證是否有設定 email，如果有設定才能使用
localStorage.setItem("tokenX_v2023", "eyJhbGciOiJSUzI1NiIsImtpZCI6Ijg1NzgwNWYxZGQ3ZmE5YTZiNTI3ZjQ0ZWNmZmJkNDhjIiwidHlwIjoiSldUIn0.eyJuYmYiOjE2NjkwMjQyOTUsImV4cCI6MTY3MTcwMjY5NSwiaXNzIjoiaHR0cHM6Ly9pZC5obGUuY29tLnR3IiwiYXVkIjpbImh0dHBzOi8vaWQuaGxlLmNvbS50dy9yZXNvdXJjZXMiLCJhcGkxIiwiSWRlbnRpdHlTZXJ2ZXJBcGkiLCJoYW5saW4tYXBpIl0sImNsaWVudF9pZCI6ImpzIiwic3ViIjoiZGJiYmEwNmEtNWNkNy00NTI5LWI2MjEtOTBlYjdhMGIxOWZlIiwiYXV0aF90aW1lIjoxNjY5MDI0MjkxLCJpZHAiOiJsb2NhbCIsIkFzcE5ldC5JZGVudGl0eS5TZWN1cml0eVN0YW1wIjoiNURHN1ZSWVVWRUdUSjJVQ1czU0FDRkpBT1NHM0RONEIiLCJyb2xlIjpbIuiAgeW4qyIsIuiAgeW4qyJdLCJlbWFpbCI6WyJraW5tYTE1OTg3NTMyQGdtYWlsLmNvbSIsImtpbm1hMTU5ODc1MzJAZ21haWwuY29tIl0sImZhbWlseV9uYW1lIjoi576FIiwiZ2l2ZW5fbmFtZSI6IuWFg-iyniIsIm5hbWUiOiLnvoXlhYPosp4iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwicHJlZmVycmVkX3VzZXJuYW1lIjoi576F5YWD6LKeIiwidXNlcl9kb21haW4iOiJlZHUiLCJzY29wZSI6WyJvcGVuaWQiLCJwcm9maWxlIiwiYXBpMSIsIklkZW50aXR5U2VydmVyQXBpIiwiaGFubGluLWFwaSIsIm9mZmxpbmVfYWNjZXNzIl0sImFtciI6WyJwd2QiXX0.fX6birbwdGyrT1iaIPRZ_g7-bIDt8pMNq-P-hIl0uDVHIvHp7cvVDFwFCUmn4JT1oRLXuULj3Grym1T3xkp68o3NzH2AoJ9_zFLjZNL8i3oWKTbsYqNHmCu2FP-sNM38eeJSv9A3Gpjbjt6MNAIh-5Ww1zVeURep7gHMs56oxLqo-957pbfMT7_2DWucPshS39S0o2FBq99jVmG1JI7czyoGUlv-Lqhiv6FRT6VKB1EI0nRhrhiNMGA9qwAX-FAs9O5vDqptFkaDy-Bz4Zgjymzo0jEDnjblKuSgqdzpx1zt8D09F73t5kmR57yN8iN_UNZOo1WKD9Qk2Knpnxibtw"); // 設定身分驗證用的 token
```
> 最後測試時間：2022/11/22  
> token 由 @jackychiu0207 提供

### ✅ 何嘉仁電子書
連結：[何嘉仁電子書](https://bookonline.hess.com.tw/bookcase/)  

```js
localStorage.setItem("isLogin", "true"); // 設定登入狀態為是 (true)
localStorage.setItem("uuid", "mock_user"); // 設定假的教師 UUID
```
> 最後測試時間：2022/12/14

### ✅ 翰林雲端命題大師
連結：[翰林雲端命題大師](https://testbank.hle.com.tw/)  

```js
var d = new Date();
d.setTime(d.getTime() + (1 * 24 * 60 * 60 * 1000));

let userInfo = {
  id_token: 'mock_id_token',
  session_state: 'test',
  access_token: 'eyJhbGciOiJSUzI1NiIsImtpZCI6Ijg1NzgwNWYxZGQ3ZmE5YTZiNTI3ZjQ0ZWNmZmJkNDhjIiwidHlwIjoiSldUIn0.eyJuYmYiOjE2NjkwMjQyOTUsImV4cCI6MTY3MTcwMjY5NSwiaXNzIjoiaHR0cHM6Ly9pZC5obGUuY29tLnR3IiwiYXVkIjpbImh0dHBzOi8vaWQuaGxlLmNvbS50dy9yZXNvdXJjZXMiLCJhcGkxIiwiSWRlbnRpdHlTZXJ2ZXJBcGkiLCJoYW5saW4tYXBpIl0sImNsaWVudF9pZCI6ImpzIiwic3ViIjoiZGJiYmEwNmEtNWNkNy00NTI5LWI2MjEtOTBlYjdhMGIxOWZlIiwiYXV0aF90aW1lIjoxNjY5MDI0MjkxLCJpZHAiOiJsb2NhbCIsIkFzcE5ldC5JZGVudGl0eS5TZWN1cml0eVN0YW1wIjoiNURHN1ZSWVVWRUdUSjJVQ1czU0FDRkpBT1NHM0RONEIiLCJyb2xlIjpbIuiAgeW4qyIsIuiAgeW4qyJdLCJlbWFpbCI6WyJraW5tYTE1OTg3NTMyQGdtYWlsLmNvbSIsImtpbm1hMTU5ODc1MzJAZ21haWwuY29tIl0sImZhbWlseV9uYW1lIjoi576FIiwiZ2l2ZW5fbmFtZSI6IuWFg-iyniIsIm5hbWUiOiLnvoXlhYPosp4iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwicHJlZmVycmVkX3VzZXJuYW1lIjoi576F5YWD6LKeIiwidXNlcl9kb21haW4iOiJlZHUiLCJzY29wZSI6WyJvcGVuaWQiLCJwcm9maWxlIiwiYXBpMSIsIklkZW50aXR5U2VydmVyQXBpIiwiaGFubGluLWFwaSIsIm9mZmxpbmVfYWNjZXNzIl0sImFtciI6WyJwd2QiXX0.fX6birbwdGyrT1iaIPRZ_g7-bIDt8pMNq-P-hIl0uDVHIvHp7cvVDFwFCUmn4JT1oRLXuULj3Grym1T3xkp68o3NzH2AoJ9_zFLjZNL8i3oWKTbsYqNHmCu2FP-sNM38eeJSv9A3Gpjbjt6MNAIh-5Ww1zVeURep7gHMs56oxLqo-957pbfMT7_2DWucPshS39S0o2FBq99jVmG1JI7czyoGUlv-Lqhiv6FRT6VKB1EI0nRhrhiNMGA9qwAX-FAs9O5vDqptFkaDy-Bz4Zgjymzo0jEDnjblKuSgqdzpx1zt8D09F73t5kmR57yN8iN_UNZOo1WKD9Qk2Knpnxibtw',
  refresh_token: 'test',
  token_type: 'bearer',
  scope: 'test',
  profile: {
    role: ['老師'],
    sub: 'mock_user_id',
    email: 'user@mock.com'
  },
  expires_at: d.getTime()
};
let key = `oidc.user:https://id.hle.com.tw/:js`;
sessionStorage.setItem(key, JSON.stringify(userInfo));
```
> 最後測試時間：2022/12/14

## 限制
- 因為此腳本僅繞過前端的身份驗證，因此可能會導致無法使用儲存班級紀錄、測驗等功能。  
- 部分工具會定時重置資料，因此需重新執行腳本。
- 現有的一些腳本有些地方的迴避方式不是很好，在未來或許可以用其他方式執行腳本來取代現行做法。

---

The script was made by [SiongSng](https://github.com/SiongSng) | 此腳本由 [菘菘](https://github.com/SiongSng) 製作  
版權所有 © 2022 [菘菘](https://github.com/SiongSng)。 保留所有權利。  
Copyright © 2022 [SiongSng](https://github.com/SiongSng). All rights reserved.