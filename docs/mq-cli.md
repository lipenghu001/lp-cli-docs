# mq-cli

> 麻雀B端脚手架

## Install

```bash
npm i -g @mq-cli/core
```

## Usage

1. 创建空项目文件夹并进入：

```bash
mkdir sparrow_xx
cd sparrow_xx
```

2. 执行脚手架命令：

```bash
mq-cli init <name_in_package.json> [--force|-f] [--debug|-d]
```

3. 根据脚手架的提示输入必要信息：（<font color="red">注意：会二次确认是否清空当前文件夹，小心误删</font>）
   - 项目版本号(package.json中的version)
   - 登录页标题（如：麻雀扶摇系统）
   - 首页标题（如：扶摇）

4. 根据脚手架的提示选择模板：

   > 通常选最新

5. 选择完模板后等待即可，脚手架会自动完成所有后续流程，包含：
   1. 项目模板下载或更新到本地缓存目录
   2. 从缓存目录拷贝目标模板到目标项目文件夹
   3. ejs动态渲染步骤三录入的变量
   4. 执行npm install安装所有依赖
   5. 自动执行 npm start



## Participate in this Cli

github-repo:  https://github.com/hanguangbaihuo/mq-cli/



## Want to access your own template？

1. Reference this repo to develop your own templates：

   ​	github-repo: https://github.com/hanguangbaihuo/mq-cli-template-qiankun

2. Contact the person in charge of cli help to access the template.



## 执行流程图

![image-20210826120840189](/Users/hulipeng/Library/Application Support/typora-user-images/image-20210826120840189.png)

