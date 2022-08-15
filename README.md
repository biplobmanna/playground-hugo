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

