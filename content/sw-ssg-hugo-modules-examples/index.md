---
title: Hugo Modules Additional Examples
type: article 
date: 2022-11-21
series: Sharing Knowledge
categories: ['Hugo']
tags: ['Static Site', 'Hugo', 'Git']
draft: True
toc: false 
comments: false 
---


Let’s take a look at some examples of site configurations using Hugo modules.

## Adding a theme as a dependency

A major dependency of a site is of course it's theme. 

So far we have added a [Hugo Theme Module](sw-ssg-hugo-modules-theme ) to our [Hugo Module](sw-ssg-hugo-modules) based site.

Quickly recapping, To initialize the site as a module and add the theme we run the following commands from the site’s root directory.

```console
$ hugo mod init github.com/DamianFlynn/hugo
go: creating new go.mod: module github.com/DamianFlynn/hugo

$ hugo mod get github.com/DamainFlynn/hugo-bilberry-theme@master
go: downloading github.com/DamainFlynn/hugo-bilberry-theme v0.0.0-20211129195303-72aefc1d4844
go: github.com/DamainFlynn/hugo-bilberry-theme master => v0.0.0-20211129195303-72aefc1d4844
```

Add a new section in the configuration file,

```yaml
# config.yaml
title: "My New Hugo Site"
# …
module:
  imports:
    - path: github.com/DamianFlynn/hugo-bilberry-theme

```

It is recommended to remove the `theme` option from the site configuration when using modules.

## Using mounts for specific files

Let’s add a layout file to our site which is a [Markdown render hook](https://gohugo.io/getting-started/configuration-markup/#markdown-render-hooks). The module `github.com/bep/portable-hugo-links` is a complete site. We will just use a specific file from it.

```console
$ hugo mod get github.com/bep/portable-hugo-links@latest
go: downloading github.com/bep/portable-hugo-links v0.5.3
go: github.com/bep/portable-hugo-links latest => v0.5.3
```

The site configuration,

```yaml
module:
  imports:
    # …
    - path: github.com/bep/portable-hugo-links
      mounts:
        - source: layouts/_default/_markup/render-link.html
          target: layouts/_default/_markup/render-link.html
```

The above import specific mount section takes the `render-link.html` from the module and makes it available at the same path in our site.

We can mount any other files from our site’s directory on a different path within our site.

```yaml
module:
  # …
  mounts:
  - source: README.md
    target: content/about/index.md
```

This will create a `/about` page using `README.md` file which is in the site root.

### Using files from non Hugo repositories

We will add jQuery (`jquery.js`) to our site at `static/js/jquery.js`.

```console
$ hugo mod get github.com/jquery/jquery-dist@3.5.1
go: downloading github.com/jquery/jquery-dist v0.0.0-20200504225046-4c0e4becb826
go: github.com/jquery/jquery-dist 3.5.1 => v0.0.0-20200504225046-4c0e4becb826
```

The site configuration,

```yaml
module:
  imports:
    # …
    - path: github.com/jquery/jquery-dist
      mounts:
        - source: dist/jquery.js
          target: static/js/jquery.js
```

With this change, `/js/jquery.js` will be available on the site.

### Combining multiple themes / modules

It is possible that a same file is present in two modules. In such cases, the file is taken from the module which appears first in the list.

Consider the following site configuration. Both `gkskt-blogstarters` and `blogstarters` have the file `archetypes/first-post.md`.

```yaml
module:
  imports:
    - path: gkskt-blogstarters
    - path: blogstarters
```

The `first-post.md` is actually taken from `gkskt-blogstarters` in above case. This way we can combine multiple themes as well.

## Other hints and references

# Local module development


When making changes to a theme or module, we often want to test it with our site. The [replace directive](https://golang.org/ref/mod#go-mod-file-replace) of Go Modules can be used.

We can add a line in the `go.mod` file like,

```
replace github.com/tummychow/lanyon-hugo => /home/bhavin/src/blog/lanyon-hugo
```

The filesystem path on the right side of **=>** needs to have a `go.mod` file in it. It can be created with `go mod init`.

The same thing can be achieved with the [module configuration option `replacements`](https://gohugo.io/hugo-modules/configuration/#module-config-top-level) and the environment variable `HUGO_MODULE_REPLACEMENTS`.

At this point in the [Master Hugo Modules](https://www.hugofordevelopers.com/series/master-hugo-modules/) series, you’re ready to start developing your own modules. But the typical development workflow involves git pushing and pulling on both the module and parent project side: an update requires a push on the module side, and also a pull on the parent project side to localize the module changes. Especially early in the development and testing of a module, this is inefficient and slows down rapid iteration.

But with a simple configuration change, it’s easy to develop a module locally. You’ll avoid git push and pulls, and even see changes hot-swapped in your Hugo development environment. Let’s take a closer look.

## The standard Hugo module development workflow

Here’s the typical workflow when developing a module:

1.  Make a change in the module,
2.  Commit and push the change,
3.  In the parent project, pull the module update with `hot mod get`; the mod update will need to be committed as well,
4.  Test and QA on the parent project development server,
5.  Wash, rinse, and repeat as needed.

## Local Hugo module development workflow

Fortunately, there’s an alternative: [you can make and test changes in a module locally](https://gohugo.io/hugo-modules/use-modules/#make-and-test-changes-in-a-module), by routing your parent project’s dependency reference to a local resource. Crack open your project’s `go.mod`, and add the following:

```
replace gitlab.com/<path-to-module>/<module-name> => <directory path as needed>/<module-name>
```

You can route the local directory wherever you like, e.g.: `replace gitlab.com/<path-to-module>/<module> => ../../<module>`

With this in place, you can make changes to your local module, save, and immediately see the results in your parent project. And this really is immediate: modules will be hot replaced and reflected in your development environment without a refresh.

With local development, your workflow becomes:

1.  Make changes to the module locally and save,
2.  Test and QA on the parent project development server,
3.  Iterate as needed,
4.  Commit the module update to its repo when the feature is complete.

Don’t forget to remove the local module override when the parent project goes live.

## Develop your Hugo modules using feature branching

Thanks to this method, it becomes easy to develop and test a module using feature branches in git — something you can’t readily do without the local override. Here, the workflow is:

5.  In your module repo, create your feature branch,
6.  In your local module, check out the feature branch,
7.  Iterate using local development as above,
8.  When the module feature is complete, merge the branch to master,
9.  And continue to check out a new feature branch in your local module, or check out the updated master branch.

## That’s it.

Developing Hugo modules locally saves a ton of time in git pushes, pulls, and server latencies.
## The Cache settings

Hugo uses `/tmp` directory to cache the downloaded modules. This may get cleaned on system reboot. To avoid this, it can be set to some other directory with the help of [`caches` option](https://gohugo.io/getting-started/configuration/#configure-file-caches) in site configuration. Take a look at [this comment](https://discourse.gohugo.io/t/hugo-modules-for-dummies/20758/8) by bep for more information.