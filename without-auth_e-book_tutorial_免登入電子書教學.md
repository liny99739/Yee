![Visits_Count](https://img.shields.io/badge/dynamic/json?label=Visits%20Count&query=value&url=https%3A%2F%2Fapi.countapi.xyz%2Fhit%2Fbaa4f015258d0312f627b5659d93d72e%2Fvisits)

# 教學用電子書免登入破解教學

**使用前請務必閱讀 [免責聲明](https://gist.github.com/SiongSng/baa4f015258d0312f627b5659d93d72e#%E5%85%8D%E8%B2%AC%E8%81%B2%E6%98%8E)**

## 免責聲明
### 請勿將本腳本作為抄答案、侵權等惡意用途，使用本腳本者，請*自行承擔*所有後果與風險。

## 簡介
此腳本用於繞過台灣主要課本/習作出版社電子書的前端身份驗證，達成不需要教師帳號即可使用電子書。

## 開發緣由
原本是因為開發者忘記帶課本，但又想要查閱課本的資料，心血來潮研究看看電子書的驗證設計。  
開發這個不是希望拿去抄答案，是希望讓真正需要用的人可以用到，也希望各家出版社能提供一種學生與家長的版本，就是只能瀏覽但不能顯示解答或者專為學習者設計，就可以完美解決這些問題。

## 如何使用
這邊示範翰林版如何使用，其他出版社以此類推
首先先前往要使用的電子書網站 (下方有連結)，接著在瀏覽器的開發者頁面 (F12) 中的主控台 (Console) 輸入以下腳本，最後重新載入網頁即可迴避登入。  
> 這邊是以 Google Chrome 的環境作為示範，Firefox/Edge/Safari 大同小異。  

![image](https://user-images.githubusercontent.com/48402225/188836801-3688329c-fe73-4c1b-b762-ba0d7d4a50c4.png)

## 腳本

### ✅ 康軒
連結：[康軒網頁媒體盒](https://digitalmaster.knsh.com.tw/downloader/box-web/index.html)  
```js
localStorage.setItem("loginAccount", "mockAccount"); // 設定一個假的帳號
localStorage.setItem("uuid", "mockUUID"); // 設定假的 UUID
```
> 最後測試時間：2022/10/18

### ✅ 南一
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
> 最後測試時間：2022/11/12  
> token 由 @jackychiu0207 提供

### ✅ 翰林
連結：[翰林行動大師電子書](https://edisc3.hle.com.tw/edisc_v3/ebook_v2023.html)  

```js
let time = new Date().getTime().toString();
localStorage.setItem("last_signinX_v2023", time); // 將帳號登入日期設定為現在，避免被判定為過期
localStorage.setItem("roleX_v2023", "老師"); // 設定身份為老師
localStorage.setItem("emailX_v2023", "test@test.com"); // 由於翰林電子書會驗證是否有設定 email，如果有設定才能使用
localStorage.setItem("tokenX_v2023", "eyJhbGciOiJSUzI1NiIsImtpZCI6Ijg1NzgwNWYxZGQ3ZmE5YTZiNTI3ZjQ0ZWNmZmJkNDhjIiwidHlwIjoiSldUIn0.eyJuYmYiOjE2NjYyNTk4NzEsImV4cCI6MTY2ODkzODI3MSwiaXNzIjoiaHR0cHM6Ly9pZC5obGUuY29tLnR3IiwiYXVkIjpbImh0dHBzOi8vaWQuaGxlLmNvbS50dy9yZXNvdXJjZXMiLCJhcGkxIiwiSWRlbnRpdHlTZXJ2ZXJBcGkiLCJoYW5saW4tYXBpIl0sImNsaWVudF9pZCI6ImpzIiwic3ViIjoiZGJiYmEwNmEtNWNkNy00NTI5LWI2MjEtOTBlYjdhMGIxOWZlIiwiYXV0aF90aW1lIjoxNjY2MjU5ODcwLCJpZHAiOiJsb2NhbCIsIkFzcE5ldC5JZGVudGl0eS5TZWN1cml0eVN0YW1wIjoiNURHN1ZSWVVWRUdUSjJVQ1czU0FDRkpBT1NHM0RONEIiLCJyb2xlIjpbIuiAgeW4qyIsIuiAgeW4qyJdLCJlbWFpbCI6WyJraW5tYTE1OTg3NTMyQGdtYWlsLmNvbSIsImtpbm1hMTU5ODc1MzJAZ21haWwuY29tIl0sImZhbWlseV9uYW1lIjoi576FIiwiZ2l2ZW5fbmFtZSI6IuWFg-iyniIsIm5hbWUiOiLnvoXlhYPosp4iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwicHJlZmVycmVkX3VzZXJuYW1lIjoi576F5YWD6LKeIiwidXNlcl9kb21haW4iOiJlZHUiLCJzY29wZSI6WyJvcGVuaWQiLCJwcm9maWxlIiwiYXBpMSIsIklkZW50aXR5U2VydmVyQXBpIiwiaGFubGluLWFwaSIsIm9mZmxpbmVfYWNjZXNzIl0sImFtciI6WyJwd2QiXX0.So0Fcvd-a_BlnQcgcmO7vXTxlCJ_AnIEPPwpoHHpqc2cP3fBCGrY496R1q4J9j2E9sYUahxeYu7M3RMhPS_79JiEq8EWcSUvNxJASwAgvmek_HxWS2sgPZbvFkCJ1zYXfqHpbUaRfeqNPZyB3Yno94OYU4nl5f0gRzwUf2kGiyM2XhTO5EQZUCXGDJfqNmBlnwL45MwlQ_l_sRSYFNllda37nTECse91Qe1DeYKCm1Z9s8MerCCnmJgpjNsKOPodvbz8ynUT7qbU2IDldb8z8h0mtI9DbW8tuG63c-Nqyr2ZHPXT5aIaWtYUUBgFrVakVW-nI0kv5cEYj8grUyuZFg") // 設定身分驗證用的 token
```
> 最後測試時間：2022/11/12  
> token 由 @jackychiu0207 提供

## 限制
- 因為此腳本僅繞過前端的身份驗證，因此可能會導致無法使用儲存班級紀錄、測驗等功能。  
- 翰林版電子書每天會自動重置資料，因此需重新執行腳本。
- 南一版電子書因設計較為嚴謹，可能在未來此破解方法將無法使用，需尋找更好的解決方案。

---

The script was made by [SiongSng](https://github.com/SiongSng) | 此腳本由 [菘菘](https://github.com/SiongSng) 製作  
版權所有 © 2022 [菘菘](https://github.com/SiongSng)。 保留所有權利。  
Copyright © 2022 [SiongSng](https://github.com/SiongSng). All rights reserved.