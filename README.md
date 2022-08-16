# Trying out Hugo in the playground

I'm trying to change the static site generator for my Github pages from Jekyll to Hugo. Since I am yet unfamiliar with Hugo, I've been trying out Hugo on a demo site on my local machine.

## 1. Installing Hugo

Installing Hugo from the official deb package.

```bash
sudo apt install hugo
```

Also, [installing **_Go_** from the official website.](https://go.dev/doc/install) <br>
Since, Hugo is built on top of Golang, it's a requirement for when additional features are required. Better to have it installed from before.

## 2. Using the Terminal Theme

The default theme of Hugo is boring. Using Hugo in the first place was due to me being impressed by how cool the [Terminal theme](https://themes.gohugo.io/themes/hugo-theme-terminal/) looked.

Used the theme as a git submodule:

```bash
git submodule add https://github.com/panr/hugo-theme-terminal.git themes/terminal
```

First thing I noticed that the default theme was _orange_ and the entire page was _left_ aligned. <br>
That needed to be rectified. Time to play around with some configuration.

In the [official page of the Terminal theme,](https://themes.gohugo.io/themes/hugo-theme-terminal/) the [configuration details](https://themes.gohugo.io/themes/hugo-theme-terminal/#how-to-configure) are given.

Playing around with those parameters.

```toml
  themeColor = "blue"
  centerTheme = true
  autoCover = true

[params.twitter]
  creator = "biplobmanna"
  site = "biplobmanna"

[languages]
  [languages.en]
    languageName = "English"
    title = "Playground-Hugo"
    subtitle = "Playing around in Hugo on a simple demo site"
    owner = "biplobmanna"
    keywords = ""
    copyright = "biplobmanna"
    menuMore = "Show more"
    readMore = "Read more"
    readOtherPosts = "Read other posts"
    newerPosts = "Newer posts"
    olderPosts = "Older posts"
    missingContentMessage = "Page not found..."
    missingBackButtonLabel = "Back to home page"

    [languages.en.menu]
      [[languages.en.menu.main]]
        identifier = "about"
        name = "About"
        url = "/about"
      [[languages.en.menu.main]]
        identifier = "posts"
        name = "Posts"
        url = "/posts"
```

Some things which I did not figure out yet:

* `autoCover` which automatically uses an image for the cover
* where _twitter cards_ are used?

There were some things which I did not like either:

* The copyright information in the footer.
  * Initially, changed the details inside the `themes/terminal/layouts/partials/footer.html` file
  * Later found out that inside the project `layouts/partials/footer.html` overrides the theme files
  * Changed it to the latter, and reverted the changes inside in the theme files

## 3. Hugo Project Folder Structure

Hugo has a [set folder structure,](https://gohugo.io/getting-started/directory-structure/) with a huge number of customization in place for granular fine tuning.

Folder Structure:

```txt
.
├── archetypes
├── assets
├── config
.   ├── _default
.   ├── staging
.   └── production
├── content
├── data
├── layouts
├── static
├── themes
└── config.toml
```

## 4. Checking out different types of content

By default, all contents are posts, and the terminal theme also supports the same. However, I have a need for multiple content types in my own blog, so there must be a way to do that.

### 4.1. Create different types of content

There are no _archetypes_ for articles, blog. The _posts_ archetype is provided by the theme. <br>
Creating _archetypes_ for articles, blog, and posts in the `./archetypes/` project path.

By default, only _posts_ are shown on the homepage. Need to show all the pages. <br>
To do this, need the [Hugo template Engine](https://gohugo.io/templates/introduction/)

Let's modify the _homepage_ so that it lists all pages.

By default, the _layouts_ are rendered from the `theme/terminal/layouts` <br>
Overwriting that in `layouts` <br>

* `Paginator` or `Paginate` is static, i.e, once instance is declared, it persists
  * [Detains on Pagination](https://gohugo.io/templates/pagination/)
  * caused a lot of issue while trying to put all `RegularPages` into `Paginator`
  * to override the `Paginator` from the theme, renamed `theme/terminal/layouts` and reverted once new build was done
  * used logic `section != null` so that all pages of all sections are auto-populated

`config.toml` showing all sections toggle:
![alter config.toml parameters](/static/img/screenshot-10-08-2022-playground-hugo-vscode-config-show-sections.png "show all sections config.toml param") <br>

`index.html` showing logic for toggle
![homepage logic](/static/img/screenshot-10-08-2022-playground-hugo-vscode-homepage-logic.png "homepage logic")

* Added logic to `index.html` to show all pages, or pages of single section on the Home Page.

All pages on Homepage:
![All 4 Pages shown in the Homepage](/static/img/screenshot-10-08-2022-hugo-playground-homepage-all-pages.png "All pages on Home Page")

Pages of only Posts:
![Pages of only Posts](/static/img/screenshot-10-08-2022-hugo-playground-homepage-single-page.png "Pages of only Posts")

* Be careful of [Hugo Layout Lookup Order](https://gohugo.io/templates/lookup-order/)
* Funky use of [where](https://gohugo.io/functions/where/) and [in](https://gohugo.io/functions/in/#readout) functions.
* [Adding different sections is as simple as adding folders/subfolders](https://gohugo.io/content-management/sections/) inside `content/`
