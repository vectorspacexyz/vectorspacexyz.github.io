---
title: neovim
comments: true
---
## set tw=80 for markdown files
file: `~/.config/nvim/lua/core/autocmds.lua`

```lua
local group = vim.api.nvim_create_augroup("VectorClearAu", { clear = true })

vim.api.nvim_create_autocmd('FileType', {
  pattern = "markdown",
  callback = function()
    vim.opt.tw = 80
  end,
  group = group,
})
```

## Autocommands
source: [TJ Devries YT](https://www.youtube.com/watch?v=ekMIIAqTZ34)

test.lua:
```lua
vim.api.nvim_create_autocmd('BufEnter', { command = "echo 'Hello'", })
```

`BufEnter` is an event. Autocommands are tied to events. To see more about events `:h events`

To check autocmds tied to a particular event: `:au BufEnter`

To clear autocmds attached to particular event: `au! BufEnter`

As it is, if `test.lua` file is executed again, it greates another autocmd. So "Hello"
gets print twice with every `BufEnter` event. This is why `augroup` is used.


Creating `augroup`:

```lua
local group = vim.api.nvim_ceate_augroup("SmashThatLikeButton", { clear = true })
-- if it was { clear = false } we'd get the old behavior
vim.api.nvim_create_autocmd('BufEnter', { command = "echo 'Hello 1'", group = group })
vim.api.nvim_create_autocmd('BufEnter', { command = "echo 'Hello 2'", group = group })
```

Now every time the file gets created, the autogroup gets cleared before its created,
and with it the autocmds part of the group also gets cleared.

Now, this might be the feature that we're gonna use more often —lua functions triggered
by `events`.

```lua
local group = vim.api.nvim_ceate_augroup("SmashThatLikeButton", { clear = true })
-- if it was { clear = false } we'd get the old behavior
vim.api.nvim_create_autocmd('BufEnter', { command = "echo 'Hello 1'", group = group })
vim.api.nvim_create_autocmd('BufEnter', { callback = function()
  print("Hello")
end, group = group })
```

## Screenshots to markdown
This script both gets the latest file in ~/Pictures where my screenshots are and then copies them to `img/` folder
in the same folder where the file is being edited.

```lua
local M = {}

M.insert_screenshot_markdown = function()
  local bufno = vim.api.nvim_get_current_buf()
  local winid = vim.fn.bufwinid(bufno)
  local path = vim.api.nvim_buf_get_name(bufno)
  local pathtofile = vim.fn.fnamemodify(path, ':h')
  local img_dir = pathtofile .. "/img"

  local stat = vim.loop.fs_stat(img_dir)
  local dir_exists = stat and stat.type == 'directory'
  if not dir_exists then
    local success, err = vim.loop.fs_mkdir(img_dir, 511)
    if not success then
      print("Unable to create dir")
      return 1
    end
  end

  local directory = vim.fn.expand('~/Pictures')
  local cmd = "ls -t " .. directory .. " | head --lines 1"

  local newest_file = vim.fn.system(cmd)
  newest_file = newest_file:gsub("\n", "")

  local cmd2 = "cp " .. directory .. "/" .. newest_file .. " " .. img_dir
  vim.fn.system(cmd2)

  if newest_file ~= "" then
    local markdown_link = string.format("![%s](img/%s)", newest_file, newest_file)
    local row, col = unpack(vim.api.nvim_win_get_cursor(winid))
    local lines = vim.api.nvim_buf_get_lines(bufno, row - 1, row, false)
    -- Check if the 'lines' table is empty
    local line = ""
    if #lines ~= 0 then
      line = lines[1]:gsub("\n", " ")
    end
    local new_line = line .. markdown_link
    vim.api.nvim_buf_set_lines(bufno, row - 1, row, false, { new_line })
    vim.api.nvim_win_set_cursor(winid, { row, col + #markdown_link })
  end
end

return M
```

The above script is placed in ~/plugins and it gets picked up by neovim lazy.nvim because of this mention
in `~/.config/nvim/lua/plugins/vector.lua`:

```
{
 dir = "/home/vector/plugins/markdown-screenshot.nvim",
 config = function()
   vim.keymap.set({ 'n', 'i' }, '<Leader>p',
     function()
       require("markdown-screenshot").insert_screenshot_markdown()
     end,
     { desc = "Paste screenshot" })
 end
},

```


## Scripting workflow
```
:vnew ~/.config/nvim/lua/development/ss.lua
:echo nvim_get_current_buf()
1
local bufno = 1
local path = vim.api.nvim_buf_get_name(bufno)
```

## Getting treesitter root
```lua
local parser = vim.treesitter.get_parser(5, "dart", {})
local tree = parser:parse()[1]
local root = tree:root()
```

## Visual range
```lua
local cursorpos = vim.fn.getpos('.')
local vstart = vim.fn.getpos('v')
-- { 0, 31, 4, 0 jo}
-- row: cursorpos[2] row: vstart[2]
-- col: curspor[3], col: vstart[3]
```

```
visual range: getpos('v') -- [2] and [3]
nvim_win_get_cursor(0) [1] and [2]
```

## Note

```
vim.fn.getpos('v')
```

to

```
vim.fn.getpos('.')
```

is visual selection in vim. Note the return value

```lua
local result = {} --string[]
local op1 = vim.fn.getpos('v')
local op2 = vim.fn.getpos('.')
table.insert(result, vim.inspect(op1))
table.insert(result, vim.inspect(op2))
vim.api.nvim_buf_set_text(19, 0, -1, result)
-- note 19 is output buffer
```
