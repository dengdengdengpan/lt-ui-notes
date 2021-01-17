# 初始化

### 搭建项目

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

