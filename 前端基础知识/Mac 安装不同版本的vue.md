# Mac 安装不同版本的vue

## 1. 安装NVM来管理多个版本的node

### 1. NVM下载地址

####  

```
https://github.com/nvm-sh/nvm 
```



### 2.通过如下命令安装 



```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```



### 3.配置文件配置（~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc 看自己电脑情况）



```
 //打开配置文件
open .zshrc


//添加一下命令
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

```



### 4. 验证NVM安装 输出版本号即可

```
 nvm -version
 
```



## 安装不同版本的node

> Vue2 和 Vue3 依赖不同的node版本
>
> Vue2 依赖node版本为 Prerequisites: [Node.js](https://nodejs.org/en/) (>=6.x, 8.x preferred), npm version 3+ and [Git](https://git-scm.com/).
>
> Vue3 依赖node 12.5.0以上

### 使用NVM 安装不同的node版本

```
 // 安装 node版本为14.18.0
nvm install v14.18.0

// 安装 node版本为8.11.2
nvm install v8.11.2
```

### 版本查看以及切换

```
// 查看安装的node版本
nvm lis

// 切换版本  使用14.18.0版本的node
nvm use v14.18.0
```



## 安装多个版本vue-cli



### 1. 桌面新建文件夹命名 my-vue-cli



### 2. 在 my-vue-cli 文件夹下 新建 各个版本 vue-cli 的文件夹 vue-cli-2-9 vue-cli-4-5



### 3. 安装不用版本的vue

```
 cd ~/my-vue-cli/vue-cli-2-9

 npm install vue-cli@2.9.6



 cd ~/my-vue-cli/vue-cli-4-5

 npm install @vue/cli@4.5.13  
```

 

### 4. 此时，我们已经在本地安装了 2个版本的 vue-cli, 接下来就是将这个两个版本的 vue-cli 定义成全局命令，这样我们就能用各个不同版本的 vue-cli 创建项目啦！ 首先在 ~/my-vue-cli 目录下通过VSCode或者记事本建个文件 命名为：MyVueCli.sh

```
#!/bin/zsh

alias vue-cli-2-9="/Users/liguicheng/Desktop/my-vue-cli/vue-cli-2-9/node_modules/vue-cli/bin/vue"

alias vue-cli-4-5="/Users/liguicheng/Desktop/my-vue-cli/vue-cli-4-5/node_modules/@vue/cli/bin/vue.js"

echo "---> has loaded my-vue-cli/MyVueCli.sh"
```



### 5. zsh下添加此source

```
// 打开配置文件
open .zshrc
```



### 6. 添加source

```
source $ZSH/oh-my-zsh.sh

// 添加source
source /Users/liguicheng/Desktop/my-vue-cli/MyVueCli.sh
```



### 7. 保存

```
source ~/.zshrc
```



### 8. 终端下验证 以下输出便是成功

```
vue-cli-2-9 -V
2.9.6
 ~  vue-cli-4-5 -V
@vue/cli 4.5.13
```



### 9. 新建项目

```

vue-cli-2-9 init webpack vue-program



vue-cli-4-5 create vue-program
```

