# create a new file in ~/.config/nvim/lua/plugins/flutter-tools.lua

```lua
return {
  "akinsho/flutter-tools.nvim",
  -- keys = {
  --   { "<leader>r", "<cmd> FlutterRun <CR>" },
  -- },
  opts = {
    fvm = true,
    ui = {
      border = "rounded",
      notification_style = "native",
    },
    decorations = {
      statusline = {
        app_version = false,
        device = true,
      },
    },
    widget_guides = {
      enabled = true,
    },
    closing_tags = {
      highlight = "ErrorMsg",
      prefix = "//",
      enabled = true,
    },
    lsp = {
      color = {
        enabled = false,
        background = false,
        foreground = false,
        virtual_text = true,
        virtual_text_str = "■",
      },
      settings = {
        showTodos = true,
        completeFunctionCalls = true,
        enableSnippets = true,
      },
    },
    debugger = {
      enabled = true,
      run_via_dap = true,
    },
    -- dev_log = {
    --   enabled = true,
    --   open_cmd = "tabedit", -- command to use to open the log buffer
    -- },
  },
}
```

## in ~/.config/nvim/lua/config/keymaps.lua
## add the following codes
```lua
local function map(mode, lhs, rhs, opts)
  local keys = require("lazy.core.handler").handlers.keys
  ---@cast keys LazyKeysHandler
  -- do not create the keymap if a lazy keys handler exists
  if not keys.active[keys.parse({ lhs, mode = mode }).id] then
    opts = opts or {}
    opts.silent = opts.silent ~= false
    vim.keymap.set(mode, lhs, rhs, opts)
  end
end

--flutter
map("n", "<leader>rf", "<cmd> FlutterRun <CR>", { desc = "Run Flutter Apps" })
map("n", "<leader>rfw", "<cmd> FlutterRun -t widgetbook/main.dart <CR>", { desc = "Run Flutter widgetbook" })
map("n", "<leader>rr", "<cmd> FlutterReload <CR>", { desc = "Reload Flutter Apps" })
map("n", "<leader>rR", "<cmd> FlutterRestart <CR>", { desc = "Restart Flutter Apps" })
map("n", "<leader>rl", "<cmd> FlutterLogClear <CR>", { desc = "Clear the log of Flutter Apps" })
map("n", "<leader>rd", "<cmd> FlutterDevices <CR>", { desc = "Check available device" })
map("n", "<leader>rq", "<cmd> FlutterQuit <CR>", { desc = "Stop Running Application" })
map("n", "<leader>rt", "<cmd> !flutter_test.sh %:p<CR>", { desc = "run flutter test on current file" })
map("n", "<leader>gt", function()
  package.loaded["fcreate"] = nil
  require("fcreate").cbx()
end, { desc = "flutter create test file" })
map("n", "<leader>rw", function()
  local current_directory = vim.fn.expand("%:p:h")
  -- Get the filename from user input
  local filename = vim.fn.input("Enter the name of the new file: ")
  -- Execute the bash script with current_directory and filename as arguments
  local command = "! /home/skypea/myScript/bin/new_widget.sh "
    .. vim.fn.shellescape(current_directory)
    .. " "
    .. vim.fn.shellescape(filename)
    .. " "
    .. vim.fn.shellescape("%:p")
  vim.cmd(command)
  -- Refresh the file explorer (if you're using any plugin like NERDTree)
  vim.cmd("silent! NERDTreeRefreshRoot")
  -- Add a new line with the desired content to the current file
end, { noremap = true, silent = true, desc = "create a new widget file" })

```
