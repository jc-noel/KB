Hugo is a general-purpose website framework for building websites. Technically speaking, Hugo is [[Static Site Generator | static site generator]]. Instead of dynamically building webpages when a visitor requests a page, Hugo builds pages when you create or update the content.

Hugo accomplishes this by taking a source directory of files and templates, then uses those as inputs to create a complete website. These sites run without the need for a database  or dependencies on runtimes like [[Ruby]], [[Python]], or [[PHP]].

Hugo is also cross platform, as its built with [[Go]], anywhere the Go compiler tool chain can run, so can hugo.

## Hugo CLI

Hugo's [[Command-Line Interface |cli]] provides a fully featured working experience from the command line.

Displaying available commands:
* `hugo help` - list of available commands and flags.
* `hugo server --help` - gets help with a subcommand.

Building a site:
* `hugo new site example` - creates a new site named example with default folder structure.
* `hugo` - this will build the site and publish the files to the `public` directory.
* `hugo server` - view your site while developing layouts or creating content.

A full list of commands and their descriptions/use-cases can be found at [Hugo Commands](https://gohugo.io/commands/).

## Directory Structure

After creating a site with the [[Hugo CLI|hugo cli]], a directory structure is created with the following:

```
example/ 
├── archetypes/ 
│   └── default.md 
├── assets/ 
├── content/ 
├── data/ 
├── layouts/ 
├── public/ 
├── static/ 
├── themes/ 
└── config.toml  
```

### Directory Explained

A high-level overview of the directories is given below:

* [[Hugo Archetypes|archetypes]] - When creating new content files using the [[Hugo CLI|hugo cli]], by default, Hugo will use archetypes to build them from. We can create our own with custom preconfigured [[Hugo Front Matter|front matter]] fields.
* [[Hugo Assets|assets]] - This folder stores all the files that need to be processed by [[Hugo Pipes]]. When using these files, only the ones whose `.Permalink` or `RelPermalink` are used will actually be published to the `public` directory.
* [[Hugo Configuration|config]] - Hugo has a large number of [[Hugo Configuration Directives|configuration directives]]. This directory is where those directives are stored, either as [[JSON]], [[YAML]], [[TOML]]. Projects with minimal settings and/or no need for environment awareness can use a single `config.{toml,json,yaml}` file at the root directory. This directory is not created by default.
* [[Hugo Content Organization|content]] - All content for your website lives in this directory. Each top-level folder in Hugo is considered a [[Hugo Content Section|content section]]. For example, if your site has three main sections -- `blog`, `articles`, and `tutorials` -- you will have three directories at `content/blog`, `content/articles`, and `content/tutorials`. These sections are assigned default [[Hugo Content Types|content types]].
* [[Hugo Data Templates|data]] - This directory is used to store configuration files that can be used when generating your site. You write them in [[JSON]], [[YAML]], and [[TOML]] format. You can also create [[Hugo Data Templates|data templates]] that pull from dynamic content.
* [[Hugo Templating|layouts]] - Stores templates in the form of `.html` files that specify how content will be rendered into a static site. Templates include [[Hugo List Templates|list pages]], your [[Hugo Homepage Template|homepage]], [[Hugo Taxonomy Templates|taxonomy templates]], [[Hugo Partial Templates|partials]], [[Hugo Single Page Templates|single page templates]], and more.
* [[Hugo Static Files|static]] - Stores all the static content: images, [[CSS]], [[JavaScript]], etc. When Hugo builds, all assets inside the static directory are copied over as-is. Good for situations where you want Hugo to copy over something without modifying its content. **>= v0.31 you can have multiple static directories.**
* [[Hugo Configuration Directives|resources]] - Caches some files to speed up generation. Can also be used to distribute files for templates, this is not created by default.