# ys-webrtc-sdk-ui

[中文](./README_zh-CN.md) | [English](./README.md) 

本项目抽离了 PBX web 通话组件 ui 及通话逻辑，使用集成好的ui组件无需额外编码。

通话核心逻辑基于 [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/README_zh-CN.md)集成。

## Install

```bash
npm install ys-webrtc-sdk-ui --save
```

## Getting started

初始化sdk，渲染UI组件。
```js
import { init } from 'ys-webrtc-sdk-ui';
const container = document.getElementById('container');
// 初始化
init(container, {
    username: '1000',
    secret: 'sdkshajgllliiaggskjhf',
    pbxURL: 'https://192.168.1.1:8088'
}).then(data => {
// 可以在这里获取暴露出的实例，处理更多业务
const { phone, pbx, destroy, on } = data;
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
        username: '1000',
        secret: 'sdkshajgllliiaggskjhf',
        pbxURL: 'https://192.168.1.1:8088'
    })
        .then(data => {
            const { phone, pbx } = data;
        })
        .catch(error => {
            console.log(error);
        });
</script>
```

## API

通过 init 方法进行初始化，初始化成功后会的到两个实例化后的 Operator 对象：phone 和 pbx。
- phone 对象包含通话相关的方法和属性，如：call、hangup 等方法。
- pbx 对象包含 PBX 的相关方法和属性，如：查询 cdr。
- 更多方法和属性请查看 [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme)。

init 函数需要两个参数 container和rtcOption。
- container 类型是 HTMLElement，用于渲染 UI 组件。
- rtcOption 为初始化选项。

### rtcOption 参数

| 属性名 | 类型 | 是否必填 | 说明 |
| --- | --- | --- | --- |
| username | string | 必填 | 分机号。 |
| secret | string | 必填 | 登录签名，通过 OPEN API 获取，[获取签名流程](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/docs/zh-CN/CreateSign.md)。 |
| pbxURL | URL \| string | 必填 | PBX 的访问地址可以为 URL 对象，地址要求包含协议以及端口如：https://192.168.1.1:8088或 https://xx.xxx.com。 |
| enableLog | boolean | 可选 | 是否开启日志输出以及错误日志上报至 PBX，默认开启。 |
| reRegistryPhoneTimes | number | 可选 | 尝试重新连接 sip 服务次数，默认无限制。 |
| deviceIds | { cameraId?: string; microphoneId?: string; speakerId:string; volume:number } | 可选 | 指定音视频输入设备 id，包含摄像头 id 、麦克风 id、扬声器 id；volume为通话、来电、拨号盘音量，音量范围0-1直接的浮点数，默认为0.6。 |
| incomingOption | { style?: React.CSSProperties; class?: string;  } | 可选 | 调整Incoming组件层级 |
| dialPanelOption | {  style?: React.CSSProperties; class?: string; } | 可选 | 调整Incoming组件层级 |
| sessionOption | [SessionOption](#session-option) | 可选 | 调整通话窗口组件位置和尺寸 |
| hiddenIncomingComponent | boolean | 可选 | 隐藏来电弹屏组件 |
| hiddenDialPanelComponent | boolean | 可选 | 隐藏拨号盘组件 |
| disableCallWaiting | boolean | 可选 | 是否禁用callWaiting，为true时不使用pbx callWaiting值且只处理单通电话。|

### Types

#### SessionOption
```ts
type SessionOption = {
    sessionSetting?: {
        width?: number;
        height?: number;
        miniWidth?: number;
        miniHeight?: number;
        x?: number;
        y?: number;
    };
    style?: React.CSSProperties;
    class?: string;
}
```

