<!-- Writing My Own Static Site Generator with a Makefile -->
<!-- 2018-03-12 -->

Over the past couple of months I've been writing my own static site generator
(SSG).  Previously, I had built this site using Jekyll, but I realized that I
wasn't using a majority of the features it offered, and I wanted to have more
control over how my SSG worked.  I also thought it would be fun to implement my
own.

At the beginning of January I started to implement the program in Python,
because that's the language I have the most experience in, and I find it
well-suited to string manipulation.  Because of schoolwork, I worked on it only
sporadically for the next couple of months.  In that time, I realized that file
manipulation was more important than string manipulation, but decided the `os`
module from Python's standard library worked well enough for what I needed.

This went on until I came across
[a discussion](https://news.ycombinator.com/item?id=16483889) about makefiles a
couple of weeks ago on [Hacker News](https://news.ycombinator.com/).  The
comments were a fairly even mix of criticism and praise, but they were enough
to convince me to consider switching my SSG to be a makefile instead of a
Python script.  I had worked with `make` briefly for building C++ programs for
my
[object-oriented programming course](http://www.ucalendar.uwaterloo.ca/1718/COURSE/course-CS.html#CS246)
but at the time it seemed unintuitive and I mainly just used a file the prof
provided.  Going back and re-learning the tool, though, I realized how useful
it could be in this situation, as it handled most of the hardest parts of
writing the SSG automatically, without adding much overhead.  To paraphrase
[Greenspun's Tenth Rule](https://en.wikipedia.org/wiki/Greenspun%27s_tenth_rule),
my Python script contained an ad-hoc, informally-specified, bug-ridden, slow
implementation of half of `make`.  By making the switch, I went from 300 lines
of Python and counting, to a little more than 40 lines of code in a makefile.
It also went from something I worked on off and on for several months to
something I finished in the course of a few nights.  I made a few adjustments
to my plans to make the SSG easier to write for `make`, but all core
functionality remains.

As was planned from the beginning, my SSG, nicknamed "Muad'Dib", is designed
with the layout of this website in mind, and so likely won't be suitable for
any other site.  That being said, anyone is welcome to use Muad'Dib and edit it
to meet their needs.  If you do this, I'd love to see the result.

The code for the Muad'Dib SSG can be found
[here](https://github.com/jdw1996/muaddib-ssg/).
