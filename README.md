# 🐚 Zsh Configuration

```zsh
PROMPT='%F{cyan}%~%f '
alias ls='ls --color=auto'

# ปิดสีไฮไลต์พื้นหลังเขียวของโฟลเดอร์ใน Windows
export LS_COLORS=$LS_COLORS:'ow=01;34:'

# เปิดใช้งานการแปลงตัวแปรใน PROMPT
setopt PROMPT_SUBST

# ล้างค่า ~d เดิม
unhash d 2>/dev/null

PROMPT='%F{cyan}${PWD/#\/mnt\/d/~}%f '

cd /mnt/d
```

# 💤 My Neovim Configuration

## ⚙️ Configuration

```lua
-- ==========================================
-- 1. การตั้งค่าพื้นฐาน (Basic Options)
-- ==========================================
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.tabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = true
vim.opt.termguicolors = true
vim.opt.cursorline = true
vim.opt.wrap = true
vim.g.mapleader = " "
vim.opt.textwidth = 80

-- ==========================================
-- 2. ติดตั้ง Plugin Manager (lazy.nvim)
-- ==========================================
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"

if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", lazypath,
  })
end

vim.opt.rtp:prepend(lazypath)

-- ==========================================
-- 3. รวมปลั๊กอินทั้งหมด (All Plugins)
-- ==========================================
require("lazy").setup({

  -- 🎨 Gruvbox Theme
  {
    "ellisonleao/gruvbox.nvim",
    priority = 1000,

    opts = {
      transparent_mode = true,
    },

    config = function(_, opts)
      require("gruvbox").setup(opts)
      vim.o.background = "dark"
      vim.cmd([[colorscheme gruvbox]])
    end,
  },

  -- 📊 Lualine
  {
    "nvim-lualine/lualine.nvim",
    dependencies = {
      "nvim-tree/nvim-web-devicons"
    },

    config = function()
      require("lualine").setup({
        options = {
          theme = "gruvbox"
        }
      })
    end,
  },

  -- 🌳 Treesitter
  {
    "nvim-treesitter/nvim-treesitter",
    build = ":TSUpdate",

    opts = {
      highlight = {
        enable = true,
      },
    },
  },

  -- 📁 NvimTree
  {
    "nvim-tree/nvim-tree.lua",
    dependencies = {
      "nvim-tree/nvim-web-devicons"
    },

    config = function()
      require("nvim-tree").setup()

      vim.keymap.set(
        "n",
        "<leader>e",
        ":NvimTreeToggle<CR>",
        { silent = true }
      )
    end,
  },

  -- 🔭 Telescope
  {
    "nvim-telescope/telescope.nvim",

    dependencies = {
      "nvim-lua/plenary.nvim"
    },

    config = function()
      local builtin = require("telescope.builtin")

      vim.keymap.set(
        "n",
        "<leader>ff",
        builtin.find_files,
        {}
      )

      vim.keymap.set(
        "n",
        "<leader>fg",
        builtin.live_grep,
        {}
      )
    end,
  },

  -- ⚡ Auto Completion
  {
    "hrsh7th/nvim-cmp",

    dependencies = {
      "hrsh7th/cmp-buffer",
      "hrsh7th/cmp-path",
      "L3MON4D3/LuaSnip",
    },

    config = function()
      local cmp = require("cmp")

      cmp.setup({
        snippet = {
          expand = function(args)
            require("luasnip").lsp_expand(args.body)
          end,
        },

        mapping = cmp.mapping.preset.insert({

          ["<C-Space>"] = cmp.mapping.complete(),

          ["<CR>"] = cmp.mapping.confirm({
            select = true
          }),

          ["<Tab>"] = cmp.mapping.select_next_item(),

        }),

        sources = cmp.config.sources({
          { name = "buffer" },
          { name = "path" },
        }),
      })
    end,
  },

})
```
