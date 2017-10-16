---
title: Getting Started
menu:
  sidebar:
    parent: section-1
    weight: 2
---

## Quick Start
To get started, clone the repo into the `themes/` folder of your Hugo project. This can be done by running the following command from the root directory of your Hugo site:

```
$ git clone git@github.com:forestryio/sapling-theme themes/sapling-theme.git
```

You can copy the contents of `exampleSite/` to the root of your Hugo project to get a guided tutorial of the theme, as well as default archetypes and integration with [Forestry.io][1].

To make changes to the CSS or Javascript of the theme, see the [Webpack Asset Pipeline](#webpack-asset-pipeline) section.

## Menus

Sapling has two menus:

- [Header Nav](#setting-up-the-header-nav)
- [Sidebar](#setting-up-the-sidebar)

Menus can be configured from your config file (e.g, `config.toml`) or directly in content files.

### Setting Up The Header Nav

When adding items via the config file, you must provide the `Name` field and a `Url` field. An optional `Weight` field can be defined to set the order of menu items.

``` config.toml
[[menus.nav]]
  Name = "Pretty Name"
  Url = "page-slug"
  Weight = 1
```

When adding items via content files, you don't have to specify a `Name` or `URL` field, as they are automatically resolved from the page. `Weight` can be optionally defined to set the order of menu items.

```_index.toml
[[menus.nav]
  Weight = 2
```

### Setting Up The Sidebar

The sidebar can be configured in three ways:

- [Global Sidebar](#global-sidebar)
- [Homepage Sidebar](#homepage-sidebar)
- [Section Sidebars](#section-sidebars)

The sidebar is separated into headings and links. You must define headings for the sidebar in your config file,and then nest links under them. This is done using the `Identifier` key on headings, and the `Parent` key on links or content files.

#### Global Sidebar 

The global sidebar is configured by adding menu items to the `sidebar` menu. This menu will be displayed on all pages, unless the homepage or a specific section menu override is defined.

```config.toml
[[menus.sidebar]]
  Name = "Heading 1"
  Identifier = "heading-1"
  Weight = 1
```

```_index.md
[[menus.sidebar]]
  Parent = "heading-1"
  Weight = 1
```

#### Homepage Sidebar

The homepage sidebar is configured by adding menu items to the `homepage` menu. This menu will be displayed on the hompage instead of the `sidebar` menu if defined.

```config.toml
[[menus.homepage]]
  Name = "Heading 1"
  Identifier = "heading-1"
  Weight = 1
```

```_index.md
[[menus.homepage]]
  Parent = "heading-1"
  Weight = 1
```

#### Section Sidebars

Section sidebars are configured by adding menu items to a menu matching the slug of the section. For example, with a section located at `content/documentation` the menu is `documentation`.

 This menu will be displayed on the hompage instead of the `sidebar` menu if defined.

```config.toml
[[menus.homepage]]
  Name = "Heading 1"
  Identifier = "heading-1"
  Weight = 1
```

```_index.md
[[menus.homepage]]
  Parent = "heading-1"
  Weight = 1
```

## Git Integration

Sapling has two levels of Git integration. The first is support for links to the repository, and to the follow page of the repository.

This is configured via the config file, under the site's `Params`. Provide the repository `id` for your repo, as well as as provider: `github`, `bitbucket`, `gitlab`.

```config.toml
[Params.repo]
id = "username/repo-id"
provider = "github"
```

The second level of integration is support for commit tracking, allowing users to see when the last change was made to a content file, and link to the commit. This is enabled by setting `enableGitInfo` to `true` in your config file.

This requires the Git CLI to be installed and in your system's `$PATH` variable. [More information available in the Hugo docs][2].

```config.toml
enableGitInfo = true
```

## Setting Up Search

The search in Sapling is fairly plug-and-play. It works by generating a search index file at `/index.json`. This requires you to enable the JSON custom output format for the homepage in your config.

```config.toml
[outputs]
home = ["HTML", "JSON"]
```

## Versioning

Sapling supports versioning of your docs by providing the links to different revisions in your config file. Doing so generates a version select in the sidebar that will redirect visitors to the different version.

This format allows you to handle versioning however you like, either by using `v1/`, `/v2/`, etc sections in your `content/` folder, or by having completely different Hugo projects for each version.

The `URL` field is passed through the `absURL` filter. This ensures that both relative links within your Hugo project as well as external links work correctly.

```config.
[[params.versions]]
Name = "v1.0.0"
URL = "/1.0.0/"
```

## Code Highlighting

Code highlighting can be enabled using Hugo's built-in support for [pygments][3], or using [Highlight.js][4]. Both are enabled via your config.

```config.toml
PygmentsCodeFences = true // Enable pygments
[Params]
Highlightjs = true // Enable Highlight.js
```

Note: support for `pygmentsuseclasses = true` is not included. You must add your own CSS styling if you wish to use classes instead of inline-styles.

## Code Tabs

Sapling includes a two shortcodes, `tabs` and `tab`, for doing multi-language code snippets. This can be used in the markdown body of content files.

You provide the `tabs` shortcode with the list of language tabs you're using, and then use the `tab` field with the specific language. See below:

```_index.toml
+++
title = "Sapling Documentation Theme"
+++
```

## A code tab example

{{< tabs "Ruby" "Python" "Javascript" >}}
{{< tab "Ruby" >}}
puts 'Hello, world!'
{{< /tab >}}
{{< tab "Python" >}}
print("Hello, world!")
{{< /tab >}}
{{< tab "Javascript" >}}
console.log("Hello, world!")
{{< /tab >}}
{{< /tabs >}}

```
{{</* tabs "Ruby" "Python" "Javascript" */>}}
  {{</* tab "Ruby" */>}}
    puts 'Hello, world!'
  {{< /tab >}}
  {{</* tab "Python" */>}}
    print("Hello, world!")
  {{< /tab >}}
  {{</* tab "Javascript" */>}}
    console.log("Hello, world!")
  {{< /tab >}}
{{< /tabs >}}
```

## Webpack Asset Pipeline

The CSS and JS for the app are compiled using Webpack and NPM.

To get started, be sure that you have the latest versions of node and npm installed.

Then, to compile a production ready JS and CSS bundle, run the following from `themes/sapling-theme`:

```
$ webpack -p
```

For development, you can have Webpack rebuild the CSS and JS bundles every time you make a change by running:

```
$ webpack --watch
```

### Javascript Modules

Webpack builds the JS bundle from `src/js/scripts.js`. Using Webpack, you get support for any browser-compatible NPM packages, and can require local JS files from `src/js/` as well.

```
var npm-module    = require('npm-module') // An npm module required from node_modules/
var local-module  = require('./local-module.js') // A local module required from src/js/
```

### Next generation CSS via CSSNext

Webpack builds the CSS bundle from `src/css/style.css`. CSS is preprocessed by using [CSSNext][5]. CSSNext allows for next-generation CSS today, such as CSS variables and CSS functions

[1]: https://forestry.io
[2]: https://gohugo.io/variables/git/
[3]: https://gohugo.io/tools/syntax-highlighting/#pygments
[4]: http://highlightjs.org
[5]: http://cssnext.io/features/
