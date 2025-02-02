<!-- markdownlint-disable MD033 MD041 -->
<div align="center">
    <img src="./assets/logo.svg">
    <h3>emmet-language-server</h3>
    <p>A language server for <a href="https://emmet.io/" target="_blank">emmet.io</a></p>
</div>

---

### Why another language server?

While [aca/emmet-ls](https://github.com/aca/emmet-ls) works for what I need, there were a couple of things that annoyed me from time to time and while trying to fix one of those things (aca/emmet-ls#55) I've discovered that we can leverage [microsoft/vscode-emmet-helper](https://github.com/microsoft/vscode-emmet-helper) and make a simple language server that wraps that package to provide completions.

So I decided to do that and it worked!

The most important thing is that [microsoft/vscode](https://github.com/microsoft/vscode) has an excellent integration with emmet and we can have that, in all editors that implement the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/).

### Setup

> **Warning**
> I've decided to rename the package to `@olrtg/emmet-language-server` for mason/lspconfig integration. Please remove the old `@olrtg/emmet-ls` package and migrate to this new one.

**Using Homebrew:**
```sh
 brew tap youngmoguler/emmet-language-server
 brew install emmet-ls
```

**Using npm:**

```sh
npm i -g @olrtg/emmet-language-server
```

**Using mason.nvim:**

```sh
:MasonInstall emmet-language-server
```

### Neovim

> **Note**
> Want deeper integration (eg. wrap with abbreviation)? Check out [nvim-emmet](https://github.com/olrtg/nvim-emmet).

**With nvim-lspconfig:**

Remember that if you don't need to support a new filetype or change the default settings of the language server you don't need to pass a table to the `setup` function (like this: `lspconfig.emmet_language_server.setup()`).

```lua
lspconfig.emmet_language_server.setup({
  filetypes = { "css", "eruby", "html", "javascript", "javascriptreact", "less", "sass", "scss", "svelte", "pug", "typescriptreact", "vue" },
  -- Read more about this options in the [vscode docs](https://code.visualstudio.com/docs/editor/emmet#_emmet-configuration).
  -- **Note:** only the options listed in the table are supported.
  init_options = {
    --- @type string[]
    excludeLanguages = {},
    --- @type table<string, any> [Emmet Docs](https://docs.emmet.io/customization/preferences/)
    preferences = {},
    --- @type boolean Defaults to `true`
    showAbbreviationSuggestions = true,
    --- @type "always" | "never" Defaults to `"always"`
    showExpandedAbbreviation = "always",
    --- @type boolean Defaults to `false`
    showSuggestionsAsSnippets = false,
    --- @type table<string, any> [Emmet Docs](https://docs.emmet.io/customization/syntax-profiles/)
    syntaxProfiles = {},
    --- @type table<string, string> [Emmet Docs](https://docs.emmet.io/customization/snippets/#variables)
    variables = {},
  },
})
```

**Without nvim-lspconfig:**

```lua
vim.api.nvim_create_autocmd({ "FileType" }, {
  pattern = "astro,css,eruby,html,htmldjango,javascriptreact,less,pug,sass,scss,svelte,typescriptreact,vue",
  callback = function()
    vim.lsp.start({
      cmd = { "emmet-language-server", "--stdio" },
      root_dir = vim.fs.dirname(vim.fs.find({ ".git" }, { upward = true })[1]),
      -- Read more about this options in the [vscode docs](https://code.visualstudio.com/docs/editor/emmet#_emmet-configuration).
      -- **Note:** only the options listed in the table are supported.
      init_options = {
        --- @type string[]
        excludeLanguages = {},
        --- @type table<string, any> [Emmet Docs](https://docs.emmet.io/customization/preferences/)
        preferences = {},
        --- @type boolean Defaults to `true`
        showAbbreviationSuggestions = true,
        --- @type "always" | "never" Defaults to `"always"`
        showExpandedAbbreviation = "always",
        --- @type boolean Defaults to `false`
        showSuggestionsAsSnippets = false,
        --- @type table<string, any> [Emmet Docs](https://docs.emmet.io/customization/syntax-profiles/)
        syntaxProfiles = {},
        --- @type table<string, string> [Emmet Docs](https://docs.emmet.io/customization/snippets/#variables)
        variables = {},
      },
    })
  end,
})
```

### Helix Editor

Install normally with npm/pnpm, then add the following to `languages.toml`:

```toml
[[language]]
name = "html"
language-server = { command = "emmet-language-server", args = ["--stdio"] }
```

### Credits

- [@aca](https://github.com/aca) for the first language server ([aca/emmet-ls](https://github.com/aca/emmet-ls))
- [@wassimk](https://github.com/wassimk) for bringing the [microsoft/vscode-emmet-helper](https://github.com/microsoft/vscode-emmet-helper) repo to my attention in aca/emmet-ls#55
- [microsoft/vscode](https://github.com/microsoft/vscode) for having such an amazing integration with emmet and the easy and open package to integrate with
- [emmetio/emmet](https://github.com/emmetio/emmet) for the awesome tool
