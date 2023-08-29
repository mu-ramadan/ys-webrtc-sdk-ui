# ys-webrtc-sdk-ui

[中文](./README_zh-CN.md) | [English](./README.md) 

Yeastar WebRTC SDK separates the PBX web calling component UI and call logic. 

**Yeastar WebRTC SDK UI** is pre-integrated UI components that require no additional coding, while the core logic for the calling functionality is based on the integration of  [Yeastar WebRTC SDK Core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme).

## Installation

```bash
npm install ys-webrtc-sdk-ui --save
```

## Getting started

Initialize the Yeastar WebRTC SDK UI and render the UI components,
```js
import { init } from 'ys-webrtc-sdk-ui';
const container = document.getElementById('container');
// Initialization
init(container, {
    username: '1000',
    secret: 'sdkshajgllliiaggskjhf',
    pbxURL: 'https://192.168.1.1:8088'
}).then(data => {
// Obtain the exposed instances for additional business needs
const { phone, pbx, destroy, on } = data;
    // ...
}).catch(err=>{
    console.log(err)
})
```

## Use HTML tags to reference the Yeastar WebRTC SDK UI

```html
<!-- Loading styles -->
<link rel="stylesheet" href="ys-webrtc-sdk-ui.css">

<div id="test"></div>
<!-- Loading Yeastar WebRTC SDK UI-->
<script src="./ys-webrtc-sdk-ui.js"></script>
<script>
    const test = document.getElementById('test');
    // Initialize Yeastar WebRTC SDK UI with the 'YSWebRTCUI' object. 
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

Initialize Yeastar WebRTC SDK UI using the `init` method. Upon successful initialization, two instantiated Operator objects are returned: 

+ **phone**: The object contains methods and attributes related to the call handling, such as 'call', 'hangup', and others.
+ **pbx**: The object contains methods and attributes related to the PBX operations, such as querying  CDR.

	**Note**: For more information, see [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme).

The `init` function requires two parameters:

+ **container**: Applicable for rendering UI components, and the data type is 'HTMLElement'.
+ **rtcOption**: Initialization options.

### rtcOption Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| username | string | Yes | Extension number. |
| secret | string | Yes | Login signature, which can be obtained using OPEN API. For more information, see [Obtain a Server-side Signature](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/docs/CreateSign.md). |
| pbxURL | URL \| string | Yes | The URL for accessing your PBX system, including the transfer protocol and the port number.<br />For example, https://192.168.1.1:8088 or https://xx.xxx.com. |
| enableLog | boolean | No | Whether to enable log output and report error logs to PBX. This feature is enabled by default. |
| reRegistryPhoneTimes | number | No | Define the number of attempts to reconnect to the SIP service. By default, it is unlimited. |
| deviceIds | { cameraId?: string; microphoneId?: string; speakerId:string; volume:number } | No | Specify the IDs of the audio and video input devices, including the camera ID, microphone ID, and speaker ID.<br />Volume refers to the volume level for calls, incoming call ringtones, and keypad tones, ranging from 0 to 1. The default value is 0.6. |
| incomingOption | { style?: React.CSSProperties; class?: string;  } | No | Adjust the styling of the 'incoming call component'. |
| dialPanelOption | {  style?: React.CSSProperties; class?: string; } | No | Adjust the styling of the 'dial panel component'. |
| sessionOption | [SessionOption](#session-option) | No | Adjust the position and size of the 'call window component'. |
| hiddenIncomingComponent | boolean | No | Hide the 'incoming call component'. |
| hiddenDialPanelComponent | boolean | No | Hide the 'dial panel component'. |
| disableCallWaiting | boolean | No | Whether to disable call waiting. When setting this value to `true`, the PBX call waiting value does NOT take effect and PBX only handles single calls. |

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

