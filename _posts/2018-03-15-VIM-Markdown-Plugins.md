---
tags: Markdown
excerpt: VIM the God of Markdown Editor
---

Also published on [S&C](https://soandcandy.us)

CC 4.0 BY-SA

----

## 编辑器之神VIM使用Markdown写作 ##


在众多文本编辑器中，如nano、gedit、pico等等，VIM可以视为VI的高级版，有着**编辑器之神**的名号，主要在程序员圈子里流传。事实上它的确是coding很方便的编辑器之一。此外，也有一些原因加强了它一代神器的光环，如：

1. 所有Unix类系统都内置VI，其它文本编辑器就没这样的待遇了；
2. 诸多软件的编辑接口都会主动调用VI；
3. 程序编辑能力，简单快捷效率高。


VIM功能强大，支持Linux、MacOS、Windows，在命令行下输入`vim filename`即可使用。


![VIM](https://i.imgur.com/MFYbkld.png)

### VIM的三种模式 ###

VIM有三种操作模式，分别为**编辑模式**、**插入模式**、**命令模式**,运行VIM时，首先进入编辑模式。

#### 1. 编辑模式 ####

主要用途是在被编辑的文件中移动光标的位置。一旦光标移到到所要的位置，就可以进行**剪切**和**粘贴**正文块，**删除**正文和**插入**新的正文。

当完成所有的编辑工作后，需要保存编辑器结果，退出编辑程序回到终端，可以发出`ZZ`命令，连续按两次大写的Z键。

##### 1.1 跳转 #####

如果键盘上有上、下、左、右箭头的导航键，就由这些键来完成光标的移动。Vim还提供稍大范围移动光标的命令：

> ctrl+f      在文件中前移一页（相当于 page down）；
>
> ctrl+b      在文件中后移一页（相当于 page up）；
>
> G           到文件尾；
>
> 10G         到第10行，10可以为任意其它数；
>
>gg           到文首；
>
>H            屏幕起始行，可以在前面加数字，表示到屏幕第几行；
>
>M            屏幕中间（前面可加数字）；
>
>L            屏幕最后一行（前面可加数字）；
>
>ma           在光标处标记一个书签a（必须小写）；
>
>`a           到书签a处（注意，这是1左边那个键）；
>
>`.           到你上次编辑文件的地方；

其他自己慢慢发掘吧。


##### 1.2 搜索 #####

键入字符 / ，后面跟以要搜索的字符串，然后按回车键。编辑程序执行正向搜索（即朝文件末尾方向），并在找到指定字符串后，将光标停到该字符串的开头；键入 n 命令可以继续执行搜索，找出这一字符串下次出现的位置。用字符 ? 取代 / ，可以实现反向搜索。

另外，还可以：

> \* 当光标停留在一个单词上，\* 键会在文件内搜索该单词，并跳转到下一处；
>
> \# 当光标停留在一个单词上，\# 在文件内搜索该单词，并跳转到上一处；

查找和替换功能是VIM最为强大的地方之一。以后专门再说。


#### 2. 插入模式 ####

##### 2.1 进入 #####

> i           在光标左侧插入正文
>
>a            在光标右侧插入正文
>
>o            在光标所在行的下一行增添新行
>
>O           在光标所在行的上一行增添新行
>
>I            在光标所在行的开头插入
>
>A           在光标所在行的末尾插入
>

##### 2.2 退出 #####

很简单，ESC键。


#### 3. 命令模式 #### 

> :w    将编辑的数据写入到硬盘中。
>
>:q     离开vi.后面加！为强制离开。
>
>:wq    保存后离开。:wq!为强制保存后离开。


### 给VIM装上Markdown的翅膀 ###

首先是给VIM装上插件管理器，可以是**Vundle**、**pathogen**等等。这里是Vundle。

#### 安装Vundle ####

先确保已经有Git安装环境，及curl。

安装：
```
   git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

```

设置.vimrc文件

```
   set nocompatible              " be iMproved, required
   filetype off                  " required

   " set the runtime path to include Vundle and initialize
   set rtp+=~/.vim/bundle/Vundle.vim
   call vundle#begin()
   " alternatively, pass a path where Vundle should install plugins
   "call vundle#begin('~/some/path/here')

   " let Vundle manage Vundle, required
   Plugin 'VundleVim/Vundle.vim'

   " The following are examples of different formats supported.
   " Keep Plugin commands between vundle#begin/end.
   " plugin on GitHub repo
   Plugin 'tpope/vim-fugitive'
   " plugin from http://vim-scripts.org/vim/scripts.html
   " Plugin 'L9'
   " Git plugin not hosted on GitHub
   Plugin 'git://git.wincent.com/command-t.git'
   " git repos on your local machine (i.e. when working on your own plugin)
   Plugin 'file:///home/gmarik/path/to/plugin'
   " The sparkup vim script is in a subdirectory of this repo called vim.
   " Pass the path to set the runtimepath properly.
   Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
   " Install L9 and avoid a Naming conflict if you've already installed a
   " different version somewhere else.
   " Plugin 'ascenator/L9', {'name': 'newL9'}

   " All of your Plugins must be added before the following line
   call vundle#end()            " required
   filetype plugin indent on    " required
   " To ignore plugin indent changes, instead use:
   "filetype plugin on
   "
   " Brief help
   " :PluginList       - lists configured plugins
   " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
   " :PluginSearch foo - searches for foo; append `!` to refresh local cache
   " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
   "
   " see :h vundle for more details or wiki for FAQ
   " Put your non-Plugin stuff after this line
```

不需要用到的Plugin请注释掉。


安装Plugin，只需要运行VIM，命令`:PluginInstall`即可。

详情可参阅[Github项目页](https://github.com/VundleVim/Vundle.vim)


#### Markdown 预览 ####

如果你觉得Markdown的理念是看着一行行语法，心中进行预览，那这一步可以省略了。对于大多数人来说，预览还是比较有用的功能。

##### 安装 instant-markdown-d #####

确认有npm安装环境，没有的话先安装npm。
`npm -g install instant-markdown-d`即可安装。

或者

##### 安装 vim-instant-markdown #####

Linux用户先安装：
- xdg-utils
- curl
- nodejs-legacy（Debian-based）

`npm -g install instant-markdown-d`即可安装。

详情参阅[github项目页](https://github.com/suan/vim-instant-markdown)


#### Markdown支持 ####

推荐安装**Markdown for Vim**。
详情参阅[github项目页](https://github.com/tamlok/vim-markdown)


#### 自动识别Markdown文件 ####

在`~/.vimrc`中添加：

```
   autocmd BufNewFile,BufReadPost *.md set filetype=markdown
```


安装安装vim-instant-markdown插件时可能需要安装node.js的话：

```
   sudo add-apt-repository ppa:chris-lea/node.js
   sudo apt-get update
   sudo apt-get install nodejs
```



效果如图：

![效果图](https://i.imgur.com/CCgJswE.png)

