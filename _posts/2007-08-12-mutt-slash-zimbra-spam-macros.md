---
id: 116
categories:
- Tech
title: mutt/zimbra spam macros
layout: post
wpurl: https://www.mijit.com/?p=116
slug: 2007-08-12-mutt-slash-zimbra-spam-macros
---
here are snippets from my <code>.muttrc</code> file which allow me to bounce messages to my <a href="https://www.zimbra.com/">zimbra</a> server spam / ham mailboxes, for the purposes of training spamassassin. this was taken from <a href="https://www.lucas-nussbaum.net/writings.php?emails">Lucas Nussbaum</a>, but his link appears to be down at the moment. i'm including these here in case anyone elses googles "<a href="https://www.google.com/search?hl=en&q=mutt+zimbra+spam+macro">mutt zimbra spam macro</a>", like i did for a few weeks to no avail.

<code>
macro index S "&lt;bounce-message&gt;spambox@mijit.com\nyd" "Learn as spam"
macro pager S "&lt;bounce-message&gt;spambox@mijit.com\nyd" "Learn as spam"
macro index H "&lt;bounce-message&gt;hambox@mijit.com\nyj" "Learn as ham"
macro pager H "&lt;bounce-message&gt;hambox@mijit.com\nyj" "Learn as ham"
</code>

these allow you to highlight a message in the index or pager, hit "S" for spam (or "H" for ham,) and the message bounces away to the appropriate zimbra mailbox for automated spamassassin learning. (additionally, the "S" for spam macro marks the message for deletion; the "H" for ham macro simply moves to the next message.) the tricky bit for me was understanding that mutt allows the "\n" newline character to simulate &lt;return&gt; at the end of the &lt;bounce-message&gt; command.

note that you must change "spambox" and "hambox" to the appropriately named mailboxes for your domain. to see what your current spam mailboxes are, issue the following command as the zimbra user:

<code>
zmprov gacf | grep SpamAccount
</code>

to change these values to something more appropriate to your domain, use zmprov again:

<code>
zmprov mcf zimbraSpamIsNotSpamAccount &lt;your ham account&gt;@example.com
zmprov mcf zimbraSpamIsSpamAccount &lt;your spam account&gt;@example.com
</code>

