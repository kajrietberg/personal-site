---
title: New site, who dis?
excerpt: As I do every year, I went through my personal site to do some refreshing and improvements. Here are the main takeaways and lessons learned, in case you are curious.
---

So, let's start with the design changes.

## Design

In terms of design no much has changed, I stuck with a minimalistic style. As a small site, scaling isn't a threat and it helps to group tokens and decisions with the mind set on performance and minimal style foot print.

I moved the site to a [new repository](//github.com/jeremenichelli/personal-site), which means you can still [check out the old one](//github.com/jeremenichelli/jeremenichelli.github.io) and compare it if you want to.

## Styles

As said before, though styles haven't changed a lot I coded all from scratch and there are good reasons for this.

I had the overall felling I was over-adjusting browsers default styles using bare tags as selectors and using some elements not for their semantic meaning but because of the _styling shortcut_ they provided.

### Tags for content, class names for styles

The methodology I went for is to _only use class names_ and override browser defaults when really necessary. This made me apply the right and preferred tags for content improving accessibility. I'm also using CSS variables now for distributing values instead of LESS variables.

I'm still using [LESS](//lesscss.org/) but as a bundling tool. I generate one specific entry file for each page, and each page has a _type_ variable in the front matter to know which styles to pick.

I pass the result through [cssnano](//cssnano.com) to optimize it, export it as partial and later inline the styles in the `head` of the page.

## Scripts

There's no much JavaScript around the site, and most of it takes care of the web font loading strategy.

The static output assumes the user has disabled JavaScript and unless an inline script runs at the beginning then no animations run on the homepage and no web font nor other dependencies are loaded, giving the user a complete opt-out of network resources needed, only content.

If the user has JavaScript enabled then, a stylesheet containing font face rules and a script which will observe the fonts are both asynchronously loaded. If this procedure is successful then I use [store-css](//github.com/jeremenichelli/store-css) to save in local storage the font face rules in addition to a `fonts-cached` flag, browser cached does the rest for future inner navigations.

I wrote about this approach in [this article](/2016/05/font-loading-strategy-static-generated-sites/) a while ago if you are interested.

## Animations

The home page has some orchestrated animations. The page waits for a maximum of three seconds for web fonts to be loaded, if they don't animations will kick off either way and won't run at all when JavaScript is disabled.

In the previous version of this site I was using transitions and transition delays, this caused them to be _fast-forwarded_ on repeated views and strange side effects on resizes. This is why I'm now using animation keyframes.

## Static generator

Originally this site was using [Jekyll](//jekyllrb.com) and [GitHub Pages](https://pages.github.com/), a really convenient setup to quickly launch something without much overhead and a solution I would still recommend to anyone.

Sadly as the content grew, Jekyll builds became super slow, making my writing experience a bit frustrating, so I decided to give [Eleventy](//11ty.io) a try.

Eleventy is basically a Jekyll implementation in Node.js, but easily extensible and more template language options. The most important thing is that _builds are really fast_ folks.

### New ecosystem, new gotchas

The move was smooth in a lot of corners, and not as smooth as I would have liked in others. Most of the package defaults are the same as in Jekyll, like template language and directories which it's great when migrating.

But now _liquid_ and _markdown_ parsers are JavaScript implementations which have differences with their Ruby counterparts so some snippets weren't _just working_ out the box.

Most of these differences are not bugs though, since there aren't real standards around but is good to keep [these](https://github.com/11ty/eleventy/issues/68#issuecomment-383386627) [gotchas](https://github.com/11ty/eleventy/issues/533) in mind if you are going to move a Jekyll site to Eleventy.

In addition, Jekyll extends _liquid_ with some useful filters that aren't present, the good thing is that Eleventy is super configurable around this so you can create these missing parts in seconds.

Eleventy has really good up-to-date documentation and Paul Lloyd wrote [a great introduction article](//24ways.org/2018/turn-jekyll-up-to-eleventy/) I strongly recommend.

## Hosting

One of the things that performance tools were _penalizing_ my site for was low server response which wasn't much in my control. Now, this site lives in [Netlify](//netlify.com) supporting secure connections, branch previews and continuous deployment in addition to CDN distribution.

> Netlify is the thing I wished was available when I was a kid throwing invalid HTML to an FTP server like twenty years&nbsp;ago.

As much as I recommend the simplest setup to new developers and beginners to focus more on code and learning than in infrastructure and servers, Netlify is just so simple that was the easiest thing in the long migration list I had.

## Wrap-up

As I said on Twitter some time ago, a personal site is this place where you can show _who you are_ and by roaming around mine and reading this piece you will notice that, first, I'm definitely not a designer and second, I'm quite obsessed with performance and content delivery.

I always say that business is the main threat to a performant web, third-party scripts, complex and badly orchestrated feature implementations are coming from it.

This is why I found interesting to hear stories on how teams dealt with optimizations while responding to business priorities, but here _there's no business_, this is me and my playground. My playground, my rules.

### Credits

_Huge thanks to [Zach Leatherman](//twitter.com/zachleat) for Eleventy, [Jun Yang](//github.com/harttle) for liquidjs, and [Alex Kocharin](//github.com/rlidwka) and [Vitaly Puzrin](//github.com/puzrin) for markdown-it, which this site heavily relies on._
