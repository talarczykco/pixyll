---
id: 81
categories:
- Tech
title: migrating from tarballed wordpress to subversioned wordpress
layout: post
wpurl: https://www.mijit.com/?p=81
slug: 2007-05-07-migrating-from-tarballed-wordpress-to-subversioned-wordpress
---
i just migrated from the tarballed wordpress source code to a locally checked-out copy of the subversion repository. this should allow me to upgrade in the future by using <code>svn update</code> or some equivalent.

here's how i did it. see <a href="https://codex.wordpress.org/Upgrading_WordPress">the upgrade documentation</a> for details.

<code>    mkdir mijit.com-new
    ( cd mijit.com-new && svn co https://svn.automattic.com/wordpress/tags/2.1.3 . )
    cp -pr mijit.com/{.htaccess,wp-config.php,wp-content} mijit.com-new
    mv mijit.com mijit.com-old && mv mijit.com-new/ mijit.com
</code>

<strong>note:</strong> on my production server, i ran into a problem where the upgrade page didn't work - it simply showed a white page, no matter how many times i reloaded it. i peppered the php code with <code>die('x')</code> to see what was failing, but it seemed to just crap out at some point in the script execution. then, i discovered the following in my apache error_log:

<code>    Allowed memory size of 8388608 bytes exhausted (tried to allocate 0 bytes)</code>

aha - so sucktastic php is crapping out and barely giving any indication that it is to blame! (i don't blame the wordpress people for not slavishly checking every memory allocation, but you'd think php itself could put something cunning like "php" in its log lines so i know the error is neither with wordpress nor apache.)

googling that line brings the solution, to increase the memory available to php. not that that stopped me from wondering why the hell an upgrade script (which presumably just updates a version string, based on a cursory examination of the code,) needs a ton of memory anyway:

in /etc/php.ini, bump from 8M to 16M:

<code>    memory_limit = 16M</code>

installation proceeded flawlessly.
