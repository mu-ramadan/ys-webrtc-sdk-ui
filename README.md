# ys-webrtc-sdk-ui

## 本项目抽离了 PBX web 通话组件 ui 及其 通话逻辑
使用集成好的ui组件无需额外编码。
## 使用
```bash
npm install ys-webrtc-sdk-ui --save
```
初始化sdk，渲染UI组件。
```js
import { init } from 'ys-webrtc-sdk-ui';
const container = document.getElementById('container');
// 初始化
init(container, {
    pbxUrl: 'https://192.168.1.1:8088', // 或者fqdn地址
    secret: '由open api创建的签名',
    username: '分机号',
    deviceInfo: {
        microphoneId: 'communications',
        cameraId: 'default',
        speakerId: 'communications'
    }
}).then(data=>{
// 可以在这里获取暴露出的实例，处理更多业务
const { phone, pbx, destroy } = data;
    // ...
}).catch(err=>{
    console.log(err)
})
```

## 标签方式引用
```html
<!-- 加载样式 -->
<link rel="stylesheet" href="ys-webrtc-sdk-ui.css">

<div id="test"></div>
<!-- 加载UI集成 SDK -->
<script src="./ys-webrtc-sdk-ui.js"></script>
<script>
    const test = document.getElementById('test');
    // 加载成功后通过YSWebRTCUI对象进行初始化
    window.YSWebRTCUI.init(test, {
        pbxUrl: 'https://' + url,
        secret: this.secret,
        username: name,
        deviceInfo: {
            microphoneId: 'communications',
            cameraId: 'default',
            speakerId: 'communications'
        }
    })
        .then(operator => {
            // 获得 PhoneOperator和PBXOperator 实例
            const { phone, pbx } = operator;
        })
        .catch(error => {
            console.log(error);
        });
</script>
```




