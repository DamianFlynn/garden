---
title: Theming Hugo with Module
type: article 
date: 2022-11-21
series: Modularising Hugo
categories: ['todo'] 
tags: ['untagged'] 
draft: True
toc: false 
comments: false 
---


To utilize modules in Hugo for our theme, we first must ensure that our site supports [Hugo Modules](sw-ssg-hugo-modules).

> You can quickly verify that you site is configured for Hugo Modules, as it should contain a  `go.mod` file in the repository root.

## Import the “theme” module

The concept of a Hugo *theme* has been superseded with the concept of *modules*, with the main difference that the former will allow you to build your site entirely, while the latter might implement only some modular features like enabling the ATOM feed, or adding a search to your website.

Importing a module as a theme will typically follow the convention:

Update the sites `config.toml` file with the below content, so that it imports the [`hugo-bilberry-theme`](https://github.com/DamianFlynn/hugo-bilberry-theme)  
    
```toml
[module]
	[module.imports](module.imports)
		path = "github.com/Lednerb/bilberry-hugo-theme/v3"
```

In this case, the path `github.com/Lednerb/bilberry-hugo-theme/v3` references the Github repo hosting the theme to be used.

> Note
> 
> It’s possible to take any Hugo theme git repo and import that as a Hugo Module even if that repo isn’t actually one i.e. doesn’t have a `go.mod`. But it’s recommended that the theme be a proper Hugo Module so that you have better dependency tracking between your site and the theme.


### Testing

Create `content/testing.md`. This step is optional and is only so that your test site as some content.

```md
---
title = "Hello"
type = Article
---
# Hey!

We have a sample article posted
``` 

That’s it! Now run the Hugo server (`hugo server`) and look at your site running on localhost .. while thinking in disbelief.. _just how easy all of this was!_ 😃.

-   Did you need to manually clone any theme? **No**
-   Would you need to deal with the `.gitmodules` file? **No**

## Tidy up the modules

Finally, run [`hugo mod tidy`](https://gohugo.io/commands/hugo_mod_tidy/) to clean up the `go.mod` and update/generate the `go.sum` file. These files will track the module dependencies for your site.

-   The `go.mod` contains the direct module dependencies for your site.
-   The `go.sum` contains the versions and hashes of all the direct **and indirect** dependencies  Just as you added a theme as a Hugo Module to your site, it’s possible that that theme is depending on other Hugo Modules (like the ones I mentioned earlier: ATOM feeds, search, etc.). for your site.

You should ensure that both the `go.mod` and `go.sum` are not excluded from your respoitory, for example due to a `.gitignore` inclusion.

Currently you `go.mod` will be similar to the following:

```text
module github.com/DamianFlynn/hugo

go 1.19

require github.com/DamianFlynn/hugo-bilberry-theme v0.0.0-20221121232919-c175ea5fa1a8 // indirect
```

while, you now also should have a new `go.sum` which again should be similar to this:

```text
github.com/DamianFlynn/hugo-bilberry-theme v0.0.0-20221121232919-c175ea5fa1a8 h1:eVzllMvd5YWOVudIKJn0xGg7za6+N5GfkBwa8p6Oz/E=
github.com/DamianFlynn/hugo-bilberry-theme v0.0.0-20221121232919-c175ea5fa1a8/go.mod h1:M3oqF+dR6itMNHe0OdkZQiQhVgkXZgVO9cZurhyxY4s=
```

## Updating the theme 

Issing the command `hugo mod get -u ./..` will instruct our Hugo module to update all the modules it depends on recursivly.

If you wish to target a specif module only, for example our theme, then we would issue the following command

```shell
hugo mod get -u github.com/DamianFlynn/hugo
```

And, if you wish to reference a specific tag or commit in [git](git) with the following format

```bash
hugo mod get <module path>@<git ref>
```

> [!info] Dependency Graph
>
> If you have a theme added as a Hugo Module, which depends on other Hugo Modules, it’s often helpful to know the dependency graph. You can do that by running:
> ```shell
>  hugo mod graph
> ```
> 
> Which in our current configuration, appears as follows
> ```text
> github.com/DamianFlynn/hugo github.com/DamianFlynn/hugo-bilberry-theme@v0.0.0-20221121232919-c175ea5fa1a8
> ```