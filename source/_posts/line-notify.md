---
title: Line 發送通知 & 綁定聊天室
date: 2022-10-25 11:42:03
categories: CICD
tags: [Line]
---

<br/>

## 1. 登入 Line Notify
https://notify-bot.line.me/
![Line Notify](line-notify.png "lineNotifyLogo")

## 2. 進入個人頁面
https://notify-bot.line.me/my/
<br>

## 3. 發行存取權杖
點擊"發行權杖"
![發行權杖](public-token.png "publicToken")
<br>

## 4. 設定功能
1. 填寫名稱: 傳送訊息時的顯示名稱
2. 選取要接收通知的聊天室
![設定功能](choose-room.png "chooseRoom")
<br>

## 5. 產生權杖
將權杖複製起來, 此權杖為該聊天室專用
![產生權杖](copy-token.png "copyToken")
<br>

## 6. 將 Line Notify 邀請至該聊天室
![邀請 Line Notify](invite-line-notify.png "inviteLineNotify")
<br>

## 7. 藉由 curl 傳送通知
```bash
# Linux
curl -X POST https://notify-api.line.me/api/notify -H 'Authorization: Bearer <your-token>' -F "message=妳好～"

# Jenkins
stage('Notify') {
  steps {
    sh """ 
      curl -X POST https://notify-api.line.me/api/notify \
      -H 'Authorization: Bearer <your-token>' \
      -F "message=妳好～"
    """ 
  }
}
```
<br>


## 8. 登登登
![在聊天室接收通知吧 ( 123為設定功能時填寫的名稱 )](line-notify-send-message.png "lineNotifySendMessage")
