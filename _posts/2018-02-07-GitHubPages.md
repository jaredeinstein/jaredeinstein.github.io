---
title: GitHub Pages
---

In the interest of me liking to learn new things, this seems to be a good place to log them.

I have always liked the idea of hosting a website, but am too cheap to pay because I don't have a legitimate purpose to pay for it.  I have hosted a site with GitHub Pages before, but it was a very basic site with one HTML page with a little bit of fancy CSS and Javascript.

Upon recent investigation, GitHub Pages support a pretty extensive suite of built in site development.  It was at least unique enough and something I wanted to investigate, and learn more about.

This post will be dedicated to some of the resources I used to make this site, and lessons learned along the way.


The first page I stumbled upon that opened my eyes to what GitHub Pages (GHP from now on) was capable of was [Jonathan McGlone's Tutorial](http://jmcglone.com/guides/github-pages/)....[github code](https://github.com/hankquinlan/hankquinlan.github.io).  In hindsight, it even has a lot of the resources listed below.  The nice thing was the background, which I mostly knew, and the step by step walk through.  He does a great job of breaking down the steps with enough detail to venture further down each avenue that GHP has to offer.

I spent a LOT of time on the [Jekyll Doc page](https://jekyllrb.com/docs/home/).  Most specifically, looking at the interactions of [Posts](https://jekyllrb.com/docs/posts/) and how to extend [Collections](https://jekyllrb.com/docs/collections/), along with [Templates](https://jekyllrb.com/docs/templates/).  I like that I could mimic a lof of what Posts has built in, but tailored to how I wanted to use it, rather than mostly as a cronological blog.  That is how the [Recipes]({{ site.baseurl }}{% link _pages/recipes.html %}) came up.  I wanted to have recipes, but wanted to group and sort them by category and type.  I figured it would be extra work, but more to my needs than what Posts offer.

Since I needed a bit more understanding of searching and grouping and sorting once I have all these parts of my collection, that is where [SiteLeaf's Liquid examples](https://www.siteleaf.com/blog/tags/liquid/) came in most handy.  It was a great jumping off point to what and how I was going to look at my data.

Markdown is pretty straightforward with this [Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet).  As well as the [GitHub specific Markdown](https://guides.github.com/features/mastering-markdown/).  And [PDF](http://packetlife.net/media/library/16/Markdown.pdf)  As well as [HTML to Markdwon Converter](https://domchristie.github.io/turndown/)

I also put the [Cayman](https://pages-themes.github.io/cayman/) theme and used the [docs to customize further](https://github.com/pages-themes/cayman) and the base [CSS](https://github.com/pages-themes/cayman/blob/master/_sass/jekyll-theme-cayman.scss)

To check compilation status on [Settings page](https://github.com/jaredeinstein/jaredeinstein.github.io/settings).

Out of curiosity, I also found a [GitHub to Google Apps Script](https://mashe.hawksey.info/2016/08/working-with-github-repository-files-using-google-apps-script-examples-in-getting-writing-and-committing-content/) tutorial.  Took some real poking, but it does work.  Not sure what I want to use it for since cannot push images GitHub.
