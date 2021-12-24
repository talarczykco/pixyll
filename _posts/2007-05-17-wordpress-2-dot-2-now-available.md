---
id: 112
categories:
- Tech
title: WordPress 2.2 now available
layout: post
wpurl: https://www.mijit.com/?p=112
slug: 2007-05-17-wordpress-2-dot-2-now-available
---
<blockquote>On behalf of the entire WordPress team, I’m proud and excited to announce <a href="https://wordpress.org/development/2007/05/wordpress-22/">the immediate availability of version 2.2 “Getz”</a> for download. This version includes a number of new features, most notably Widgets integration, and over two hundred bug fixes. It’s named in honor of tenor saxophonist Stan Getz.
–Matt, from <a href="https://wordpress.com/">WordPress</a></blockquote>

Here's my "short list" of observations:

<ul>
<li>Widgets (ie, AJAX-y sidebar thingies)</li>
<li>Full Atom support (plus new XML-RPC APIs)</li>
<li>Blogger.com integration</li>
<li><a href="https://jquery.com/">jQuery</a> used for internal functions on admin page</li>
<li>Optionally set “home” and “siteurl” options in wp-config.php instead of the database (useful when supporting a dev/prod setup)</li>
<li>Tenor players (eg, Getz) rule</li>
</ul>

if you followed <a href="https://www.mijit.com/2007/05/07/migrating-from-tarballed-wordpress-to-subversioned-wordpress/">my previous post regarding subversion access to WP code</a>, I can attest that the upgrade is now as easy as:

<code>    svn switch https://svn.automattic.com/wordpress/trunk/</code>

...and hitting the database upgrade link. I needed to <code>svn switch</code> since, as you may have noticed, I had checked out the 2.1.3 tag, not the trunk. Switching to trunk should mean that all future upgrades are as easy as <code>svn upgrade</code>. Whee, etc!
