---
layout: post
title: Up and Running!
categories: meta misc
---

Well there we have it, I've finally got everything set up the way I
like it, so now I can start writing about things (that no one will
ever read). The main intended purpose for this blog is for me to have
a place to talk about the various projects that I'm currently working
on. I'll also be writing about various quirks & problems (hopefully
with solutions!) that I encounter with software in day-to-day
development.

The Blog Itself
---------------

### Jekyll

This blog has been created using [Jekyll](http://jekyllrb.com/), which
the creator describes as "a simple, blog aware, static site
generator". The basic premise is that jekyll converts your posts -
written in Textile, Markdown or other markup languages - into static,
HTML pages; so there's none of the overhead usually encountered in
dynamic Content Management Systems (such as Wordpress).

Development is made easier by allowing pages to declare their layouts
(and other information) using [YAML Front
Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter),
which is a short snippet of configuration data at the top of the
file; this means that I can write all of my pages to use the same
'master' layout, which means that I only have one lot of <head> tags
to worry about. It also makes it extremely easy to make pages
consistent in design without duplicating too much stuff. Jekyll
makes use of [Liquid](http://liquidmarkup.org/) Templating, which
means I can throw conditional statements into my pages or layouts too.

Jekyll itself is written in Ruby, which was my first experience with
the language, and I've very much liked what I've seen so far. Jeykll
is incredibly extensible, allowing use of drop-in plugins that can add
new functionality and extend or even replace existing functionality.

### Compass

I decided to go (almost) all out Ruby with this site, employing the
use of [Compass](http://compass-style.org/) to convert between [SCSS](http://sass-lang.com/) and
CSS; I've always found CSS's syntax a little bit clunky to deal with,
and SCSS alleviated much of this for me.

SCSS allows me to do things like this:
{% highlight scss %}
nav {
    ul {
        margin:0.5em;
        font: {
            family:'JosefinSlabLightItalic', Arial, sans-serif;
            size:125%;
        }
        display:inline;
        float:right;
        li {
            display:inline;
            margin:0 0.5em;
        }
    }
}
{% endhighlight %}

I think it's probably one of those you either love or hate; to me,
this is a much more compact and logical way of writing CSS, probably
because it follows the same nested block structure of the programming
languages I'm familiar with, but also because it saves time. To
others this syntax may well look messy, but hey, each to their own.

### Pygments

As seen above, I'm using [Pygments](http://pygments.org/) to provide
syntax highlighting for code snippets; I figured that this was
important, given that the blog will be primarily focusing on
code. Pygments is written in Python, and provides a syntax
highlighting for a _huge_ range of programming and markup
languages. (It even did SCSS above!)

Jeykll provides support for Pygments out of the box (although it's
disabled by default) and so the integration is perfect, it's as simple
as:

{% highlight c %}
{{ "{% highlight c " }}%}
   int main(int argc, char** argv) {
       printf("Hello world!\n");
       return 0;
   }
{{ "{% endhighlight " }}%}

{% endhighlight %}

### All in all...

The combination of tools I'm using allows me to do what I really
wanted to do _write_ and not waste time trying to get things to work
properly; I can write my posts in Markdown, meaning that I'm not
slowed down or distracted by a WYSIWYG editor, nor am I busy
incessantly tweaking HTML & CSS whilst doing it by hand.

Oh, and I can manage the whole thing with [git](http://git-scm.com/)
too.
