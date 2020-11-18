# 导入SDK

## 环境要求

| name | version |
| - | -|
| Android Studio | 2.0+ |


## 导入 Module

>1. File ->new-> import Module
>2. settings.gradle 中加入 include ':lib_module'
>3. build.gradle 中加入  api project(path: ':lib_module')

# 初始化数

1. 请使用开发功能之前从卓朗云开发者控制台注册得到的 Appkey，通过 TimLib 的单例，传入 initWithAppKey: 方法，初始化 SDK。
2. 开发者在使用卓朗云 SDK 所有功能之前，开发者必须先调用此方法初始化 SDK。 在 App 的整个生命周期中，开发者只需要将 SDK 初始化一次。
```objc
TimLib.initWithAppKey(application: Application, appId: String, uploadMethod: String, debug: Boolean, host: String)
```
### 参数说明

| 参 | 类型 | 必填 | 说明 |
| - | - | - | - |
| application | Application | 是 | 本应用的Application |
| appId | String | 是 | 后台提供的appId |
| uploadMethod | String | 是 | 上传方式 默认"local" |
| debug | Boolean | 是 | BuildConfig.DEBUG |
| host | String | 是 | 链接地址 |

# 连接卓朗云

```objc
TimLib.connectWithTokenAndUserId(userId: String, token: String, deviceToken: String,brand: String, operateListener: MessageOperateListener)
```
### 参数说明

| 参数 | 类型 | 必填 | 说明 |
| - | - | - | - |
| userId | String | 是 | 用户ID |
| token | String | 是 | 用户令牌 |
| deviceToken | String | 是 | 设备Token 推送用 |
| brand | Boolean | 是 | 推送方式 Android 目前是"Code.BrandType.umeng" |
| operateListener | MessageOperateListener | 是 | 链接的回调，成功or失败 |

# 获取会话列表

1. 获取会话列表
```objc
TimLib.getConversationList()
```

# 发送消息

1. 所有消息类型均集成自MC_ContentMessage。
2. 调用 TimLib 中 sendMessage 方法发送。
3. `success` 即发送成功回调，会返回消息的 `messageId`。
4. `error` 即发送失败回调，会返回消息的 `messageId` 和 发送失败的错误码 `errorCode`。
```objc
  TC_TextMessage textMessage = new TC_TextMessage();
        textMessage.setMentionAll(true);
        textMessage.setMention(true);
        textMessage.setMentionedMembers(memberList);
        textMessage.setContent("@所有人 " + content.getText().toString());
TimLib.sendMessage(targetId, conversationType, textMessage, new MessageOperateListener())
```

# 接收消息

1. 设置接收消息监听。
```objc
// 设置消息接收监听
TimLib.INSTANCE.registerReceivedMessageListener(new ReceivedMessageListener() {
 public void onReceivedMessage(Message message){}
});

```

# 获取历史消息

1. 从本地数据库中获取历史消息。
2. oldestMessageId 从messageId的一条消息开始获取。
3. count 即获取消息的数量。
```objc
TimLib.getHistoryMessages(targetId: String, conversationType: String, oldestMessageId: String, count: Int);
```

# 断开连接

1. 断开连接后不会再收到消息。
2. 在下次连接成功后，会收取上次离线后的消息，离线消息最多可以保存 7 天。
```objc
   TimLib.logout()
```
