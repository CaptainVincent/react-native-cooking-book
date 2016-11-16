# 部署到裝置

### iOS
* 取得開發者帳號
* Xcode 加入帳號
* Xcode > Identity 選 fix issue 最後會需要登入裝置的 UUID 取得憑證
* 登記完成後會出現在可部署的裝置列表上 (連接裝置後, 會與模擬器在同一個清單)

> 書中提及 AppDelegate.m 要修改 jsCodeLocation 成 server 端的實體 IP (取代 localhost), 不過目前的修改方式已經跟書中的不同了, 之後再補充。

