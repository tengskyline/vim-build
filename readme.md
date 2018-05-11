# 1.centos7 安装vim8 和YouCompleteMe #

## 下载必要的安装组件  ##

    yum install -y gcc gcc-c++ ruby ruby-devel lua lua-devel  \
    ctags git python python-devel \
    tcl-devel ncurses-devel \
    perl perl-devel perl-ExtUtils-ParseXS \
    perl-ExtUtils-CBuilder \
    perl-ExtUtils-Embed

	* 
## 安装vim8 ##


    访问https://github.com/vim/vim/releases，下载最新的release版本，我这里是vim-8.0.1645.tar.gz

    tar zxvf vim-8.0.1645.tar.gz
    cd vim-8.0.1645
    
    ./configure --with-features=huge \
    --enable-multibyte \
    --enable-rubyinterp=yes \
    --enable-pythoninterp=yes \
    --with-python-config-dir=/usr/lib64/python2.7/config \
    --enable-perlinterp=yes \
    --enable-luainterp=yes \
    --enable-cscope \
    --prefix=/usr/local
    
    make VIMRUNTIMEDIR=/usr/local/share/vim/vim80
    make install
    注意： --with-python-config-dir这项要看自己的实际路径，是python-devel带的目录


## 更改下系统编辑器 ##

    sudo update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
    sudo update-alternatives --set editor /usr/local/bin/vim
    sudo update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
    sudo update-alternatives --set vi /usr/local/bin/vim


# 安装YouCompleteMe #
下载Vundle
切换要安装的用户

    git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
配置~/.vimrc文件，填入如下内容

    set nocompatible  " be iMproved, required
    filetype off  " required
    
    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')
    
    " let Vundle manage Vundle, required
    Plugin 'VundleVim/Vundle.vim'
    Plugin 'Valloric/YouCompleteMe'
    
    
    " All of your Plugins must be added before the following line
    call vundle#end()" required
    filetype plugin indent on" required
    " To ignore plugin indent changes, instead use:
    "filetype plugin on
    "
    " Brief help
    " :PluginList   - lists configured plugins
    " :PluginInstall- installs plugins; append `!` to update or just :PluginUpdate
    " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " :PluginClean  - confirms removal of unused plugins; append `!` to auto-approve removal
    "
    " see :h vundle for more details or wiki for FAQ
    " Put your non-Plugin stuff after this line


下载YouCompleteMe

打开vim，输入如下命令
    :PluginInstall

    注意：
    1、这里考验网速，可能会比较慢，下载好后，整个.vim文件夹大约150MB。
    2、libclang库可能会下载不全（sha256值不对），导致下面编译YouCompleteMe时出错，可以先用迅雷下载，再替换过去。
    下载地址：https://dl.bintray.com/micbou/libclang/libclang-6.0.0-x86_64-linux-gnu-ubuntu-14.04.tar.bz2
    目的文件夹：.vim/bundle/YouCompleteMe/third_party/ycmd/clang_archives/

##  下载CMake（yum安装的2.8版本不行） ##


    访问https://cmake.org/download/，我这里是cmake-3.10.3.tar.gz
    解压编译安装
    tar zxvf cmake-3.10.3.tar.gz
    cd cmake-3.10.3
    ./bootstrap && make && make install



 ## 编译YouCompleteMe（支持C/C++） ##


    cd ~/.vim/bundle/YouCompleteMe/
    ./install.py --clang-completer
    编译完成后，整个.vim文件夹大约350MB。
    支持golang
    ./install.py --clang-completer --go-completer


 配置YouCompleteMe（c++）


    
    编辑~/.vimrc，增加以下内容
    let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'  "设置全局配置文件的路径
    let g:ycm_seed_identifiers_with_syntax=1" 语法关键字补全
    let g:ycm_confirm_extra_conf=0  " 打开vim时不再询问是否加载ycm_extra_conf.py配置
    let g:ycm_key_invoke_completion = '<C-a>' " ctrl + a 触发补全，防止与其他插件冲突
    set completeopt=longest,menu"让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
    nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> "定义跳转快捷键



    第一行所需的.ycm_extra_conf.py，可以从自带的复制一份过来，再根据需要进行配置
    cp .vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py ~

# 2.go vim环境配置 #

## 安装 VIM-GO 插件 ##


    装好了插件管理器，就可以开始安装我们想要的插件了。进入目录 ~/.vim/bundle 后执行命令 git clone https://github.com/fatih/vim-go.git。编辑 ~/.vimrc 文件，加入以下内容（最后一行用于禁止自动下载）：

    syntax enable
    filetype plugin on
    set number
    let g:go_disable_autoinstall = 0此时，插件本身已经安装完成，你可以根据 github.com/fatih/vim-go 的说明进行使用，其中要指出的是 <C-x><C-o> 为代码补全提示，且一般需要在输入 . 操作符之后使用

	
## 安装 tagbar ##



        首先果断的你需要先安装 ctags，我是 Mac 所以用的 brew install ctags 就搞定了。
        然后 go get -u github.com/jstemmer/gotags 安装 Go 语言的相关解析器。
        接着在你的 ~/.vimrc 文件加入以下内容：




    let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds' : [
    \ 'p:package',
    \ 'i:imports:1',
    \ 'c:constants',
    \ 'v:variables',
    \ 't:types',
    \ 'n:interfaces',
    \ 'w:fields',
    \ 'e:embedded',
    \ 'm:methods',
    \ 'r:constructor',
    \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
    \ 't' : 'ctype',
    \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
    \ 'ctype' : 't',
    \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
    \ }


    是时候装 tagbar 插件了，和安装 VIM-GO 一样，首先进入 ~/.vim/bundle 目录。然后执行 。
    编辑 ~/.vimrc 文件，加入行 nmap <F8> :TagbarToggle<CR>。这是个快捷键映射，你可以把 F8 换成任意的。

