# 教學用電子書免登入破解教學

**使用前請務必閱讀 [免責聲明](https://gist.github.com/SiongSng/baa4f015258d0312f627b5659d93d72e#%E5%85%8D%E8%B2%AC%E8%81%B2%E6%98%8E)**

## 簡介
此腳本用於繞過台灣主要課本/習作出版社電子書的前端身份驗證，達成不需要教師帳號即可使用電子書。

## 開發緣由
原本是因為開發者忘記帶課本，心血來潮研究看看電子書的驗證設計，除了南一有透過伺服器再加以驗證，其他的電子書都是直接在前端判斷。  
開發這個不是希望拿去抄答案，是希望讓真正需要用的人可以用到，也希望電子書能提供一種學生與家長的版本，就是只能瀏覽但不能顯示解答，就可以完美解決這些問題。

## 如何使用
首先先前往要使用的電子書網站，接著在瀏覽器的開發者頁面 (F12) 中的主控台 (Console) 輸入以下腳本，重新載入網頁即可迴避登入。

## 腳本

### 翰林
連結：[翰林行動大師電子書](https://edisc3.hle.com.tw/edisc_v3/home.html)  

```js
let time = new Date().getTime().toString();
localStorage.setItem("last_signinX", time); // 將帳號登入日期設定為現在，避免被判定為過期
localStorage.setItem("roleX", "老師"); // 假冒身份為老師
localStorage.setItem("emailX", "test@test.com"); // 由於翰林電子書會驗證是否有設定 email，如果有設定才能使用
```
> 最後測試成功：2022/4/6

### 康軒
連結：[康軒網頁媒體盒](https://digitalmaster.knsh.com.tw/downloader/box-web/index.html)  
```js
localStorage.setItem("loginAccount", "mockAccount"); // 設定一個假的帳號
localStorage.setItem("uuid", "mockUUID"); // 假的 UUID
```
> 最後測試成功：2022/4/2

### 南一
連結：[OneBook 南一電子書](https://reader.oneclass.com.tw/bookshelf)  
```js
let mockToken = JSON.stringify({
    "code": "SUCCESS",
    "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbXlhY2NvdW50Lm5hbmkuY29vbC8iLCJzdWIiOiJ1c2Vycy9zaW9uZ3NuZyIsImZyb20iOiJOYW5pIiwidXNlcm5hbWUiOiJzaW9uZ3NuZyIsImVtYWlsdmFsaWQiOnRydWUsIm1vYmlsZXZhbGlkIjpmYWxzZSwiZW1haWwiOiJycnQ0Njc3NzhAZ21haWwuY29tIiwidWlkIjoiNGVlZDQzZTAtYzUwNi0xMWViLThhZWQtYjM0Y2EzZDExZTcwIiwianRpIjoiNmU3MGZjN2UtN2EzOC00ZWU2LTgxYWQtOTU2YzcyYzgxNGJkIiwiaWF0IjoxNjQ1NDQwMDQxLCJleHAiOjE2NTA2MjQwNDF9.9XUHfpcxWPxUl9PEUXcg-YQYGOHv16iyoVw-cbX9dyM"});

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
> 最後測試成功：2022/4/6

## 限制
- 因為此腳本僅繞過前端的身份驗證，因此可能會導致無法使用儲存班級紀錄、測驗等功能。  
- 翰林版電子書每天會自動重置資料，因此需重新執行腳本。
- 南一版電子書因設計較為嚴謹，可能在未來此破解方法將無法使用，需尋找更好的解決方案。

## 免責聲明
此腳本僅用於技術用途，請勿拿去作為任何惡意用途，希望您是拿去學習/對答案，而不是抄答案，使用本腳本者，請自行承擔所有後果與風險。

Made by [SiongSng](https://github.com/SiongSng)  
由 [菘菘](https://github.com/SiongSng) 製作