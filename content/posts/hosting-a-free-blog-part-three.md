---

title: "Hosting a blog for free: Part III"
date: 2018-02-02T18:02:11+11:00
draft: true
tags: ["blog", "wyam", "vsts", "azure", "app service web app"]
---

This is the third article detailing my exploits to build a free blog.  The first attempt, failures and learning can be found in [Part 1](/posts/hosting-a-free-blog-part-one),.

## Background context
In case you din't read the first article, I am trying to solve the following problems:

1. Host a blog cheaply as possible - ideally for free.
2. Simplicity is king - particularly when it comes to authoring and publishing.  
3. **Realised during first attempt**: Needs to work cross platform - as I use both Windows and OSX.

### Custom domain and CloudFlare
- buy domain (from namecheap.com)
- find out https://jameshfisher.com/2017/08/08/hosting-on-netlify.html
- use Cloudflare for DNS and SSL
- following instructions in cloudflare: delete a record and cname.  scan DNS. Replace cname with andypalmer.netlify.com 
- change NameCheap's name servers to cloudflare (as per https://www.goheroe.org/2017/08/21/host-your-blog-for-free-with-hugo-github-netlify-and-cloudflare/)
- wait
- in netlify enter the custom domain
- turn "Always use HTTPS" on in cloudflare


https://testmysite.io/?q=andypalmer.info
vs
https://testmysite.io/?q=andypalmer.netlify.com

### taxonimies
- add tags

### menus


### theme as sub project


