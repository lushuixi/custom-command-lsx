# custom-command-lsx
2023/2/27 , custom node command sugar123

### 自定义命令 sugar123

将命令`sugar123`链入全局,并在终端执行该命令, 得到下面打印信息

```
你好,我是露水晰123,是一名前端开发
很高兴能与你相遇

[npm init]初始化项目,生成package.json
package.json新增bin字段,添加命令sugar123且指定命令入口文件bin/sugar123.js
在bin/sugar123.js文件第一行写上[#!/usr/bin/env node]指定执行当前脚本的解释器node
继续打印点东西

[npm link]能将该包对应的命令链入本地,从而可全局使用
本地测试命令,打开终端,输入命令[sugar123]

[npm adduser]第一次发布需要,输入用户名、密码和邮箱账号
一直报错 403 Forbidden - PUT https://registry.npmmirror.com/-/user/org.couchdb.user:lushuixi - [FORBIDDEN] Public registration is not allowed
本地npm使用镜像指向了taobao镜像(https//registry.npm.taobao.org),所以报这个错
[npm config set registry https://registry.npmjs.org/]将镜像切回再执行[npm adduser]
[npm publish]将包发布到npm
另一台电脑测试,安装新包,package.json里的命令可直接执行吗?
在另一台电脑上创建一个node包[npm init]
[npm i custom-command-lsx],局部安装包,其命令会在node_modules/bin目录下(要想执行需要链入全局)
[sugar123]执行命令,会发现该命令并没有被链入
[npm i custom-command-lsx -g]全局安装,再输入[sugar123]便可以执行命令了,为什么?
因为输入的命令会到全局里去找, 当前模块自定义命令需要链入全局才可使用
```

### 发包到npm流程

- 首先要有一个 [npm账号](https://www.npmjs.com/);
- 第一次发布先添加用户 **npm adduser** , 输入用户名、密码和邮箱账号;
- 非第一次发布先登录 **npm login**;
- 发包 **npm publish**, 注意发包前版本号要更新;
