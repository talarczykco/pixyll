---
id: 115
categories:
- Tech
title: A beginning Test::More suite, VIM discoveries, and FuseFS musings
layout: post
wpurl: https://www.mijit.com/?p=115
slug: 2007-08-10-a-beginning-test-more-suite-vim-discoveries-and-fusefs-musings
---
<h2>A Perl Test::More suite for small networks</h2>

I've been wanting to implement some simple tests for my home network to make sure everything is running the way I expect it to on an ad-hoc basis. I've got a Nagios setup monitoring my Apache, MySQL, and Zimbra services, but I wanted a bit more granularity to my tests, a command-line interface, and the ability to separate out the "business logic" a la MVC.

Since I had documented my server installation routine in chronological order (eg, "first, unpack the box,") I immediately noticed I had roughly determined a five-step overview of the process. Since most tests I've seen run numerically, I decided on:

<ul>
<li>00prereqs.t - do I have everything I need to run the other tests?</li>
<li>01dns.t - can I resolve host names, so I can find hosts and services?</li>
<li>02time.t - is my clock correct (so later time dependencies would work?)</li>
<li>03hosts.t - can I find all hosts I expect to find?</li>
<li>04services.t - can I contact all services (SMTP, HTTP, etc) I expect to find?</li>
</ul>

UPDATE: I forgot that I moved the DNS test earlier in the sequence - my NTP test relies on DNS to find external time masters to get the current time.

Note: I discovered via 02time.t that Net::Time doesn't work for me; Net::NTP does. I don't know what the difference is, but "working" is always a plus in my book. Basically, using it, I test whether localtime is the same as NTP time (see Net::NTP time for details.)

I am not sure whether 04services.t will grow into multiple files for various services, but this works well enough for now. I won't go into each individual test in each file for this post, but suffice it to say this is how I now test for sanity from my SVN tree:

<code><pre>
~/src/mijit
[204]meatbag$ prove -l t/
t/00prereqs.....ok                                                           
t/01time........ok                                                           
t/02dns.........ok                                                           
t/03hosts.......ok                                                           
t/04services....ok                                                           
All tests successful.
Files=5, Tests=11,  2 wallclock secs ( 0.97 cusr +  0.26 csys =  1.23 CPU)
</pre></code>

<h2>VIM discoveries</h2>

<ul>
  <li>When using the <code>gq</code> command, the <code>autoindent</code> option can be your friend! This allows indented text to be left-aligned correctly (a necessity for my notes.txt file.)</li>
  <li>some <code>matchparen</code> plugin that is installed on some of my machines and not others adds annoying cyan brace-matching highlighting. This drags my eye away <strong>completely</strong> away from the cursor - bad plugin! The quick fix: :NoMatchParen disables the plugin, and :DoMatchParen enables it again. Do as you like to your <code>plugin/</code> directory.</li>
</ul>

<h2>FuseFS musings:</h2>

Much seems to have been made of FuseFS lately, which I think is a really neat (but not necessarily great,) idea - and I mean that in a it works in theory, but not reality. At least, that's been my limited experience. Given the state of these tools and the tech know-how needed to deploy them, I find myself asking why not just set an expectation that WebDAV can do it for you (yes, I know it has its own problems with various clients requiring certain server headers. Maybe that means we should pressure people <strong>more</strong> to follow open standards.) That said, I have been finding the following quite useful to <a href="https://www.pthree.org/2007/02/10/sshfs/">mount a remote directory to a local directory over SSH:</a>

<code><pre>
sshfs -p $REMOTE_PORT $REMOTE_HOST:$REMOTE_DIR $LOCAL_DIR
</pre></code>
