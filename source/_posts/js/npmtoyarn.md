---
title: 从 npm 迁移到 yarn
date: 2019-12-14 17:15:37
tags: npm
comments: true
categories: "js"
---

### 迁移文档地址(https://yarn.bootcss.com/docs/migrating-from-npm/)

### CLI commands comparison（命令比较）
| npm (v5) | Yarn |
|-|-|
| npm install | yarn install |
| (N/A) | yarn install --flat |
| (N/A) | yarn install --har |
| npm install --no-package-lock | yarn install --no-lockfile |
| (N/A) | yarn install --pure-lockfile |
| npm install [package] --save | yarn add [package] |
| npm install [package] --save-dev | yarn add [package] --dev |
| (N/A) | yarn add [package] --peer |
| npm install [package] --save-optional | yarn add [package] --optional |
| npm install [package] --save-exact | yarn add [package] --exact |
| (N/A) | yarn add [package] --tilde |
| npm install [package] --global | yarn global add [package] |
| npm update --global | yarn global upgrade   |
| npm rebuild | yarn add --force |
| npm uninstall [package] | yarn remove [package] |
| npm cache clean | yarn cache clean [package] |
| rm -rf node_modules && npm install | yarn upgrade   |
| npm version major | yarn version --major   |
| npm version minor | yarn version --minor   |
| npm version patch | yarn version --patch |
