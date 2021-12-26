---
id: 92
categories:
- Tech
title: the occasional vim error
layout: post
wpurl: https://www.mijit.com/?p=92
slug: 2007-08-14-the-occasional-vim-error
---
this is something that had been bothering me for ages. i am one of those who keeps virtually all my info in one notes.txt file - no <a href="https://www.davidco.com/">fancy GTD system</a>, just a bunch of text that i noodle with on an ad hoc basis. i use <a href="https://www.vim.org/">vim</a> to manage it, and it stays open 24x7 on multiple machines (via <a href="https://www.gnu.org/software/screen/">screen</a> - something i'll go into another time.)

but, the problem with keeping the window open is that, at least on my fedora boxes, a system cron script comes along once a week and cleans out the /tmp directory – including the temp files that vim is using to keep state in my editing session. this makes me have to save/close/reopen vim, usually when i try to launch some external command like <a href="https://www.gnu.org/software/textutils/textutils.html">sort</a>.

the solution i found was to touch vim's temp file daily, making it look like a "new" file to the system cleanup script. vim makes temp file directories in a format like this:

<code><pre>
/tmp/v\d??????
</pre></code>

so, in my personal crontabs, i use this to ferret these out and touch them up-to-date:

<code><pre>
find /tmp -follow -type d -name v?????? -exec touch {} &#92;; 2>/dev/null
</pre></code>

works like a charm.
