---
title: "Automatic mapping of Formatter, Linter in mason.nvim"
date: "2022-11-25T21:54:31+09:00"
draft: false
comments: true
socialShare: true
toc: false
tags: ["neovim", "mason.nvim", "null-ls"]
---

I'm going to write down that `mason-lspconfig` will automatically map LSPs, as the help shows how to do it and it's quick and easy, but Formatter, Linter requires a bit of reading the documentation.
Use `null-ls` for formatting.


<pre class="line-numbers language-lua">
<code>
local mason = require("mason")
local mason_package = require("mason-core.package")
local mason_registry = require("mason-registry")
local null_ls = require("null-ls")


mason.setup({})

local null_sources = {}

for _, package in ipairs(mason_registry.get_installed_packages()) do
        local package_categories = package.spec.categories[1]
        if package_categories == mason_package.Cat.Formatter then
                table.insert(null_sources, null_ls.builtins.formatting[package.name])
        end
        if package_categories == mason_package.Cat.Linter then
                table.insert(null_sources, null_ls.builtins.diagnostics[package.name])
        end
end

null_ls.setup({
        sources = null_sources,
})
</code>
</pre>

## About: PackageSpec

<pre class="line-numbers language-lua">
<code>
require("mason-registry").get_installed_packages()
</code>
</pre>

to receive the installed packages as an array of Package.
`PackageSpec` can be received via Package.spec, which contains an array of
- name
- desc
- homepage
- categories
- languages
- install

The values are contained as an array.
If you are using `formatter.nvim` or similar, you will need a language name such as lua: stylua, which can be taken from here.
