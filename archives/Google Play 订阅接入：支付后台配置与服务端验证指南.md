# Google Play 订阅接入：支付后台配置与服务端验证指南

本文将详细介绍如何在 Google Play 中配置订阅支付后台，并完成服务端验证。通过合理的步骤和配置，确保支付流程顺畅且安全合规。

## 一、Google 开发者平台回调配置

### 1. 配置回调地址
首先，进入 [Google Cloud Pub/Sub 主题列表](https://console.cloud.google.com/projectselector2/cloudpubsub/topic/list)，新建一个主题。

![新建主题](https://bbtdd.com/img/80192864278.webp)

![主题创建成功](https://bbtdd.com/img/4850136529467470.webp)

### 2. 创建订阅
输入回调服务端地址，用于在付款成功后接收通知。

![创建订阅](https://bbtdd.com/img/848000128.webp)

### 3. 设置权限
必须添加 `google-play-developer-notifications@system.gserviceaccount.com` 权限，以确保回调通知能够正常发送。

参考官方文档：[Google Play Billing 配置指南](https://developer.android.com/google/play/billing/getting-ready?authuser=1&hl=zh-cn#configure-rtdn)

![权限设置示例](https://bbtdd.com/img/16430352.webp)

### 4. 检查通知配置
在 [Google Play Console](https://play.google.com/console/) 中，通过 *创收设置* 检查是否能够接收到 Google 的通知。

![创收设置](https://bbtdd.com/img/76855208714617.webp)

---

## 二、API 项目配置与验证流程

### 1. 创建 API 项目
注意：支付使用的 API 项目与登录项目不同。

### 2. 开启 Google Play Android Developer API
在 Google API 控制台中，搜索并开启 *Google Play Android Developer API*。

### 3. 设置 OAuth 同意屏幕
配置授权登录页面，填写必填项即可。

### 4. 创建 Web 应用 OAuth 客户端 ID
创建后，获取 `clientId` 和 `clientSecret`，这些将在后续步骤中使用。

### 5. 关联 API 项目
在 Google Play 开发者后台的 *API 权限* 菜单中，关联刚刚创建的 API 项目。一个 Google Play 账号只能关联一个 API 项目。

### 6. 获取授权 Code
通过授权页面，使用 Google 开发者账号登录，获取 `code`。

### 7. 换取 RefreshToken
使用 `code` 换取 `refreshToken`。**注意**：`refreshToken` 仅在首次请求时返回，必须永久保存。

### 8. 刷新 AccessToken
使用 `refreshToken` 获取 `accessToken`，用于查询订单状态。`accessToken` 有效期为 5 分钟。

### 9. 查询订单状态
通过 `accessToken` 查询订单状态，确保支付流程顺利完成。

---

## 三、常见问题与注意事项

1. **支付测试条件**  
   Google 应用需处于封闭测试状态，并审核通过后才能支付。内部测试可能无法满足支付条件。

2. **API 项目区分**  
   支付与登录使用的 API 项目相互独立，支付项目可查询关联账号中的所有应用订单。

3. **RefreshToken 过期问题**  
   - API 项目同意屏幕为测试状态时，有效期为 7 天。  
   - `refreshToken` 若 6 个月未使用，将失效。  
   - 授权超过 50 个刷新令牌时，最早的令牌将失效。  
   - 修改开发者账号密码可能导致令牌失效。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)