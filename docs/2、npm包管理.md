# 1、[Vue速成]npm包管理

### npm 是什么？

* ```npm```全名 ```node package manger```；是javascript模块登记和管理系统平台
* 使用```npm```提供的命令行工具，可以方便地下载、安装、升级、删除包，方便项目的依赖项目管理

> 在npm出现之前，前端开发主要是根据```<script>```标签引入第三方库，比如```jQuery```，引用的方式可以是远程的和本地的项目中引入的会有以下两个问题

* 1、 在没有提取公共头部时，每一个页面都是需要单独引入库文件，当需要切换库的版本时，需要全局源码进行批量的替换，改动是比较大的，甚至有漏改的文件
* 2、如果是从依赖库是存贮在本地项目中的，会额外的增加项目的实际大小，在多人开发的源码文件传输上造成其大浪费（比如一个项目有可能实际只有5M，但包含了其他依赖库的文件之后，增大到了200M，这在互相传输源码和存贮上都是极大的浪费）

> 使用了```npm包管理```后则解决了上述的问题

### 如何安装npm?

* 需要安装nodejs，安装好nodejs后npm是捆绑在一起安装的
* nodejs下载地址：http://nodejs.cn/download/
* 通常安装好后就可以在控制台中执行：```npm -v```查看到npm 的版本号了，如果有类似提示：```command not found: npm ```
  ，那么很大概率是系统的环境变量没有设置。可百度搜索：```nodejs安装及环境变量配置```依照步骤设置环境变量即可

### 使用npm init 命令初始化项目

