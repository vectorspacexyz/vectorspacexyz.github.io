---
title: vimwiki
date: 2024-02-14
comments: true
---

Mkdocs for material has replaced vimwiki for me. But I still use it for the useful bindings it sets. But
some of its bindings conflicted with those of other plugins, notably `oil.lua`. My `vimwiki.lua` that addresses
this issue:

```lua
-- Define a function to remove the binding
local function remove_binding()
    vim.api.nvim_buf_set_keymap(0, 'n', '<YourBindingToRemove>', '<Nop>', {noremap = true, silent = true})
end

-- Set autocmd to trigger the function for Markdown files
vim.cmd[[
    augroup MarkdownSettings
        autocmd!
        autocmd FileType markdown lua remove_binding()
    augroup END
]]
```

source: [reddit](https://www.reddit.com/r/neovim/comments/14m9vjo/how_could_i_disable_keybinds_with_vimwiki/)
