---
title: Copy Paste Cut for Vim
date: 2015-11-10 12:48:26
categories:
- Linux
tags:
- vim
- copy
- paste
- cut
---
#### General Usage
##### Cut and Paste

1. Position the cursor where you want to begin cutting.
2. Press `V` to select characters (or uppercase `V` to select whole lines, or `Ctrl-v` to select rectangular blocks).
3. Move the cursor to the end of what you want to cut.
4. Press `d` to cut (or `y` to copy).
5. Move to where you would like to paste.
6. Press `P` to paste before the cursor, or `p` to paste after.

##### Copy and Paste
*Copy and paste* is performed with the same steps above except for step 4 where you would press `y` instead of `d`:
> `d` stands for `delete` in Vim, which in other editor is usually called cut

> `y` stands for `yank` in Vim, which in other editor is usually called copy

#### Other Usage

For search, you can do like this, `1,$ s/ccc/rrr/g`, it will replace `ccc` with `rrr` characters.


more about this, pls refer to [here][source-addr].

[source-addr]: http://vim.wikia.com/
