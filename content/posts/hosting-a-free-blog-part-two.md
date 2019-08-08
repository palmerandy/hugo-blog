---

title: "Hosting free blog using Netlify & Hugo"
date: 2018-01-05T18:02:11+11:00
draft: false
tags: ["blog", "hugo", "markdown", "vs code", "github", "netlify"]
---

This is the second article detailing my exploits to build a free blog.  The first attempt, failures and learning can be found in [Part 1](/posts/hosting-a-free-blog-part-one).

In case you din't read the first article, I am trying to create a blog to solve the following problems:

1. Host a blog cheaply as possible - ideally for free.
2. Simplicity is king - particularly when it comes to authoring and publishing.  
3. [Realised after first attempt](/posts/hosting-a-free-blog-part-one): Solution needs to work cross platform - as I use both Windows and OSX.

#### <a name="the-approach"></a>The approach

#### Research
After discovering the shortcomings outlined in [Part 1](/posts/hosting-a-free-blog-part-one) I did more research.  Eventually I stumbled across GitHub Pages and Hugo combined with Netlify.  To be honest I think either would work for me but [Netlify's comparison](https://www.netlify.com/github-pages-vs-netlify/) along with my own researched made me want to try it.  One day I might re-create this blog from GitHub and do a comparison.

#### Hugo
I used Hugo to create the static site and it seemed familiar to Wyam.

> Hugo is one of the most popular open-source static site generators.  With its amazing speed and flexibility, Hugo makes building websites fun again.

I became familiar with Hugo using the [excellent quick start](https://gohugo.io/getting-started/quick-start/).  This worked well but I wanted to change my theme. I chose my theme [Hyde](https://github.com/spf13/hyde) from the [list of blog themes on the](https://themes.gohugo.io/tags/blog/).  To swap themes I tweaked the instructions from the [quick start step three](https://gohugo.io/getting-started/quick-start/) to:
{{< highlight cmd "linenos=table">}}
cd quickstart;\
git init;\
git submodule add https://github.com/spf13/hyde themes/hyde;\

echo 'theme = "hyde"' >> config.toml
{{< /highlight >}}

#### Markdown and Visual Studio Code
Hugo supports [authoring blogs in Markdown](https://daringfireball.net/projects/markdown/).  

> Markdown is a text-to-HTML conversion tool for web writers. Markdown allows you to write using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML).

Visual Studio Code (VS Code) is fantastic, if you haven't heard about it.

> Visual Studio Code is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages (such as C++, C#, Java, Python, PHP, Go) and runtimes (such as .NET and Unity)

For the blog I use VS Code when authoring articles, specifically the [Markdown Preview functionality](https://code.visualstudio.com/docs/languages/markdown#_markdown-preview).  The *The Markdown:Open preview to side* from the *Command Palette* works exactly how you I want it to.

#### GitHub
At this point I have a fully functioning blog that I can run locally that is stored safely in a public [GitHub repository](https://github.com/palmerandy/blog).  

### Netlify 
[Netlify's](https://www.netlify.com/) marketing site is great, and they nailed their mission statement and marketing materials:

> Netlify is a unified platform that automates your code to create high-performant, easily maintainable sites and web apps.

>Write frontend code. Push it. We handle the rest.

> All the features developers need right out of the box: Global CDN, Continuous Deployment, one click HTTPS and more...

I loosely followed the [Hugo's documentation to setup hosting on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/).  This walked me through creating the free account, connecting Netlify to my GIT repository, and configuring Netlify to build with a specific version of Hugo.  I chose the [latest release of Hugo](https://gohugo.io/categories/releases).