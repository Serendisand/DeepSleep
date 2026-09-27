# DeepSleep

把 DeepSeek 的 logo 改成 DeepSleep 的浏览器扩展。

## 起因

某天闲着没事按了个 F12，本来只是想看看页面结构，结果一眼扫到 logo 那行——好家伙，不是图片，不是字体，是一堆 `<path>` 硬拼出来的矢量字母。

当时就来了兴趣：既然字母是 path 拼的，那我是不是也能自己拼一个？

于是就有了这个扩展。把 `deepseek` 改成 `deepsleep`，字母全部复用原 SVG 里的矢量轮廓，连那只小鲸鱼图标都原样保留，风格一模一样。

## 效果

原 logo：

```
🐋deepseek 
```

替换后：

```
🐋deepsleep 
```


## 安装

1. 下载或克隆本仓库到本地
2. 打开浏览器扩展管理页
   - Chrome：`chrome://extensions`
   - Edge：`edge://extensions`
3. 打开右上角「开发者模式」
4. 点击「加载已解压的扩展程序」
5. 选择本项目文件夹
6. 刷新 DeepSeek 页面即可看到效果

## 文件结构

```
deepsleep-logo/
├── manifest.json   扩展配置
├── content.js      核心逻辑：匹配并替换 logo
└── style.css       样式
```

## 工作原理

1. 监听页面 DOM，查找 `viewBox="0 0 143 23"` 的 SVG 元素
2. 命中后，从原 SVG 里提取出 `d`、`e`、`p`、`s`、`l` 这几个字母的 path 数据
3. 用 `getBBox()` 动态测量每个字母的真实宽度和起点
4. 按 `deepsleep` 的顺序重新排布，逐个 `translate` 到目标位置
5. 保留原图标（鲸鱼）和颜色变量 `--dsw-alias-brand-primary`
6. 通过 `MutationObserver` 处理动态加载的 logo，页面切换也不怕

字母来源：

| 目标字母 | 来源 |
|----------|------|
| d | 原 d |
| e | 原 e |
| p | 原 p |
| s | 原 s |
| l | 原竖线路径 |

## 自定义

想改成别的词，编辑 `content.js` 顶部的 `ORDER` 数组即可，例如：

```js
const ORDER = ["d", "e", "e", "p", "s", "l", "e", "e", "p"];
```

可用字母：`d`、`e`、`p`、`s`、`l`。

想调大小就改 `SCALE`：

```js
const SCALE = 1.15;
```

想调字距就改 `GAP`：

```js
const GAP = 0.5;
```

## 兼容性

- Chrome / Edge（Manifest V3）
- Firefox 需调整 `manifest.json` 为 V2 格式

## 说明

本项目纯属娱乐，源于一次无聊的 F12，与 DeepSeek 官方无关。

## License

MIT
