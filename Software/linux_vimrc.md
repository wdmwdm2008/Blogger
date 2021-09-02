### vimrc默认设置

```
syntax on
set tabstop=4
set shiftwidth=4
set expandtab
set ai! 
set ruler
set incsearch
set hlsearch
set nu
filetype plugin indent on

set background=light

let g:tagbar_width = 30

nmap <F2> :NERDTreeToggle<CR>
nmap <F3> :TagbarToggle<CR>
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
```
