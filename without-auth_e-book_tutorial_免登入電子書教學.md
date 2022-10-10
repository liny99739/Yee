![Visits_Count](https://img.shields.io/badge/dynamic/json?label=Visits%20Count&query=value&url=https%3A%2F%2Fapi.countapi.xyz%2Fhit%2Fbaa4f015258d0312f627b5659d93d72e%2Fvisits)

# 教學用電子書免登入破解教學

**使用前請務必閱讀 [免責聲明](https://gist.github.com/SiongSng/baa4f015258d0312f627b5659d93d72e#%E5%85%8D%E8%B2%AC%E8%81%B2%E6%98%8E)**

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

### ✅ 翰林
連結：[翰林行動大師電子書](https://edisc3.hle.com.tw/edisc_v3/ebook_v2023.html)  

```js
let time = new Date().getTime().toString();
localStorage.setItem("last_signinX_v2023", time); // 將帳號登入日期設定為現在，避免被判定為過期
localStorage.setItem("roleX_v2023", "老師"); // 假冒身份為老師
localStorage.setItem("emailX_v2023", "test@test.com"); // 由於翰林電子書會驗證是否有設定 email，如果有設定才能使用
```
> 最後測試時間：2022/10/3

### ✅ 康軒
連結：[康軒網頁媒體盒](https://digitalmaster.knsh.com.tw/downloader/box-web/index.html)  
```js
localStorage.setItem("loginAccount", "mockAccount"); // 設定一個假的帳號
localStorage.setItem("uuid", "mockUUID"); // 假的 UUID
```
> 最後測試時間：2022/10/3

### ✅ 南一
連結：[OneBook 南一電子書](https://reader.oneclass.com.tw/bookshelf)  
```js
let mockToken = JSON.stringify({
    "code": "SUCCESS",
    "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbXlhY2NvdW50Lm5hbmkuY29vbC8iLCJzdWIiOiJ1c2Vycy9qYWNreWNoaXUwMTAxIiwiZnJvbSI6Ik5hbmkiLCJ1c2VybmFtZSI6ImphY2t5Y2hpdTAxMDEiLCJlbWFpbHZhbGlkIjp0cnVlLCJtb2JpbGV2YWxpZCI6ZmFsc2UsImVtYWlsIjoiamFja3ljaGl1MDEwMUBnbWFpbC5jb20iLCJ1aWQiOiJhZGY2MmYyMC1iMjY3LTExZWItOWE2OC1mZjU4MTI3MTY5ZDkiLCJqdGkiOiI1YmNmNjc5OC1iMzdlLTQ2OWQtOTEwMS04YTgwZjg3MTI5OTAiLCJpYXQiOjE2NjUzNzg3NTIsImV4cCI6MTY3MDU2Mjc1Mn0.dHEIVW6dUInCBnoQlghufltOhW0_Dm62J2LMGywoRzs"});

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
    
localStorage.setItem("nani_tokenInfo", mockToken);
```
> 最後測試時間：2022/10/10
> Token 由 @jackychiu0207 提供

## 限制
- 因為此腳本僅繞過前端的身份驗證，因此可能會導致無法使用儲存班級紀錄、測驗等功能。  
- 翰林版電子書每天會自動重置資料，因此需重新執行腳本。
- 南一版電子書因設計較為嚴謹，可能在未來此破解方法將無法使用，需尋找更好的解決方案。

## 免責聲明
此腳本僅用於技術用途，請勿拿去作為抄答案或其他侵權等惡意用途，使用本腳本者，請**自行**承擔**所有**後果與風險。

---

The script was made by [SiongSng](https://github.com/SiongSng) | 此腳本由 [菘菘](https://github.com/SiongSng) 製作  
版權所有 © 2022 [菘菘](https://github.com/SiongSng)。 保留所有權利。  
Copyright © 2022 [SiongSng](https://github.com/SiongSng). All rights reserved.