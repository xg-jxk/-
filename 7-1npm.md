# npm

- **包语义化版本规范**

```
1. 总共三位数字
2. 第一位数字大版本  ,第二位功能版本  ,第三位Bug修复版本
3. 前面的版本号增长了,则后面的版本号归零
```

- **package.json**

```js
#node_modules文件添加到.gitignore忽略文件中

根目录创建 package.json配置文件    # npm init -y   (只能在英文目录运行,不能出现空格)

- `package.json` 中必须包含 `name`，`version`，`main` 这三个属性，分别代表包的名字、版本号、包的入口

```

- **README.md 文档**

```
`README.md `文档中，会包含以下几项内容

- 安装方式
- 导入方式
- 用法
- 开源协议  ISC
```



- **nrm 切换镜像源**

```
npm i nrm -g             全局安装 
nrm ls                   查看可用镜像源  
nrm use taobao           切换镜像源 
```

- **上传包**

```js
npm login    登陆npm账号     //要先切换npm镜像源
npm publish                 //切换到包的文件夹输入命令即可发布包
npm unpublish 包名 --force   //从npm删除已发布的包  只能删除72小时以内发布的包
```

- **安装包**

```js
npm install                 //一次性安装package.json中dependencies节点中的所有包

npm install 包名             //可以一次安装多个包 , 名字之间空格隔开

npm install 包名 --save-dev  //记录到devDependencies节点中(开发依赖包) 简写 npm i 包名 -D

npm run serve   启动服务器
```

- **卸载包**

```js
npm uninstall 包名      
```

- **npm切换镜像源**

```js
//查看当前下包镜像源
npm config get registry
//切换成淘宝镜像源
npm config set registry=https://registry.npm.taobao.org/
```

# yarn

https://yarn.bootcss.com/

## 切换镜像

```js
yarn config set registry https://registry.npm.taobao.org/

yarn config set registry https://registry.yarnpkg.com
```

## 常用命令

```bash
#测试 Yarn 是否安装成功
yarn --version

# 1. 初始化, 得到package.json文件(终端路径所在文件夹下)
yarn init

# 2. 添加依赖(下包)
# 命令: yarn add [package]
# 命令: yarn add [package]@[version]
yarn add 包名
yarn add 包名@版本

# 3. 移除包
# 命令: yarn remove [package]
yarn remove 包名
             
# 4. 安装项目全部依赖(一般拿到别人的项目时, 缺少node_modules)          
yarn
# 会根据当前项目package.json记录的包名和版本, 全部下载到当前工程中

# 5. 全局
# 安装: yarn global add [package]
# 卸载: yarn global remove [package]

yarn global add @vue/cli  # 注意: global一定在add左边

vue -V  查看版本

yarn serve          启动服务器
```

