# fzf-preview.vim

fzf-preview is a Neovim plugin that provides a preset of commands using fzf.  
Provides multiple resources and a preview command for it.

fzf-preview mainly uses neovim floating window.
vim may work depending on the setting, but it is not recommended.

This plugin does not use [fzf.vim](https://github.com/junegunn/fzf.vim) but uses the library attached to fzf.
Though it is different from this plugin and has a lot of functions,
[fzf.vim](https://github.com/junegunn/fzf.vim) has no preview of the project's file list and grep on the interactive project.

## Features

1. Fzf can be operated using floating window (or any layout).
2. Fast file and buffer search using fuzzy match and preview.
3. Search through all the project files and history.
4. Real time preview of the selected file.
5. Searching from file history file using oldfiles or mru.
6. File search from git status with diff preview.
7. It is possible to interactively execute grep from within the project by specifying the directory
8. Highlight code in preview with bat or ccat. (Optional)
9. Export fzf candidates to QuickFix

## Demo

### Project Files

![fzf-preview](https://user-images.githubusercontent.com/5423775/73932616-18138380-491e-11ea-9078-de222fb47998.gif "fzf-preview")

### Git Status

![fzf-preview](https://user-images.githubusercontent.com/5423775/73932624-1ba70a80-491e-11ea-8594-de71558fae75.gif "fzf-preview")

### Project Grep

![fzf-preview](https://user-images.githubusercontent.com/5423775/73932630-1ea1fb00-491e-11ea-8547-4fd68e45857b.gif "fzf-preview")

### Export QuickFix

![fzf-preview](https://user-images.githubusercontent.com/5423775/74020208-68e9b180-49dc-11ea-9cbb-6e7423d065df.gif "fzf-preview")

## Requirements

- **Neovim** <https://neovim.io/>
- git <https://git-scm.com/>
- fzf <https://github.com/junegunn/fzf>

### Optional

- **bat (A cat clone with syntax highlighting)** (Recomended) <https://github.com/sharkdp/bat>
- **ccat (Colorizing cat)** (Recomended) <https://github.com/jingweno/ccat>
- **Python3 (Used grep preview)** (Recomended) <https://www.python.org/>
- **file-line (Enable opening a file in a given line)** (Recomended) <https://github.com/bogado/file-line>
- **ripgrep (Require FzfPreviewProjectGrep and FzfPreviewDirectoryFiles)** (Recommended) <https://github.com/BurntSushi/ripgrep>
- neomru.vim (Require FzfPreviewProjectMruFiles and FzfPreviewMruFiles) <https://github.com/Shougo/neomru.vim>
- vim-bookmarks (Require FzfPreviewBookmarks) <https://github.com/MattesGroeger/vim-bookmarks>
- vim-jumptoline (Require FzfPreviewJumpToLine) <https://github.com/rbtnn/vim-jumptoline>
- exa (Can be used to color the file list) <https://github.com/ogham/exa>
- vim-devicons (Use devicons) <https://github.com/ryanoasis/vim-devicons>

When bat or ccat is installed you can highlight the preview and see it.

Otherwise, head will be used

## Installation

Use [Dein](https://github.com/Shougo/dein.vim), [vim-plug](https://github.com/junegunn/vim-plug) or any Vim plugin manager of your choice.

If you are using MacOS and installed fzf using Homebrew
suffice:

```vim
set  runtimepath+=/usr/local/opt/fzf
call dein#add('yuki-ycino/fzf-preview.vim')
```

You install fzf as well using Dein:

```vim
call dein#add('junegunn/fzf', { 'build': './install --all', 'merged': 0 })
call dein#add('yuki-ycino/fzf-preview.vim')
```

## Usage

### Command

```vim
:FzfPreviewProjectFiles               " Select project files

:FzfPreviewGitFiles                   " Select file from git ls-files

:FzfPreviewDirectoryFiles             " Select file from current directory files (Required [ripgrep](https://github.com/BurntSushi/ripgrep))

:FzfPreviewGitStatus                  " Select git status listed file

:FzfPreviewBuffers                    " Select buffers

:FzfPreviewProjectOldFiles            " Select project files from oldfiles

:FzfPreviewProjectMruFiles            " Select project mru files with neomru

:FzfPreviewProjectGrep {word or none} " Grep project files from args word (Required [Python3](https://www.python.org/) and [file-line](https://github.com/bogado/file-line) plugin)

:FzfPreviewOldFiles                   " Select files from oldfiles

:FzfPreviewMruFiles                   " Select mru files from neomru

:FzfPreviewQuickFix                   " Select line from QuickFix (Required [Python3](https://www.python.org/) and [file-line](https://github.com/bogado/file-line) plugin)

:FzfPreviewLocationList               " Select line from LocationList (Required [Python3](https://www.python.org/) and [file-line](https://github.com/bogado/file-line) plugin)

:FzfPreviewBookmarks                  " Select bookmark from vim-bookmarks (Required [Python3](https://www.python.org/) and [file-line](https://github.com/bogado/file-line) plugin)

:FzfPreviewJumpToLine                 " Select the window to open from quickfix using [vim-jumptoline](https://github.com/rbtnn/vim-jumptoline)

:FzfPreviewFromResources              " Select files from selected resources (project, git, directory, buffer, project_old, project_mru, old, mru)
```

### Function

```vim
call fzf_preview#window#create_centered_floating_window() " Function to display the floating window used by this plugin

" Example
call fzf#run({
\ 'source':  files,
\ 'sink':   'edit',
\ 'window': 'call fzf_preview#window#create_centered_floating_window()',
\ })
```

## Keymap

```text
<C-x>
  Open split

<C-v>
  Open vsplit

<C-t>
  Open tabedit

<C-q>
  Build QuickFix

<C-d>
  Preview page down

<C-u>
  Preview page up

?
  Toggle display of preview screen

DEPRECATED
<C-s> (Neovim Only)
  Toggle window size of fzf, normal size and full-screen
```

## Optional Configuration Tips

- Increase the size of file history:

```vim
" oldfiles uses viminfo, but the default setting is 100
" Change the number by setting it in viminfo with a single quote.
" Ref: viminfo-'
set viminfo='1000
```

- Set values for each variable. The default settings are as follows.

```vim
" Add fzf quit mapping
let g:fzf_preview_quit_map = 1

" Use floating window (for neovim)
let g:fzf_preview_use_floating_window = 1

" Commands used for fzf preview.
" The file name selected by fzf becomes {}
let g:fzf_preview_command = 'head -100 {-1}'                       " Not installed ccat and bat
" let g:fzf_preview_command = 'bat --color=always --style=grid {-1}' " Installed bat
" let g:fzf_preview_command = 'ccat --color=always {-1}'             " Installed ccat

" Commands used for binary file
let g:fzf_binary_preview_command = 'echo "{} is a binary file"'

" Commands used to get the file list from project
let g:fzf_preview_filelist_command = 'git ls-files --exclude-standard'               " Not Installed ripgrep
" let g:fzf_preview_filelist_command = 'rg --files --hidden --follow --no-messages -g \!"* *"' " Installed ripgrep

" Commands used to get the file list from git reposiroty
let g:fzf_preview_git_files_command = 'git ls-files --exclude-standard'

" Commands used to get the file list from current directory
let g:fzf_preview_directory_files_command = 'rg --files --hidden --follow --no-messages -g \!"* *"'

" Commands used to get the git status file list
let g:fzf_preview_git_status_command = "git status --short --untracked-files=all | awk '{if (substr($0,2,1) !~ / /) print $2}'"

" Commands used for project grep
let g:fzf_preview_grep_cmd = 'rg --line-number --no-heading'

" Commands used for preview of the grep result
let g:fzf_preview_grep_preview_cmd = expand('<sfile>:h:h') . '/bin/preview_fzf_grep'

" Keyboard shortcuts while fzf preview is active
let g:fzf_preview_preview_key_bindings = 'ctrl-d:preview-page-down,ctrl-u:preview-page-up,?:toggle-preview'

" Specify the color of fzf
let g:fzf_preview_fzf_color_option = ''

" Keyboard shortcut for opening files with split
let g:fzf_preview_split_key_map = 'ctrl-x'

" Keyboard shortcut for opening files with vsplit
let g:fzf_preview_vsplit_key_map = 'ctrl-v'

" Keyboard shortcut for opening files with tabedit
let g:fzf_preview_tabedit_key_map = 'ctrl-t'

" Keyboard shortcut for building quickfix
let g:fzf_preview_build_quickfix_key_map = 'ctrl-q'

" Command to be executed after file list creation
let g:fzf_preview_filelist_postprocess_command = ''
" let g:fzf_preview_filelist_postprocess_command = 'xargs -d "\n" ls —color'          " Use dircolors
" let g:fzf_preview_filelist_postprocess_command = 'xargs -d "\n" exa --color=always' " Use exa

" Use vim-devicons
let g:fzf_preview_use_dev_icons = 0

" devicons character width
let g:fzf_preview_dev_icon_prefix_length = 2

" DEPRECATED
" fzf window layout
let g:fzf_preview_layout = 'top split new'

" DEPRECATED
" Rate of fzf window
let g:fzf_preview_rate = 0.3

" DEPRECATED
" Key to toggle fzf window size of normal size and full-screen
let g:fzf_full_preview_toggle_key = '<C-s>'
```

## Inspiration

- [Blacksuan19/init.nvim: An Opinionated Minimalist Neovim Configuration](https://github.com/Blacksuan19/init.nvim)

## License

The MIT License (MIT)
