# Vue CLI Vs Vite
**Vite比Vue CLI快10- 100倍? **

### Vue CLI的功能
- 工程脚手架
- 开发服务器
- 插件系统
- 用户UI界面

Vue CLI构建是基于**Webpack**的。主要耗时都在Webpack的性能上。

### Vite
利用浏览器的**原生ES模块**，基于**Rollup**进行构建。
处于测试阶段，不是一体化的工具，目的就是一个**快速**的开发服务器和**简单的**构建工具。

![image-20210926163911099](/Users/hulipeng/Library/Application Support/typora-user-images/image-20210926163911099.png)

###对比时刻

全新项目：

- vue-cli： 3s
- Vite:   瞬间，毫秒级

###它为什么这么快?


### Vite的缺点