## 项目说明
基于`Vue3` + `Typescript`开发

自己写了个New Tab页面，主要是因为想在插件功能太多了，我想用的只有切换不同的搜索引擎进行搜索，于是花点时间，写了个这个极其简单的New Tab项目。

目前只支持Google,Bing,Bilibili（别问，问就是我常用这三个）

## 如何使用

先克隆项目：

```bash
git clone https://github.com/Tuning-Luna/MyNewTab
```

然后找到里面的`dist`文件夹，再把目录下的`manifest.json`文件拷贝至dist文件夹下（为了被识别成Chrome插件）

然后进入Chrome浏览器，进入插件页面打开开发者模式，把`dist`文件夹导入即可使用。

按下q触发搜索框，使用上下箭头进行搜索引擎切换。

## 如何开发
先克隆项目：

```bash
git clone https://github.com/Tuning-Luna/MyNewTab
```

**确保电脑上有Nodejs环境**，如果没有请安装Nodejs环境

开发模式：
```bash
npm run dev
```

打包模式：
```bash
npm run build
```