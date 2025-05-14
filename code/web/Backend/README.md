# 📘 後端相關 API 文件 (User Management API)

## 基本資訊
Base URL: `/api`
資料格式：application/json
認證方式：JWT Token (Authorization: Bearer <token>)
管理員頁面認證方式 : Security Key

## 📥 相關 API 請求資訊

### 👥 使用者管理 API 文件 (User Management API)

#### 📌 登入使用者
* 方法 : `POST`
* 路徑 : `/api/login`
* 描述 : 驗證使用者登入
* 📥 請求格式
```json
{
  - email : "HelloWorld@example.org",
  - password : "HelloWorld"
}
```
