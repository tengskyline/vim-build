1.centos7 ��װvim8 ��YouCompleteMe
	* 
���ر�Ҫ�İ�װ���


    

yum install -y gcc gcc-c++ ruby ruby-devel lua lua-devel  \
    ctags git python python-devel \
    tcl-devel ncurses-devel \
    perl perl-devel perl-ExtUtils-ParseXS \
    perl-ExtUtils-CBuilder \
    perl-ExtUtils-Embed

	* 
��װvim8


        ����https://github.com/vim/vim/releases���������µ�release�汾����������vim-8.0.1645.tar.gz

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
         ע�⣺ --with-python-config-dir����Ҫ���Լ���ʵ��·������python-devel����Ŀ¼

         ������ϵͳ�༭��

sudo update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
sudo update-alternatives --set editor /usr/local/bin/vim
sudo update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
sudo update-alternatives --set vi /usr/local/bin/vim

	* 
��װYouCompleteMe


        ����Vundle

�л�Ҫ��װ���û�

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
         ����~/.vimrc�ļ���������������

set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'


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

	* 
����YouCompleteMe



��vim��������������
:PluginInstall

ע�⣺
1�����￼�����٣����ܻ�Ƚ��������غú�����.vim�ļ��д�Լ150MB��
2��libclang����ܻ����ز�ȫ��sha256ֵ���ԣ��������������YouCompleteMeʱ������������Ѹ�����أ����滻��ȥ��
���ص�ַ��https://dl.bintray.com/micbou/libclang/libclang-6.0.0-x86_64-linux-gnu-ubuntu-14.04.tar.bz2
Ŀ���ļ��У�.vim/bundle/YouCompleteMe/third_party/ycmd/clang_archives/


	* ����CMake��yum��װ��2.8�汾���У�




����https://cmake.org/download/����������cmake-3.10.3.tar.gz
��ѹ���밲װ
tar zxvf cmake-3.10.3.tar.gz
cd cmake-3.10.3
./bootstrap && make && make install



	* ����YouCompleteMe��֧��C/C++��





cd ~/.vim/bundle/YouCompleteMe/
./install.py --clang-completer
������ɺ�����.vim�ļ��д�Լ350MB��
֧��golang
./install.py --clang-completer --go-completer


	* ����YouCompleteMe��c++��



�༭~/.vimrc��������������
let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'  "����ȫ�������ļ���·��
let g:ycm_seed_identifiers_with_syntax=1    " �﷨�ؼ��ֲ�ȫ
let g:ycm_confirm_extra_conf=0  " ��vimʱ����ѯ���Ƿ����ycm_extra_conf.py����
let g:ycm_key_invoke_completion = '<C-a>' " ctrl + a ������ȫ����ֹ�����������ͻ
set completeopt=longest,menu    "��Vim�Ĳ�ȫ�˵���Ϊ��һ��IDEһ��(�ο�VimTip1228)
nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> "������ת��ݼ�



��һ�������.ycm_extra_conf.py�����Դ��Դ��ĸ���һ�ݹ������ٸ�����Ҫ��������
cp .vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py ~

2.go vim��������

	* ��װ VIM-GO ���


    װ���˲�����������Ϳ��Կ�ʼ��װ������Ҫ�Ĳ���ˡ�����Ŀ¼ ~/.vim/bundle ��ִ������ git clone https://github.com/fatih/vim-go.git���༭ ~/.vimrc �ļ��������������ݣ����һ�����ڽ�ֹ�Զ����أ���

syntax enable
filetype plugin on
set number
let g:go_disable_autoinstall = 0��ʱ����������Ѿ���װ��ɣ�����Ը��� github.com/fatih/vim-go ��˵������ʹ�ã�����Ҫָ������ <C-x><C-o> Ϊ���벹ȫ��ʾ����һ����Ҫ������ . ������֮��ʹ��

	* 
��װ tagbar



        ���ȹ��ϵ�����Ҫ�Ȱ�װ ctags������ Mac �����õ� brew install ctags �͸㶨�ˡ�
        Ȼ�� go get -u github.com/jstemmer/gotags ��װ Go ���Ե���ؽ�������
        ��������� ~/.vimrc �ļ������������ݣ�




let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
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


��ʱ��װ tagbar ����ˣ��Ͱ�װ VIM-GO һ�������Ƚ��� ~/.vim/bundle Ŀ¼��Ȼ��ִ��     ��
�༭ ~/.vimrc �ļ��������� nmap <F8> :TagbarToggle<CR>�����Ǹ���ݼ�ӳ�䣬����԰� F8 ��������ġ�

	* ��װĿ¼����� nerdtree


