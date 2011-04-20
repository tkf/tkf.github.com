---
layout: post
title: Python script to plot histogram of commit frequency of Mercurial or Git repository
---

# {{ page.title }} #

When I choose a open-source software from several projects, I'd want to
choose more active one. Although some repository service like
[Google Code](http://code.google.com/) provides "activity" information,
that was not enough because:

1. I want to know same "activity" measurement across the projects.
2. I can't see activity if the repository service does not provides
   information anyway.
3. It'd be better to know that if the repository has high "activity"
   continuously.

So, I wrote a simple Python script to plot a histogram of commit frequency
(number of commit per day).

The figure bellow is an example. It is the histogram of the Numpy repository.
You can see, for example, that it became very active at 2006.

[![repo-act-numpy by takafumi_a, on Flickr][fig_img_1]][fig_link_1]

[fig_img_1]: http://farm6.static.flickr.com/5303/5610051566_08c5ca8a96.jpg
[fig_link_1]: http://www.flickr.com/photos/arataka/5610051566/

For your information, this Python script can read any (point process)
data if it is a sequence of an unix time per line. Please let me know
if you have some other use of this script!

You can find the script at
[tkf's gist: 913543 — Gist](https://gist.github.com/913543)
or bottom of this post.

## Usage ##

Execute this at working directory of the repository of which you want to
know the commit frequency.

    hg log --template '{date}\\n' | datehist.py
    git log --format='%at' | datehist.py

`datehist.py` can read the data (the output of `hg log` or `git log`)
like this from stdin:

    1298586880
    1298516219
    1298426531
    1298426393
    1298418897
    1298407083
    1298387222
    1298387170
    1295910432
    (... and so on)

This is a sequence of unix time separated by new line `\n`.

You can specify the plot title from the command line option `-t`.
For example, if you want to use the full path of the working directory
as the title:

    hg log --template '{date}\\n' | datehist.py -t `hg root`
    git log --format='%at' | datehist.py -t `git rev-parse --show-toplevel`


## Application: Which distributed bug tracking system is active? ##

I compared the following projects:

1. [b](http://www.digitalgemstones.com/projects/b/)
2. [Bugs Everywhere](http://bugseverywhere.org/be/show/HomePage)
3. [Ditz](http://ditz.rubyforge.org/)
4. [pitz](http://pitz.tplus1.com/)

[![screenshot-2011-04-04-185810 by takafumi_a, on Flickr][fig_img_2]][fig_link_2]

[fig_img_2]: http://farm6.static.flickr.com/5144/5588561616_2ae2ee5c2f.jpg
[fig_link_2]: http://www.flickr.com/photos/arataka/5588561616/

Hmmm... Looks like none of them is *very* active.

I'm using b for some reason.

## datehist.py ##

You can get the newest one from
[tkf's gist: 913543 — Gist](https://gist.github.com/913543).

Note:
It looks like you need a local repository to see its log.

- [4.23. How can I do a "hg log" of a remote repository? - FAQ - Mercurial](http://mercurial.selenic.com/wiki/FAQ#FAQ.2BAC8-CommonProblems.How_can_I_do_a_.22hg_log.22_of_a_remote_repository.3F)
- [git - git log of remote repositories.](http://git.661346.n2.nabble.com/git-log-of-remote-repositories-td4899042.html)

<script src="https://gist.github.com/913543.js?file=datehist.py">
</script>
