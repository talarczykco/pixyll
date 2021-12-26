---
id: 189
categories:
- Tech
title: "/dev/null: Permission Denied"
layout: post
wpurl: https://www.mijit.com/?p=189
slug: 2007-09-10-slash-dev-slash-null-permission-denied
---
i was getting this error repeatedly on boxes in my home domain. i would set up a system and then, seemingly at random, i would try to ssh to it and it would spout several "/dev/null: Permission Denied" as i tried to fire up an ssh-agent. looking at /dev/null, it showed a file mode of (if i remember correctly,) 600 and owned by root. i had no idea what was even *accessing* /dev/null - i certainly didn't have any cron jobs set up to alter it. googling the problem didn't really enlighten me, but i did check my .bashrc scripts and noted that i had set MYSQL_HISTFILE to /dev/null. a-ha! some reports on the web showed that various apps like to set files' permissions in a sneaky sort of way, so i theorized that perhaps mysql sets the perms on that file to something sensible for a history file (to the detriment of other apps, of course, but mysql isn't really expecting to write to a file other people will need, anyway. and mysql isn't suid root, so this is still just a guess.) removing this line seems to work, for now, so hopefully i will have solved this. and hopefully this will have helped you, too!
