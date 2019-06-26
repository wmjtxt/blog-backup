---
layout: post
title: 安装YouCompleteMe
date: 2018-05-14
tags: [vim, Linux]

---

参考[https://github.com/Valloric/YouCompleteMe](https://github.com/Valloric/YouCompleteMe)

* 1.安装Vundle
	* `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
	* 把以下内容复制到.vimrc:
<!-- more -->

	```shell
	set nocompatible              " be iMproved, required
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
	"Plugin 'file:///home/gmarik/path/to/plugin'
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
	* Install Plugins: 打开vim执行`:PluginInstall` 或者直接输入命令`sudo vim +PluginInstall +qall`
* 2.Install development tools and CMake:
	* `sudo apt-get install build-essential cmake`
* 3.Make sure you have Python headers installed:
	* `sudo apt-get install python-dev python3-dev`
* 4.安装clang
	* `sudo apt-get install clang`
* 5.Compiling YCM with semantic support for C-family languages:
	* `git clone https://github.com/Valloric/YouCompleteMe.git ~/.vim/bundle/YouCompleteMe`
	* `cd ~/.vim/bundle/YouCompleteMe`
	* `./install.py --clang-completer`
	* 如果出错执行 `git submodule update --init --recursive`
* 6.把以下内容复制到.vimrc中

	```shell
	"----------------------------"
	"------ YouCompleteMe -------"
	"----------------------------"
	let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/cpp/ycm/.ycm_extra_conf.py'
	
	
	" YouCompleteMe
	set runtimepath+=~/.vim/bundle/YouCompleteMe
	let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM 基于标签引擎
	let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全
	let g:syntastic_ignore_files=[".*\.py$"]
	let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全
	let g:ycm_complete_in_comments = 1
	let g:ycm_confirm_extra_conf = 0
	let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']  " 映射按键, 没有这个会拦截掉tab, 导致其他插件的tab不能用.
	let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
	let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
	let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
	let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全
	let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
	let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查
	inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>" |            " 回车即选中当前项
	nnoremap <c-j> :YcmCompleter GoToDefinitionElseDeclaration<CR>|     " 跳转到定义处
	"let g:ycm_min_num_of_chars_for_completion=2                 " 从第2个键入字符就开始罗列匹配项
	```

按说这样应该就可以用了，不过提示有错误。

`YouCompleteMe unavailable: requires Vim compiled with Python (2.7.1+ or 3.4+) support.`

在网上找了好久，试了好多方法，最终还是按深度社区的方法才解决。
[编译安装vim8.0,添加python支持](https://bbs.deepin.org/forum.php?mod=viewthread&tid=43716)
* 1.`vim --version|grep python` 查看是否支持python
* 2.`sudo apt-get install python-dev python3-dev libncurses5-dev`，前面安装过python-dev python3-dev,这里会提示已是最新版本
* 3.`git clone https://github.com/vim/vim.git `
* 4.`cd ~/vim`
* 5.`./configure --with-features=huge --enable-python3interp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python3-config-dir=/usr/lib/python3.6/config-3.6m-x86_64-linux-gnu/ --enable-multibyte --enable-cscope      --prefix=/usr/local/vim/`,这里python2.7和python3.6要跟你安装的版本相对应
* 6.`sudo make && sudo make install `
* 7.`which vim`查看vim安装位置，我的是/usr/bin/vim，然后`sudo cp /usr/local/vim/bin/vim /usr/bin/`


截图:

![YCM](/images/YouCompleteMe.JPG)
