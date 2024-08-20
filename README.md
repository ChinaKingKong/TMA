# 电报小程序(telegram-mini-app)开发实践

### 前言
Telegram Mini Apps（TMA）是在 Telegram 消息传递应用程序内运行的 Web 应用程序。它们是使用 Web 技术构建的 —— HTML、CSS 和 JavaScript。Telegram Mini Apps 可用于创建 DApps、游戏以及其他可以在 Telegram 内运行的应用程序类型。

2022年4月Telegram的MiniApp（之前为Web App，6.0版后改名为Mini App）上线，Mini Apps（简称 TMAs，中文名：小程序）很可能会变成一个类似于微信小程序的平台，使得Telegram 更接近一个“超级应用”。目前，电报小程序推出不久，版本还在快速迭代中，开发人员也较少，但电报庞大的用户群基础很可能会产生大量的小程序。


### 1、Vite+React+Typescript 开发电报小程序
直接运行以下命令创建带有 TypeScript 支持的 React 项目：

使用npm
```
# npm 7+, extra double-dash is needed:
npm create vite my-react-telegram-mini-app -- --template react-ts
```
使用yarn
```
# or yarn
yarn create vite my-react-telegram-mini-app --template react-ts
```

进入项目工程
```
# this will change the directory to recently created project
 cd my-react-telegram-mini-app&&code .
```

开始项目的开发模式，在终端运行以下命令：

使用npm
```
# npm
npm install
npm run dev --host
```
使用yarn
```
# or yarn
yarn
yarn dev --host
```

添加 @twa-dev/sdk 到工程中

使用npm
``` 
npm install @twa-dev/sdk
```
使用yarn
```
yarn add @twa-dev/sdk
```

打开 /src/main.tsx 文件并添加以下内容：
```
import WebApp from '@twa-dev/sdk'

WebApp.ready();
``` 

WebApp.ready() - 是一个方法，向 Telegram 应用程序通知小程序已准备好显示。建议尽可能早地调用此方法，一旦加载了所有必要的接口元素。一旦调用此方法，加载的占位符将被隐藏，小程序将被显示。

添加小程序元素与用户交互。进入 src/App.tsx，这里我们添加带有弹框的按钮。
```
import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'

import WebApp from '@twa-dev/sdk'

function App() {
  const [count, setCount] = useState(0)

  return (
    <>
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
      </div>
        {/* Here we add our button with alert callback */}
      <div className="card">
        <button onClick={() => WebApp.showAlert(`Hello World! Current count is ${count}`)}>
            Show Alert
        </button>
      </div>
    </>
  )
}

export default App
```

### 2、Vite+Vue+Typescript 开发电报小程序
直接运行以下命令创建带有 TypeScript 支持的 Vue 项目：
使用npm
```
# npm 7+, extra double-dash is needed:
npm create vite my-vue-telegram-mini-app -- --template vue-ts
```
使用yarn
```
# or yarn
yarn create vite my-vue-telegram-mini-app --template vue-ts
```

进入项目工程
```
# this will change the directory to recently created project
 cd my-vue-telegram-mini-app&&code .
```

开始项目的开发模式，在终端运行以下命令：

使用npm
```
# npm
npm install
npm run dev --host
```
使用yarn
```
# or yarn
yarn
yarn dev --host
```

安装vue-tg库
```
npm i vue-tg

```

vue-tg 官方文档说明
https://github.com/deptyped/vue-telegram?tab=readme-ov-file

在 index.html 中添加电报js库的引用
```
<script src="https://telegram.org/js/telegram-web-app.js"></script>
```

在/src/components/HelloWorld.vue中添加组件使用代码：
[HelloWorld.vue 代码](/my-vue-telegram-mini-app/src/components/HelloWorld.vue)

### 3、创建并配置小程序
打开 Telegram 应用程序或网页版本。
在搜索栏中搜索 @BotFather 或打开链接 https://t.me/BotFather。
通过点击 START 按钮来开始与 BotFather 的对话执行创建以及配置步骤。

1、创建Bot，Bot与App是一对多的关系，一个Bot可以创建多个App 如下图：

![alt text](images/P6.png)

2、创建App，这里需要提供一张640x360尺寸的Logo图片，如下图：

![alt text](images/P7.jpg)

需要注意的是，现在小程序中必须使用https协议才能部署运行。

### 4、本地调试小程序
使用以下工具来Debug小程序：

#### Android
在您的设备上启用USB调试。
在Telegram设置中，向下滚动，按住版本号两次。
在调试设置中选择启用WebView调试。
将手机连接到计算机，并在Chrome中打开chrome://inspect/#devices - 当您在手机上启动小程序时，您会在那里看到它。

#### Windows和Linux上的Telegram桌面版
下载并启动[Telegram桌面版的Beta版本](https://desktop.telegram.org/changelog#beta-version)。
转到设置 > 高级 > 实验设置 > 启用WebView检查。
右键单击WebView并选择检查元素。

#### macOS上的Telegram
下载并启动[Telegram macOS的Beta版本](https://install.appcenter.ms/users/keepcoder/apps/Telergam-Beta-Updated/distribution_groups/public)
快速点击5次设置图标以打开调试菜单，并启用调试Web App。
右键单击小程序并选择检查元素。

#### iOS上的Telegram
下载最新的App，然后安装如上步骤进行配置操作即可。

然后这里我们需要使用ngrok做内网穿透，将本地程序端口映射到外网。这样做的目的仅是为了给本地端口号使用https协议的网址。

进入官网 https://ngrok.com/ 注册账号，然后按照步骤安装即可。

![alt text](images/P8.png)

映射端口：

![alt text](images/P1.png)

然后进入电报修改之前绑定的Web App URL, 如下图：

![alt text](images/P9.png)

最终运行效果图：

![alt text](images/P10.png)

至此我们从小程序代码创建到本地调试的操作步骤就基本跑通了，让我们开启电报小程序的开发之旅吧！有问题欢迎讨论交流。
