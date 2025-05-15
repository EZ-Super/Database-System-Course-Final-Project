# 📘 後端相關 API 文件 (User Management API)

## 基本資訊
Base URL: `/api`
資料格式：application/json
認證方式：JWT Token (Authorization: Bearer <token>)
管理員頁面認證方式 : Security Key

### 🧩 錯誤碼說明

| 狀態碼 | 說明             |
|--------|------------------|
| 200    | 請求成功         |
| 201    | 建立成功         |
| 400    | 錯誤的請求內容   |
| 401    | 未授權、驗證失敗 |
| 403    | 權限不足         |
| 404    | 資源不存在       |
| 500    | 伺服器錯誤       |
| 775    | 帳號鎖定         |



## 📥 相關 API 請求資訊

### 👥 使用者管理 API 文件 (User Management API)

#### 📌 登入使用者
* 方法 : `POST`
* 路徑 : `/api/login`
* 描述 : 驗證使用者登入
* 📥 請求格式
```json
{
  "email" : "HelloWorld@example.org",
  "password" : "HelloWorld"
}
```
✅ 成功回應 (相關JWT 會存在後端Cookie 中)
```json
{
  "success" : true,
  "status" : 200,
  "Name" : "Hello World",
  "LoginStatus" : "已登入"
}
```
❌ 錯誤回應 
```json 
{
  "success" : false ,
  "status" : 401,
  "Reason" : "Password Wrong"
}
```



#### 📌 登入次數計數
* 方法 : `GET`
* 路徑 : `/api/login_history`
* 描述 : 取得每個月的登錄紀錄
✅ 成功回應 (相關JWT 會存在後端Cookie 中)
```json
{
  "success" : true,
  "status" : 200,
  "login_history" : [
    {"month":4,"count":2}
    {"month":5,"count":1}
  ]

}
```
❌ 錯誤回應 
```json 
{
  "success" : false ,
  "status" : 500,
  "Reason" : "Server Error"
}
```





#### 📌 取得當前Cookie 
* 方法 : `GET`
* 路徑 : `/api/session`
* 描述 : 取得每個月的登錄紀錄
✅ 成功回應 (相關JWT 會存在後端Cookie 中)
```json
{
  "success" : true,
  "status" : 200,
  "reason" : "登入成功",
  "username" : "Hello World",
  "user_id" : 123,
  "mail" : "helloworld@example.com"

}
```
❌ 錯誤回應 
```json 
{
  "success" : false ,
  "status" : 401,
  "Reason" : "未登入"
}
```
