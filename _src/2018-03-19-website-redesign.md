<!-- Website Redesign -->
<!-- 2018-03-19 -->

Last week,
[I wrote about](/2018-03-12-writing-my-own-static-site-generator-with-a-makefile.html)
how I implemented my own static site generator, using a makefile.  At the same
time, I redesigned the site, as previously it used the
[Hyde theme](http://hyde.getpoole.com/) for sites built using
[Jekyll](https://jekyllrb.com/).  In redesigning it, I aimed to make the site
as simple as possible without making it unpleasant to read or navigate.

## Design

A big part of the design ethos came from the Motherf\*\*\*ing websites (which,
fair warning, do contain quite a bit of profanity):
[original](http://motherfuckingwebsite.com/),
[better](http://bettermotherfuckingwebsite.com/), and
[best](https://bestmotherfucking.website/).  Essentially, I decided I didn't
want to have to load anything from external sources, set up any kind of fancy
layout, or run any JavaScript.  Among other things, this means that the site
will always use fonts you have installed on your device.  I'm using a pretty
extensive font stack to minimize the chance of getting stuck with, for example,
Arial for text and Courier New for code, but I can't guarantee that you won't
see one of those if you're using an old or uncommon platform.  For this
website, my first choices of text and code font are IBM Plex Sans and IBM Plex
Mono, respectively, and if it's important to you, you can download them for
free from [here](https://github.com/ibm/plex).

I also took inspiration from
[Contrast Rebellion](http://contrastrebellion.com/) and
[SB Nation](https://www.sbnation.com).

## Structure

It took a few iterations before I settled on the current structure of the
website.  Initially, I was planning on having separate pages for my work
experience, projects, and blog, just as I had on the old version of the site.
That changed when I came across
[Vatsal Ambastha's blog](http://www.vatsalambastha.com/) while browsing
[Brutalist Websites](http://brutalistwebsites.com/) for inspiration.  Reading
it, I realized that there was no reason not to put all of the information about
myself, along with links to blog posts, on the landing page.  After all, I
don't have *that* much content I want to include.  This structure is simpler,
easier to navigate, and less cluttered with links.

## Things Left to Do

There are a couple more ideas to improve the site that I'm toying with, too:

* *Tagging posts by subject.*  If this were implemented, it could be easier to
  find the posts you're interested in, but I don't think that's too difficult
  to do now, given the current number of posts.  Tags would also add to
  clutter.  I will keep this in mind as a possibility in the future.
* *Add permalinks to headings within pages.*  This would make it easier to
  share specific parts of posts, but I don't see a need for that coming up too
  often.  I also don't have an elegant way of implementing it given the current
  system for generating the site, so I'm going to hold off on this for now as
  well.  If you really need a link to a certain heading, convert the heading
  title to lower-case, remove punctuation, and replace spaces with hyphens to
  get the section ID.  Then you can append the ID after a `#` to the URL to get
  a link to the heading.

If you find any issues with the website, or think there's something that could
be improved, send me an email at
[jdwinters96@gmail.com](mailto:jdwinters96@gmail.com) or
[open an issue](https://github.com/jdw1996/jdw1996.github.io/issues/new).
