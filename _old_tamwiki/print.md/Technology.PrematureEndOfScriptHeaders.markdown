<div id="wikitext">

<div class="vspace">

</div>

suEXEC Problems
---------------

Ganked from <http://encodable.com/suexec_problems/>

<div class="vspace">

</div>

<div class="round lrindent quote">

...and how to fix them.

When running a Perl CGI script, you may see the "premature end of script
headers" error in your Apache error.log or error\_log file.?? This can
be caused by some of the same problems (and fixed with some of the same
fixes) as the Internal Server Error message.?? However, this can also be
caused by suEXEC errors, and in that case, it may give you no indication
whatsoever that suEXEC is the cause.

Check if you have a /var/log/apache/suexec.log file; if so, and if it's
the problem, it'll explain the reason why it's denying your script from
executing properly.?? One easy fix to try is adding a "-w" to the end of
the first line of your Perl script, i.e. change the first line from
"\#!/usr/bin/perl" to "\#!/usr/bin/perl -w" and see if that makes suEXEC
happy.?? Another common fix is to make sure that your CGI script has the
same user/group ownership as your cgi-bin folder.

</div>

Posted: 2012-03-11T06:12:11-0500

<div class="vspace">

</div>

</div>
