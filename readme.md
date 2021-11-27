# Style Guide

When possible, split each topic into its own Markdown file.

# Purpose

This is intended for *me.* It's a "cheatsheet" notebook of things I would like to remember about Ruby and Rails.

If it's useful to anybody else, it would be a happy miracle.

You may notice that large chunks of Ruby and Rails are not covered. They are the parts I know well, thanks.

# Syntax Highlighting in Sublime Text

Prefer the "Markdown (Extended)" syntax in order to get syntax highlighting within code blocks.

https://github.com/jonschlinkert/sublime-markdown-extended

The supported languages are:  (erb seems to work too... are there others?)

- coffee|coffeescript
- coffee front matter
- cpp
- csharp
- css
- c
- c++
- diff
- ejs
- erlang
- underscore
- go
- graphql
- lodash
- handlebars|hbs: requires the Sublime Text Handlebars package
- html|html5
- ini
- jade
- java
- javascript|js
- json
- json front matter
- julia
- less
- ls|livescript|LiveScript
- lua
- md|markdown
- nginx
- objective-c
- objective-c++
- perl
- r
- ruby
- sass
- scala
- scss
- shell
- bash
- sql|ddl|dml
- postgresql|postgres|pgsql
- styl
- swift
- swig
- liquid
- xml
- yaml
- yaml front matter

# Color Schemes in Sublime Text

Many color schemes in Sublime Text don't have great support for Markdown out of the box.

You can add it with `Preferences` -> `Customize Color Scheme` and adding something like the following.

Below are my customizations for the "Oceanic Next" color scheme. **TODO:** Make this more general across other color schemes.

  ```json
  {
    "variables":
    {
    },
    "globals":
    {
    },
    "rules":
    [
      {
        "name": "Markdown Code Blocks",
        "scope": "markup.raw.block.fenced.markdown, variable.language.fenced.markdown, punctuation.definition.fenced.markdown",
        "background": "rgba(255, 255, 255, 0.025)",
        "foreground": "#acb4c2"
      },
      {
        "name": "Markdown Code Fences",
        "scope": "punctuation.definition.fenced.markdown, variable.language.fenced.markdown",
        "foreground": "#65737e",
      },
      {
        "name": "Markdown Headings",
        "scope": "markup.heading",
        "foreground": "#ebc98f",
        "background": "rgba(151, 243, 247, 0.22)",
        "font_style": "glow",
      },
      {
        "name": "Comments",
        "scope": "comment, punctuation.definition.comment",
        "font_style": "italic"
      }
    ]
  }
  ```