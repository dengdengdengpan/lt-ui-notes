# 概览

### 网站开发基本流程

1. 立项，确定是否要做后、确定人员以及预算等
2. 需求，收集和分析：
   - 收集比分析更难，往往用户也不知道自己的需求时什么
   - 分析需求可以用用例图
3. 可行性分析
4. 系统设计（功能设计、框架设计）
   - UML图、时序图等
5. 原型设计（草图、线框图）
   - 草图可以直接用画图软件或纸笔画
   - 线框图可以用 Balsamiq
6. 交互设计
   - Axure RP、Sketch、墨刀
7. 视觉设计
   - Sketch、Photoshop
8. 代码开发
9. 测试
10. 功能预演
11. 内测
12. 灰度发布
13. 正式发布

### 搭建项目流程

- 代码放在哪里？

  - github
  - 自建的 gitlab

- git 初始化以及推送本地仓库到远程仓库

  ```bash
  mkdir 文件夹名
  cd 创建的文件夹
  git init
  touch README.md
  git add .
  git commit -m 'git init'
  git remote add origin 自己仓库的git
  git push -u origin master
  ```

- 创建开源协议：MIT

- JavaScript 包管理工具以及初始化

  - 选用 npm 作为包管理工具
  - `npm init` 进行初始化

- 使用 Vue 框架

  ```bash
  npm install vue
  ```