> npm有很多指令，但是常用的也就几个，本文仅仅介绍常用的指令，多动手打几遍即可熟悉。需要了解更多指令可[点击查看官方文档](https://docs.npmjs.com/cli/v9/commands)

#### 1) ```npm help``` 帮助指令

* ```npm help```可以快速查看npm的指令帮助，数量后可以节省我们查看文档的时间，简写为```npm -h```，下列将展示```npm help```
  的用法

```shell
npm help # 输出 npm 指令的使用说明

npm -h  # 简写模式 同上输出 npm 指令的使用说明

npm init -h # 输出 npm init 指令的使用说明

npm install -h # 输出 npm install 指令的使用说明

```

#### 2) ```npm init```初始化配置文件

* 使用该指令可以在当前文件夹下快速生成一个```package.json```的文件
* ```package.json```主要声明了该项目的依赖和一些项目说明等，在多人开发中，开发者只需要拥有了该配置文件，即可从网络中快速的获取到对应的依赖包，而无须共享公共的第三方库文件

```shell
# 我们创建一个空的文件夹，在该文件夹下打开控制台，输入如下指令后，依次填写每一步骤的信息（这些信息后续都是可更改的），或者一路回车使用默认的信息。即可自动生成```package.json```文件。
npm init
```


#### 3) ```package.json```文件的说明

* 完整的配置说明可[点击查看官方文档](https://docs.npmjs.com/cli/v9/configuring-npm/package-json)
* 该配置文件的内容大概如下（上一步中我们填写的内容都会在下列生成）：

```json5
{
  "name": "vue-example", // 项目名称
  "version": "1.0.0", // 项目版本号
  "description": "", // 项目说明
  "author": "", // 作者
  "license": "ISC", // 许可证
  // 上面的配置通常在项目发布到www.npmjs.com库管理中时，都是可以忽略的
  
  "main": "index.js", // 入口文件（如果未设置，则默认为包根文件夹中下的index.js）
  "scripts": { // 项目的自定义脚本命令（第6小节讲解）
    "start": "http-server -o ./html",
    "dev": "http-server -o ./html",
    "dev:demo": "npm run dev"
  },
  "dependencies": { // 本项目的依赖
    "axios": "^1.1.3" // 依赖的包名为：axios 版本为：1.1.3 后续通过升级指令升级的范围限定为： >= 1.1.3 且 < 2.0.0 (依赖包可在www.npmjs.com中通过包名搜索，版本号的说明在下一小结进行说明)
  },
  "devDependencies": { // 本项目的开发环境的依赖（在项目工程化中，使用一些打包工具压缩打包源码的时候是不会引入这些依赖包的，这样可以减少生成的产品的源码大小，这些包只是在开发环境中方便开发者开发的一些工具）
    "http-server": "^14.1.1"
  }
}
```

#### 4）依赖包的版本号说明

* ```1.1.1```版本号固定为：1.1.1，后续通过```npm update```指令是无法直接升级的，需要升级可通过手动修改```package.json```文件后，通过```npm install```进行安装，或者```npm update```进行版本升级
* ```^1.1.1```当前使用的版本号为：1.1.1，后续通过```npm update```最高可升级的版本永远会小于2.0.0（这是最常用的版本包指定，因为大版本之间的变动通常是很大的，互不兼容的）
* ```~1.1.1```当前使用的版本号为：1.1.1，后续通过```npm update```最高可升级的版本永远会小于1.2.0（小版本限制）
* ```>=1.1.1```当前使用的版本号为：1.1.1，后续通过```npm update```永远会升级到最新的稳定版本（更宽松的指定，会跨版本升级，应该特别注意）

> 上面是最常用的 当然还有其他的版本号填写方式，下列仅做扩展

* ```alpha``` 预览版 内部测试版
* ```beta``` 测试版 公开测试版
* ```rc``` 最终测试版本
* ......

#### 5）使用npm指令管理依赖包

* 在安装之前我们需要知道2个知识点：
  * 1、在执行安装指令后项目跟目录下会自动创建一个```node_modules```文件夹，所有的依赖包的源码都会下载到该文件夹内，该文件夹是该项目独有的依赖包源码，其它项目是不可用的，当我们在共享源码的时候```切记```忽略掉该文件夹，其它开发者只要拿到```package.json```文件即可快速的拉取第三方库，并且自动生成该文件夹内容
  * 2、我们在项目之外还有一个全局的```node_modules```文件夹，通常在nodejs的按照目录下可找到，全局安装的依赖包通常都是一些快捷的全局指令的库，比如：http-server、nodemon、cnpm、yarm、pnpm等包

> 下列代码演示依赖包安装、卸载、版本号变动。使用到的指令都是全完整的，见词知意（有简写指令、别名指令，需要了解可自行百度）


```shell

npm install # 执行该指令会按照package.json的描述进行安装未进行安装的依赖包

npm install axios # 在当前项目下安装axios包的最新稳定版本（实际使用中应该需要添加 --save 或者 --save-dev选项）

npm install axios --save # 在当前项目下安装axios包的最新稳定版本，并且将axios的依赖项目写入到package.json文件的dependencies属性中

npm install axios --save-dev # 在当前项目下安装axios包的最新稳定版本，并且将axios的依赖项目写入到package.json文件的devDependencies属性中

npm install axios --global # 将axios包安装到npm全局中

npm uninstall axios # 移除axios(移除包指令默认会自动将package.json中的依赖配置清除，无须添加任何的--save选项)

npm update # 升级项目内的所有依赖包（升级的版本规则参考上一个小节）

npm update axios # 升级项目内的axios包（升级的版本规则参考上一个小节）

npm update axios@"^0.26.1" # 将项目内的axios包变更（升级/降级）为：^0.26.1

npm update axios@0 # 将项目内的axios包变更v0大版本下的最大稳定版

npm update axios@1 # 将项目内的axios包变更v1大版本下的最大稳定版


npm search axio  # 查找包含 ```axio```关键词的包（有时我们可能只记得某些关键词，又懒得打开npmjs.com去搜索的时候，可以通过该指令快速搜索到包名）

npm view axios # 快速查看axios包的信息（通常看到的就是axios的package.json的信息内容）

npm view axios versions # 列举出axios包的所有可安装版本（通常在我们需要快速升级或者降级某个包时很有用！）

npm list # 查看当前项目中的所有依赖详情

```

* 当然npm命令还有很多，上面列举的只是个人认为对开发者比较有用的指令，而我们最常用的其实就是 ```init```、```install```、```uninstall```、```update```这4个，甚至很多时候只需要中间的两个指令就够了！
* 需要了解其它命令[点击查看官方文档](https://docs.npmjs.com/cli/v9/commands)

#### 6）使用npm run xxx 执行自定义项目中的脚本

* 在第3小节中我们提到的```package.json```中的```scripts```配置项中的自定义脚本，即可通过``` npm run xxx``` 执行

```json5
{
  "scripts": { // 自定义脚本指令
    "start": "http-server -o ./html",  // 执行：npm run start 或者 npm start (start指令是比较特殊的可以省略run 其它指令是不可省略的)
    "dev": "http-server -o ./html", // 执行： npm run dev
    "dev:demo": "npm run dev", // 执行：npm run dev:demo
    "run": "npm install && npm start" //执行： npm run run 串行的执行全局的npm脚本
  },
}
```

* 如上便```npm run```指令的用法；那么使用 ```npm run dev```和直接执行```http-server -o ./html```有什么区别呢？
  * 直接执行```http-server -o ./html```会使用全局安装的```http-server```工具依赖包，如果全局中没有是会报错的
  * 而使用```npm run dev```，再通过```scripts```去执行则，该指令中用到的```http-server```会首先从项目下的```node_modules/.bin/```目录下去查找```http-server```工具依赖包，如果找不到再去```全局的node_modules/.bin/```去查找，再找不到才会报错

#### 7）使用npx指令临时执行一些命令

* 有时候我们并不想使用```npm run```指令但是又需要执行某些命令，该命令包又不想安装在全局中时，会临时使用```npx``` 
* ```npx``` 会在当前目录下的./node_modules/.bin里去查找是否有可执行的命令，没有找到的话再从全局里查找是否有安装对应的模块，全局也没有的话就会自动下载对应的模块，如：```npx http-server -o ./html``` 如果都找不到```http-server```可执行命令，则会将 ```http-server```下载到一个临时目录，用完即删，不会占用本地资源。
* 注：```npx```指令是只有```npm >= 5.2.0```才会有的


#### 8) npm包管理器的一些不足

> 本小节并不会详细的列举npm的不足，只是为了引出一些更好用的包管理器，不过依赖配置文件package.json都的不便的，当然在安装依赖包的时候，无论是哪一个包管理工具，都会生成一个自己的额外的lock文件，如果感兴趣可以自行搜索了解

* ```cnpm```这是国内版的```npm```唯一的区别的就是依赖包下载地址的不同而已，当然依赖包下载的东西都是与官方同步过来的镜像，据说有10分钟的延迟（无法访问外网时可尝试使用）
* ```yarn```可以认为这是```npm```的一个非官方的优化版，他最主要的特性是实现依赖包的```并行下载```功能，大大提高了下载效率
* ```pnpm```可以认为是```npm```的高性能版本，我们说npm的依赖包，每个项目是独立，即：两个项目如果用到的同一个包同一个版本，是会下载两次的，这个从效率和硬盘占用上都是浪费的。```pnpm```会进行统一的管理所有的依赖包，不同的项目之间通过软链接的形式依赖。```同一个包同一个版本```

#### 9) pnpm包管理器的一些不足

> pnpm 的用法和npm基本是一样的，熟悉npm命令即可快速上手，本小节只需要介绍 pnpm 的安装即可


```shell
npm install -g pnpm # 全局安装pnpm （安装后即可在命令行中使用了）

pnpm add axios # 安装axios包（npm版本的install，会自动写入dependencies中）

pnpm add axios -D # 安装axios包（写入devDependencies中）

pnpm add axios # 安装axios包（install别名）

pnpm remove axios # 移除axios包（npm版本的uninstall）

```

* 其它更多指令文档[点击查看pnpm中文文档](https://www.pnpm.cn/cli/add)


### 结语

> 拖了很久，又写了一篇，看来年初定下的目标也不知何年能完成了


