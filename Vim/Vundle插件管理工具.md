# Vundle插件管理工具

---

### 安装Vundle

1. 创建`～/.vim/bundle`目录

2. 在`～/.vim/config`目录下添加`Vundle.vim`配置文件，专门用于vim配置文件独立出插件配置文件：

   ```shell
   #在.vimrc文件中添加以下代码
   if filereadable(expand("~/.vim/config/Vundle.vim"))
       source ~/.vim/config/Vundle.vim
   endif
   ```

3. 在`~/.vimrc`文件中加载`Vundle.vim`配置文件

4. 下载：`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

5. 配置安装程序，在`Vundle.vim`文件中添加以下语句：

   ```shell
   set nocompatible
   filetype off
   set rtp+=~/.vim/bundle/Vundle.vim
   call vundle#begin()
   Plugin 'VundleVim/Vundle.vim'
   ##可使用多个 `Plugin 插件路径` 的形式安装、加载多个插件
   call vundle#end()
   filetype plugin indent on
   ```

6. 打开vim，运行`:PluginInstall`命令来自动安装插件，过程中有可能需要输入github用户名和密码。等待Vundle安装完成即可。

### DoxygenToolkit快速添加注释块插件

1. 安装，在`Vundle.vim`文件中的插件加载列表中添加以下语句：

   ```shell
   ...
   call vundle#begin()
   Plugin 'VundleVim/Vundle.vim'
   ##可使用多个 `Plugin 插件路径` 的形式安装、加载多个插件
   Plugin 'vim-scripts/DoxygenToolkit.vim'
   call vundle#end()
   ...
   ```

2. 在vim中使用安装命令`:PluginInstall`安装插件

3. 新建`～/.vim/config/Vundle.vim`文件添加以下语句：

   ```shell
   let g:DoxygenToolkit_commentType = "c" 
   let g:DoxygenToolkit_briefTag_pre = "@brief\t"
   let g:DoxygenToolkit_briefTag_funcName = "yes"
   let g:DoxygenToolkit_briefTag_post = ""
   let g:DoxygenToolkit_templateParamTag_pre = "@tparam\t"
   let g:DoxygenToolkit_paramTag_pre = "@param\tstring\t"
   let g:DoxygenToolkit_returnTag = "@return\t"
   let g:DoxygenToolkit_throwTag_pre = "@throw\t"
   let g:DoxygenToolkit_fileTag = "@script\t"
   let g:DoxygenToolkit_dateTag = "@modify\t"
   let g:DoxygenToolkit_authorTag = "@author\t"
   let g:DoxygenToolkit_versionTag = "@version\t"
   let g:DoxygenToolkit_versionString = "1.0.0"
   let g:DoxygenToolkit_blockTag = "@name\t"
   let g:DoxygenToolkit_classTag = "@class\t"
   let g:DoxygenToolkit_authorName = "Nelg<nicenelg@gmail.com>"
   let g:doxygen_enhanced_color = 1 
   ```

4. 在`～/.vim/config/Vundle.vim`文件中添加以下语句：

   ```shell
   if filereadable(expand("~/.vim/config/DoxygenToolkit.vim"))
       source ~/.vim/config/DoxygenToolkit.vim
   endif
   ```

   