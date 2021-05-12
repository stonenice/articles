如何使用Ant Design构建第一个应用
---

# 环境准备
## 安装nodejs
### Windows
方式1：到官网下载安装包直接按照
方式2：
```bash
scoop install nodejs
```

### Mac
``` bash
brew install node
```

### Linux

```bash
CentOS:
yum install node

Ubuntu:

apt-get install node
```

## 安装包管理器Yarn
```bash
npm install -g yarn
```

### 使用yrm工具管理yarn的镜像源
> 目前主流使用的镜像源时taobao,速度最快：yrm use taobao

由于yarn默认使用的镜像源在国外，在使用yarn进行依赖包安装时非常慢。解决方法时将默认的镜像源改为使用国内的镜像源。目前主流使用的镜像源为淘宝搭建的镜像源，配置方式为：

首先安装yrm(Yarn Registry Manager)
```bash
npm install -g yrm
```

查看可用镜像源列表
```bash
yrm ls
```

测试镜像源速度
```bash
yrm test taobao 
```

切换使用镜像源
```bash
yrm use taobao
```



```bash

# 安装包依赖
yarn install 

```

## 安装Visual Studio Code编辑器
到官网地址下载安装包

# 项目创建
在电脑上创建一个文件夹，文件夹的名字就为项目的名字，如下：
```bash
# 以项目名称创建文件夹
mkdir demo


#进入文件夹
cd ./demo

#基于umijs模板初始化项目目录
yarn create @umijs/umi-app

#安装包依赖
yarn install
```

或者也可以不用提前创建项目文件夹，直接使用命令的方式：
```bash
yarn create @umijs/umi-app demo
```

## 常用地址
umi: https://umijs.org/zh-CN/docs
AntDesign: https://ant.design/docs/react/introduce-cn
And-Mobile: https://mobile.ant.design/index-cn



# 启动项目

启动项目：

```bash
#命令参照packge.json
yarn start

#或者使用umi命令行工具
yarn umi dev

```

# 用命令创建第一个页面
> 参考文档: https://umijs.org/zh-CN/docs/cli

目前umi generate 命令仅支持page/html/tmp
可以参照：https://github.com/umijs/umi/blob/master/packages/preset-built-in/src/plugins/commands/generate/generate.ts

```bash

#标准命令是yarn umi generate 不过可以使用 yarn umi g代替

#该命令模式使用js 和 css 同时创建的文件直接放在pages目录
yarn umi g page hello


#最佳实践请使用如下命令,此时会先在pages目录下创建文件夹aboutus,使用typescript 和 less
yarn umi g page about/index --typescript --less

```

通过上面命令能快速创建一个页面，此时需要在.umirc.ts文件中配置路由

```javascript
import { defineConfig } from 'umi';

export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes: [
    { path: '/', component: '@/pages/home/index' },
    //配置新增页面路由
    { path: '/about', component: '@/pages/about/index' },
  ],
  fastRefresh: {},
});

```

【项目目录结构】

![Build Block](https://github.com/stonenice/articles/blob/main/React/images/umi-app-dirs.jpg)


## 基础页面配置
> umi工程的配置文件为.umirc.ts 

文件内容如下（配置项参考https://umijs.org/zh-CN/config#favicon）：
```javascript
import { defineConfig } from 'umi';

export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes: [
    { path: '/', component: '@/pages/index' }
  ],
  fastRefresh: {},
});


```

### 设置页面标题

```javascript
title:'demo'
```

### 设置favicon

```javascript
favicon: '/favicon.ico',
```

### 如何激活使用约定式路由
> 参考原文：https://umijs.org/zh-CN/docs/convention-routing

除配置式路由外，Umi 也支持约定式路由。约定式路由也叫文件路由，就是不需要手写配置，文件系统即路由，通过目录和文件及其命名分析出路由配置。

如果没有 routes 配置，Umi 会进入约定式路由模式，然后分析 src/pages 目录拿到路由配置。

【划重点】

```js
//.umijs.rc 配置文件中检查是否存在routes配置项，若存在直接删除掉，否则umi不会激活约定式路由

import { defineConfig } from 'umi';

export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  //删除后才会激活约定式路由
  routes: [
    { path: '/', component: '@/pages/index' },
  ],
  fastRefresh: {},
});

```

### 如何使用model

```js
//示例目录结构
demo
---- src
-------- models
------------ global.js //全局state
-------- pages
------------ home
---------------- index.tsx //页面
---------------- model.js //页面state


//=== model.js
export default {
  namespace:'home',
  state:{
    dataList:[]
  },
  reducers:{
    addEntryToList(state,{payload}){
      let newDataList=state.dataList;
      newDataList.push(payload);
      return{...state,
          dataList:newDataList
      }
    }
  }
}


//index.tsx
import {Component} from 'react'
import {connect} from 'umi'

//为解决在TypeScript下通过继承Component方式编译器会报this中不存在props的语法错误，需要在Component中增加泛型:Component<any,any>
class HomePage extends Component<any,any>{
  constructor(props){
    super(props);
    this.onAddEntryClick=this.onAddEntryClick.bind(this);
  }
  onAddEntryClick(entry){
    const {dispatch}=this.props;
    dispatch({
      type:'home/addEntryToList',
      payload:entry
    });
  }
  render(){
    let {dataList}=this.props;
    return (
      <div>
         dataList.foreach(x=>(<p>{x}</p>))
      <div>
    );
  }
}
export default connect((model)=>{....model['home']})(HomePage)
```