����Ŀ¼ ~/.vim/bundle ��ִ������ git clone https://github.com/scrooloose/nerdtree.git
�༭ ~/.vimrc �ļ��������� map <C-n> :NERDTreeToggle<CR>�����һ����������Ҫ���Ŀ¼��ʱ�򣬾Ϳ���ʹ�ÿ�ݼ� <Ctrl+n> ��������������ˡ�


	* 
����vim����



	* 
vimrc ����



    ����Ŀ¼~/.vim/
    git clone https://github.com/tengskyline/vim-monokai.git

    �༭ ~/.vimrc �ļ� �������
    syntax enable
    colorscheme monokai


syntax enable
colorscheme monokai

"filetype off                  " required  
set nocompatible              " be iMproved, required
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'
Plugin 'fatih/vim-go'
Plugin 'scrooloose/nerdtree'
Bundle 'majutsushi/tagbar'  
call vundle#end()            " required
filetype plugin indent on    " required

set number
let g:go_fmt_command = "goimports"
let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
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
" ��vim�Ĳ�ȫ�˵���Ϊ��һ��IDEһ��
set completeopt=longest,menu
"�뿪����ģʽ���Զ��ر�Ԥ������
"autocmd InsertLeave * if pumvisible() == 0|pclose|endif\
"�س���ѡ�е�ǰ��
inoremap <expr> <CR>    pumvisible()?"\<C-y>":"<CR>"
"�������Ҽ�����Ϊ ����ʾ������Ϣ
inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"
"youcompleteme  Ĭ��tab  s-tab ���Զ���ȫ��ͻ
"
let g:ycm_key_list_select_completion = ['<Down>']
let g:ycm_key_list_previous_completion = ['<Up>']
let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'
" ���� YCM ���ڱ�ǩ����
let g:ycm_collect_identifiers_from_tag_files = 1
" �ӵ�2�������ַ��Ϳ�ʼ����ƥ����
let g:ycm_min_num_of_chars_for_completion=2
"" ��ֹ����ƥ����,ÿ�ζ���������ƥ����(����ֶ���ַ���bug)
"let g:ycm_cache_omnifunc=0
" �﷨�ؼ��ֲ�ȫ
let g:ycm_seed_identifiers_with_syntax = 0
let g:ycm_confirm_extra_conf=0
let g:ycm_key_invoke_completion = '<C-/>'
" �ڽ��ܲ�ȫ�󲻷��ѳ�һ��������ʾ���ܵ���
set completeopt-=preview
"force recomile with syntastic
nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>
"��ע��������Ҳ�ܲ�ȫ
let g:ycm_complete_in_comments = 1
"���ַ���������Ҳ�ܲ�ȫ
let g:ycm_complete_in_strings = 1
"ע�ͺ��ַ����е�����Ҳ�ᱻ���벹ȫ
let g:ycm_collect_identifiers_from_comments_and_strings = 0
"����error��warning����ʾ�������û�����ã�ycm����syntastic��g:syntastic_warning_symbol
"�� g:syntastic_error_symbol ������Ϊ׼
let g:ycm_error_symbol='>>'
let g:ycm_warning_symbol='>*'
"������ת�Ŀ�ݼ���������ת��definition��declaration
"nnoremap <leader>gc :YcmCompleter GoToDeclaration<CR>
"nnoremap <leader>gf :YcmCompleter GoToDefinition<CR>
"nnoremap <leader>gg :YcmCompleter GoToDefinitionElseDeclaration<CR>
let g:ycm_show_diagnostics_ui = 0
let g:ycm_add_preview_to_completeopt = 0
"enter ֮���Ƴ���ȫ
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
"����ƶ�λ�õ��ʸ���
let g:go_auto_sameids = 1

" �������ģʽ��delete/backspce��ʧЧ����
set backspace=2

let NERDTreeIgnore=['\.o$', '\.ko$', '\.so$', '\.a$', '\.swp$', '\.bak$', '\~$']
let NERDTreeSortOrder=['\/$', 'Makefile', '\.c$', '\.cc$', '\.cpp$', '\.h$', '*', '\~$']
let NERDTreeMinimalUI=1
let NERDTreeQuitOnOpen=1



nmap <F8> :TagbarToggle<CR>
map <F5> :NERDTreeToggle<CR>
map <F7> :GoReferrers<CR>
"imap <F6> <C-x><C-o>








