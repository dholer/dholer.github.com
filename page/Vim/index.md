---
layout: page 
title: Vim
---

##常用命令

##\_vimrc 配置文件

    set nocompatible
    source $VIMRUNTIME/vimrc_example.vim
    source $VIMRUNTIME/mswin.vim
    behave mswi


    "set laststatus=2 " Enables the status line at the bottom of Vim
    "set statusline=%{GitBranchInfoString()}

    set laststatus=2
    set statusline=%{GitBranch()
    

    """"""""""""""""""""""""""""""
    " Tag list (ctags)
    """"""""""""""""""""""""""""""
    let Tlist_Ctags_Cmd = 'ctags'
    let Tlist_Show_One_File = 1            "不同时显示多个文件的tag，只显示当前文件的
    let Tlist_Exit_OnlyWindow = 1          "如果taglist窗口是最后一个窗口，则退出vim
    let Tlist_Use_Right_Window = 1         "在右侧窗口中显示taglist窗口 
    map <silent> <F9> :TlistToggle<cr> 


    








