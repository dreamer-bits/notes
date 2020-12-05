# Linux C开发环境搭建

---

### 配置Vim编辑器

在用户根目录的`.vimrc`文件中加入如下配置：

```shell
" 语法高亮显示
syntax enable
syntax on    
set background=dark
set nocompatible
filetype on
set number
" 记录历史的行数
set history=1000
" 字符长度提示线
set colorcolumn=80

set autoindent
set cindent

" 设置C/C++语言的具体缩进方式
set cinoptions={0,1s,t0,n-2,p2s,(03s,=.5s,>1s,=1s,:1s
set smartindent
" 空格数量
set ts=4
" 自动缩进的宽度
set shiftwidth=4
set showmatch
set nobackup
" 检测到如下文件格式将tab替换成ts的配置
if has("autocmd")
	autocmd FileType xml,html,c,h,cs,java,perl,shell,bash,cpp,python,vim,php,ruby,conf,ini set expandtab
endif

"just for encode
set fileencodings=utf-8,gb2312,gbk,gb18030
set termencoding=utf-8
set fileformats=unix
set encoding=prc
" 高亮显示所有匹配
set hlsearch
set splitright
set splitbelow

" ctags 索引文件 (根据已经生成的索引文件添加即可, 这里我额外添加了 hge 和 curl 的索引文件)
set tags+=~/.vim/systags
set tags+=/home/nelg/workspace/tags

" 加载Vundle配置文件
if filereadable(expand("~/.vim/config/Vundle.vim"))
    source ~/.vim/config/Vundle.vim
endif
```

### Ctags（查看源码）

- 安装Ctags：`yum install -y ctags`

- 进入源代码的最顶层目录，运行 ctags -R，例如：

  ```shell
  cd ~/work/code/
  ctags -R
  ```

  > 此时 ~/work/code 目录下会生成一个 tags文件

- 为系统库生成`tags`文件：

  ```shell
  ctags -I __THROW -I __attribute_pure__ -I __nonnull -I __attribute__ --file-scope=yes --langmap=c:+.h --languages=c,c++ --links=yes --c-kinds=+p --c++-kinds=+p --fields=+iaS --extra=+q  -f ~/.vim/systags /usr/include/* /usr/include/sys/* /usr/include/netinet/* /usr/include/arpa/* /usr/include/mysql/*
  ```

  > 可根据自身Linux环境扩展的库添加目录，生成后的`tags`文件将存放在`~/.vim/systags`中。

- 使vim引用`tags`索引文件进行代码跳转：

  打开`~/.vimrc`文件添加选项：`set tags+=~/.vim/systags`

  > 多个`tags`文件用`;`隔开
 
 ### Vim补全插件-coc.vim
 
 - 安装地址：https://github.com/neoclide/coc.nvim.git
