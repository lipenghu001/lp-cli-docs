# Lerna简介

## 原生脚手架开发痛点分析

- 痛点一：重复操作
  - 多Package本地link
  - 多Package依赖安装
  - 多Package单元测试
  - 多Package代码提交
  - 多Package代码发布
- 痛点二：版本一致性
  - 发布时版本一致性
  - 发布后相互依赖版本升级

> package版本越多，管理复杂度越高

## Lerna简介

> Lerna is a tool that optimizes the workflow around managing multi-package repositories with git and npm.

Lerna是一个优化基于git+npm的多package项目的管理工具

## 优势

- 大幅减少重复操作
- 提升操作的标准化

> Lerna是架构优化的产物，它揭示了一个架构真理:项目复杂度提升后，就需要对项目进行架构优化。架构
> 优化的主要目标往往都是以效能为核心。

## 官网

官网：https://lerna.js.org/

## 案例

使用Lerna管理的大型项目:

- babel: https://github.com/babel/babel
- vue-cli: https://github.com/vuejs/vue-cli
- create-react-app: https://github.com/facebook/create-react-app

### Lerna开发脚手架流程(划重点)

![image-20210816224647209](/Users/hulipeng/Library/Application Support/typora-user-images/image-20210816224647209.png)

## 基于Lerna创建项目

安装Lerna