## 安装目录浏览器 nerdtree ##


进入目录 ~/.vim/bundle 后执行命令 git clone https://github.com/scrooloose/nerdtree.git
编辑 ~/.vimrc 文件，加入行 map <C-n> :NERDTreeToggle<CR>。如此一来，当你需要浏览目录的时候，就可以使用快捷键 <Ctrl+n> 来调出浏览窗口了。



## 更换vim主题 ##




    vimrc 配置

    进入目录~/.vim/
    git clone https://github.com/tengskyline/vim-monokai.git

    编辑 ~/.vimrc 文件 添加如下
    syntax enable
    colorscheme monokai

#  vimrc 最终配置  #
    syntax enable
    colorscheme monokai
    
    "filetype off  " required  
    set nocompatible  " be iMproved, required
    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    Plugin 'VundleVim/Vundle.vim'
    Plugin 'Valloric/YouCompleteMe'
    Plugin 'fatih/vim-go'
    Plugin 'scrooloose/nerdtree'
    Bundle 'majutsushi/tagbar'  
    call vundle#end()" required
    filetype plugin indent on" required
    
    set number
    let g:go_fmt_command = "goimports"
    let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds' : [
    \ 'p:package',
    \ 'i:imports:1',
    \ 'c:constants',
    \ 'v:variables',
    \ 't:types',
    \ 'n:interfaces',
    \ 'w:fields',
    \ 'e:embedded',
    \ 'm:methods',
    \ 'r:constructor',
    \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
    \ 't' : 'ctype',
    \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
    \ 'ctype' : 't',
    \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
    \ }
    
    " ycm setting
    " 让vim的补全菜单行为与一般IDE一致
    set completeopt=longest,menu
    "离开插入模式后自动关闭预览窗口
    "autocmd InsertLeave * if pumvisible() == 0|pclose|endif\
    "回车即选中当前项
    inoremap <expr> <CR>pumvisible()?"\<C-y>":"<CR>"
    "上下左右键的行为 会显示其他信息
    inoremap <expr> <Down> pumvisible() ? "\<C-n>" : "\<Down>"
    inoremap <expr> <Up>   pumvisible() ? "\<C-p>" : "\<Up>"
    inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
    inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"
    "youcompleteme  默认tab  s-tab 和自动补全冲突
    "
    let g:ycm_key_list_select_completion = ['<Down>']
    let g:ycm_key_list_previous_completion = ['<Up>']
    let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'
    " 开启 YCM 基于标签引擎
    let g:ycm_collect_identifiers_from_tag_files = 1
    " 从第2个键入字符就开始罗列匹配项
    let g:ycm_min_num_of_chars_for_completion=2
    "" 禁止缓存匹配项,每次都重新生成匹配项(会出现多打字符的bug)
    "let g:ycm_cache_omnifunc=0
    " 语法关键字补全
    let g:ycm_seed_identifiers_with_syntax = 0
    let g:ycm_confirm_extra_conf=0
    let g:ycm_key_invoke_completion = '<C-/>'
    " 在接受补全后不分裂出一个窗口显示接受的项
    set completeopt-=preview
    "force recomile with syntastic
    nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>
    "在注释输入中也能补全
    let g:ycm_complete_in_comments = 1
    "在字符串输入中也能补全
    let g:ycm_complete_in_strings = 1
    "注释和字符串中的文字也会被收入补全
    let g:ycm_collect_identifiers_from_comments_and_strings = 0
    "设置error和warning的提示符，如果没有设置，ycm会以syntastic的g:syntastic_warning_symbol
    "和 g:syntastic_error_symbol 这两个为准
    let g:ycm_error_symbol='>>'
    let g:ycm_warning_symbol='>*'
    "设置跳转的快捷键，可以跳转到definition和declaration
    "nnoremap <leader>gc :YcmCompleter GoToDeclaration<CR>
    "nnoremap <leader>gf :YcmCompleter GoToDefinition<CR>
    "nnoremap <leader>gg :YcmCompleter GoToDefinitionElseDeclaration<CR>
    let g:ycm_show_diagnostics_ui = 0
    let g:ycm_add_preview_to_completeopt = 0
    "enter 之后推出补全
    let g:ycm_key_list_stop_completion = ['<CR>']
    
    let g:go_highlight_functions = 1
    let g:go_highlight_methods = 1
    let g:go_highlight_structs = 1
    let g:go_highlight_operators = 1
    let g:go_highlight_build_constraints = 1
    let g:go_version_warning = 0
    let g:go_highlight_extra_types = 1
    let g:go_highlight_fields = 1
    let g:go_highlight_types = 1
    "光标移动位置单词高亮
    let g:go_auto_sameids = 1
    
    " 解决插入模式下delete/backspce键失效问题
    set backspace=2
    
    let NERDTreeIgnore=['\.o$', '\.ko$', '\.so$', '\.a$', '\.swp$', '\.bak$', '\~$']
    let NERDTreeSortOrder=['\/$', 'Makefile', '\.c$', '\.cc$', '\.cpp$', '\.h$', '*', '\~$']
    let NERDTreeMinimalUI=1
    let NERDTreeQuitOnOpen=1
    
    
    
    nmap <F8> :TagbarToggle<CR>
    map <F5> :NERDTreeToggle<CR>
    map <F7> :GoReferrers<CR>
    "imap <F6> <C-x><C-o>








