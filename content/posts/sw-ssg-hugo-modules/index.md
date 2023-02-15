---
title: Hugo Modules Introduction
type: article 
layout: post
description: Leverage agile frameworks to provide a robust synopsis for high level overviews. Iterative approaches to corporate strategy foster collaborative thinking to further the overall value proposition.
date: 2022-11-21
series: Sharing Knowledge
categories: ['Hugo']
tags: ['git', 'hugo', 'staticsite']
authors: ['damian']
draft: False
image: https://via.placeholder.com/1200x800
image_caption: Photo by [Oliver Sjöström](https://via.placeholder.com/1200x800) on [Unsplash](https://via.placeholder.com/1200x800)
toc: False
featured: True
comments: False
---


What are Hugo Modules and should you convert your Hugo site to be one?

The **Hugo Modules** feature was added to Hugo back in [July 2019](https://github.com/gohugoio/hugo/releases/tag/v0.56.0). It enables collecting different pieces of your Hugo site source from different repositories, each piece is a _module_. 

Modules can range from content to themes, providing one or more of the 7 component types defined in Hugo: 
* **static**
* **content**
* **layouts**
* **data**
* **assets**
* **i18n**
* and **archetypes**.

You can combine modules in any combination you like, and even mount directories from non-Hugo projects, forming a big, virtual union file system.

You need to install `go` in order to use Hugo Modules.

## What can be a Hugo Module

A Hugo module is a directory which may containe additional subdirectories like layouts, static, data etc. These subdirectories are referred to as *mounts* in Hugo modules.

## Why Use Hugo Modules?

Very basic use case of Hugo Modules is adding themes to the site. We don’t have to use git submodules or copy the theme files. This also enables versioning of the theme and pinning it to specific commit.

Similarly the files we copy over for shortcodes, static files etc. We just have to add the module and that’s it, Hugo takes care of downloading the files and using them. This simplifies the automated deployment workflows.

With the ability to use only a specific directory or a file from a module, Hugo modules opens up the door to a lot of possibilities and combinations.

### Separation of reusable components

This benefit is same as that of using _git submodules_ for managing themes. You might have separate repositories for [adding ATOM feed](https://github.com/kaushalmodi/hugo-atom-feed), another for adding [search](https://github.com/kaushalmodi/hugo-search-fuse-js) which you would want to reuse on multiple sites, and you integrate those in your main site repo as submodules. Hugo modules allows you to do the same, and then more (recursive module updates, printing dependency graphs, etc.).

### Mounts

Once a site repo is a Hugo module, it can start using the [**mounts** feature](https://gohugo.io/hugo-modules/configuration/#module-config-mounts).

Let’s call the main Hugo site repository the *self module*. _Mounts_ are analogous to creating symbolic links very similar to [unionfs](unionfs), it’s kind of like that. From any file or directory in the *self module* to **any** file or directory in one or more of the imported _modules_ (including the [self module](https://scripter.co/hugo-modules-getting-started/#org-radio--self-module)).

As an example, you might have a `README.md` in the root of your git repository. Using _mounts_, you can tell Hugo to use that same file as if it were `content/_index.md`  as follows

```toml
[module.mounts](module.mounts)
source = "README.md"
target = "content/_index.md"
```

Now the same `README.md` serves as the content page for your site’s home page! 🤯

## How to use Hugo Modules?

### Install a recent version of `go`

The _Hugo Modules_ feature is powered by [Go Modules](https://github.com/golang/go/wiki/Modules). So even if you don’t develop anything in `go`, you need to have a recent version of `go` installed.

```bash
brew install go
```

### Convert your Hugo site to a _Hugo Module_

The first and one time step is to initialize the site as a module. This can be done with `hugo mod init <module path>`.

```shell
# cd /path/to/your/site/repo/
hugo mod init github.com/USER/PROJECT
```


Here, `github.com/USER/PROJECT` is your Go/Hugo module name. This name does not need to be your git remote repository URL, It can be any globally unique string you can think of.

> Note: The “https://” prefix should not be included in the _[module name](https://scripter.co/hugo-modules-getting-started/#org-radio--module-name)_.

If all went well, a `go.mod` file will be created that will look like this:

```go
module github.com/USER/PROJECT

go 1.19
```

Right now, the `go.mod` only contains your site’s module name and the `go` version used to create it. Once other modules are added as dependencies, they will be added in this same file.

>  `go.mod` file is similar to the `.gitmodules` file when we use _git submodules_.

Once other module dependencies are added, a `go.sum` file would also be created containing the checksums of all your module dependencies.

> [!Note] Git Submodules
> 
> If you are managing your Hugo themes via _git submodules_, don’t worry! Converting your site to a Hugo Module will not break anything.

### Hugo Module dependencies

To add a module as a dependency of a site, we have to run the command `hugo mod get <module path>`. Then add an entry for the module in site configuration (in a config.yaml or config.toml file).

The `<module path>` is required to be a valid Go module path like `github.com/Lednerb/bilberry-hugo-theme/v3`. For local modules we don’t have to run the `get` command. We just need to keep them in the `themesDir` (default is `themes`) and add an entry for them.

The Go module path of the dependencies should resolve to a valid repository. It can be hosted anywhere like GitLab, GitHub etc. It may or may not have a go.mod file in it. _Local Hugo modules don’t need to have a go.mod file if they don’t have any dependencies._

If your site’s source code is not pushed to a public repository, the Go module path for the site can be a single word as well. _Note that if the module name does not resolve to a repository, it won’t be possible for others to use the site as a dependency._

## Sample Setup

The following commands, will get a new Hugo site configured, and setup for modules

```bash
cd ~/Sites
hugo new site damianflynn
cd damianflynn
rm -rf architypes/default.md
```

With the base empty site created, we can publish this to GitHub
```bash
git init
git add -A :/
git commit -m "feat: new hugo site"
git branch -M main
git remote add origin https://github.com/DamianFlynn/hugo.git
git push -u origin main
```

Now, time to convert the site into a Hugo Module

```bash
hugo mod init github.com/DamianFlynn/hugo
```

With the site now functioning, we can proceed with a look at [posts/sw-ssg-hugo-modules-theme](posts/sw-ssg-hugo-modules-theme).