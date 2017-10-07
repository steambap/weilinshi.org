+++
title = "No Javascript"
date = 2017-10-07T18:18:05+08:00
draft = false
+++

## Remove JS from my blog
Almost every modern website requires JavaScript to function properly nowadays. At my day job, single page application is the way to build website. If you disable JS, good luck finding a website that is useful. And there is the debate over whether websites should suppport JavaScript-free mode. I'm not going to argue for that issue. I think that depend on your website's content and your budget. However, make my blog JavaScript-free is simple.

## The actual process
My blog have two script files: a syntax highlighter and Google analytics. Thanks to [chroma](https://github.com/alecthomas/chroma), a Golang native syntax highlighter, I can get rid of JS syntax highlighter. If you have used Hexo before, you will know that server side syntax highlight with highlight.js is slow when building large site. On contrary, Chroma is fast. Although Chroma is only four month old at the time of writing, it is production ready for static site. Chroma is bundled with Hugo v0.28.0 and I can remove my JS syntax highlighter.

Removing GA is also easy. I do not need GA. I do not need to anything about my blog reader. Period.

Now, enjoy my JavaScript-free blog!
