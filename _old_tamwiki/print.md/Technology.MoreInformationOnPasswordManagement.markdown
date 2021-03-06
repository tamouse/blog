<div id="wikitext">

<span style="font-size:83%"> (copied from
<http://stackoverflow.com/questions/270485/password-management-best-practices-soup-to-nuts-not-just-storage-or-generation>
) </span>

<div class="vspace">

</div>

Password Management Best Practices (soup to nuts, not just storage or generation)
---------------------------------------------------------------------------------

We have a site with personal user information. I need to know
best-practices for password management.

<div class="vspace">

</div>

1.  These are average users - should I impose 'hard' passwords?
2.  Is there any disadvantage to using the user's email address as a
    userid?
3.  How should I handle forgotten password requests? Obviously, I can't
    email them to the user.
4.  How should the passwords be stored, encrypted? Should I encrypt the
    userids as well?

edited Nov 6 '08 at 22:38

asked Nov 6 '08 at 22:00 Jason

<div class="vspace">

</div>

------------------------------------------------------------------------

\`+1 for considering your users, and for balancing security with
usability! - Adam Liss Nov 6 '08 at 22:26

<div class="vspace">

</div>

-   Hard passwords, in general, are terrible. They force the average
    user to write them down, or worse, type them into a text file in
    order to remember them. Good to enforce some minimal characteristics
    (e.g. at least one number or capital) and do some basic checks
    (minimum length, must differ from user ID, at least a few different
    characters), but you want something your users can remember.
    <div class="vspace">

    </div>

-   Hash the passwords with a secure algorithm (see SHA-1), as there's
    no reason to retrieve them. When a user logs in, hash the input and
    compare it to the stored hash.
    <div class="vspace">

    </div>

-   Email address as user ID is reasonable, as your users will probably
    remember it (especially if your login prompt says "Enter email
    address"). No need for users to update it if their address changes
    unless you use it for something else (such as password reset).
    Better to have a separate field for password recovery information.
    <div class="vspace">

    </div>

-   Encrypt the user IDs, since you will need to retrieve them. Storing
    them unencrypted tells an attacker which accounts are valid,
    eliminating half the break-in battle.
    <div class="vspace">

    </div>

-   **Never** send a forgotten password to anyone. (This is easier to
    abide by if you hash the passwords, as you will no longer have the
    plaintext.) A reasonable practice is to email a one-time link to the
    user's pre-registered email address, directing the user to a "reset
    password" page. Depending on the required level of security, you
    might want to ask for some sort of identifying information, then ask
    them to enter a new password.

As always, tailor your implementation to fit the required security
level: what's at stake if the data is compromised, and to what lengths
should you and your users go to protect it? When in doubt, follow the
model of an online banking system. In general, where money is involved,
a lot of research has been invested in best practices for security.

edited Nov 7 '08 at 0:33

answered Nov 6 '08 at 22:08 Adam Liss

<div class="vspace">

</div>

------------------------------------------------------------------------

"When in doubt, follow the model of an online banking system." -- Yes
and no, cf thedailywtf.com. "In general, where dollars are involved, a
lot of research has been invested in best practices for security." --
Yes and no, cf thedailywtf.com. Better practices are at euro banks than
dollar banks. ?? Windows programmer Nov 7 '08 at 0:12

One round of SHA-1 is too fast. And what about salt? - erickson Nov 7
'08 at 0:21

Oops - shame on me for being the typical US-centric American. Thanks for
the opportunity to improve worldliness and accuracy! - Adam Liss Nov 8
'08 at 1:05

@erickson, Salting the passwords out will help if an attacker manages to
get the table of password hashes and usernames, because then a
rainbowtables-based approach will be thwarted. - cdeszaq Jan 21 '09 at
19:17

<div class="vspace">

</div>

------------------------------------------------------------------------

<div class="vspace">

</div>

Hard passwords
--------------

I quite dislike when sites try to impose what kind of password I should
use, but it all depends on the site... however, sites should always
enforce an absolute minimum password quality (at least 6-8 chars,
different from username)

<div class="vspace">

</div>

Storing passwords:
------------------

As said in first reply, store passwords using a one-way hash function.
to compare them just hash the password again.

For better security when storing the hashed passwords, prepend a "salt"
string to them: instead of storing sha1("password"), store
sha1("somesalt":"password"). This will makes password cracking
exponentially harder if by some chance the hash is obtained by a
malicious user, and in a way eases the need for strong passwords.

<div class="vspace">

</div>

Forgotten password requests
---------------------------

To handle password requests, create a new password token and send a
"regenerate password" link to the requesting user's email. when such
link is accessed, create (or allow user to choose) a new password, and
invalidate (delete) the token. Also, tokens should not be guessable or
brute-forced (use at least 8 random hex/base64 chars)

<div class="vspace">

</div>

Other
-----

Obviously, every security aspect will fall back to the weakest link so
specific implementation details should take security in account, like
using https for logins, etc...

'store sha1("somesalt":"password")' --\> store
"somesalt"+sha1("somesalt":"password") so that you'll know what salt to
use later when the user inputs a password string. This salt is something
you should randomize yourself in order to defeat rainbow cracking; don't
show it to the user. - Windows programmer Nov 7 '08 at 0:09

It doesn't matter if the attacker knows the salt. Just make sure that
different salt is used for each password. An easy yet safe approach is
to use 8 to 16 bytes from a cryptographic RNG. - erickson Nov 7 '08 at
0:25

Be permissive in what characters you allow in passwords. I've seen
enough password fields that wanted to restrict me to a fairly short
number of letters, and exclude special characters. Sort of like they're
trying to mandate weak passwords, even when I want to use a strong one.

answered Jan 21 '09 at 19:05 David Thornley

<div class="vspace">

</div>

------------------------------------------------------------------------

1\) These are average users - should I impose 'hard' passwords?

Are you making a forum? No. Protecting bank details? Storing their
children's addresses? Maaaybe. Depends on what you're letting the user
do, and what he gains access to.

2\) Is there any disadvantage to using the user's email address as a
userid?

What about when I change my e-mail address?

3\) How should I handle forgotten password requests? Obviously, I can't
email them to the user.

It's academic, but I'll point to a [whitepaper by a
friend](http://www.cs.stevens.edu/~lyang/) who studied this question and
wrote a paper called Quantifying the Security of preference-based
Authentication. Basically he uses preferences instead of silly questions
like "Mother's maiden name".

4\) How should the passwords be stored, encrypted? Should I encrypt the
userids as well?

I'll give my canned link to [bcrypt
hashing](http://www.securityfocus.com/blogs/262)

answered Nov 6 '08 at 22:05 Tom Ritter

<div class="vspace">

</div>

------------------------------------------------------------------------

'Hard' passwords are a tricky problem. I've found that a lot of password
checking algorithms have rules that are too inflexible -- and that's
what people object to. What I found successful is to score a new
password with the rules and have a minimum accepted score. This lets the
end-users do things like have a (much) longer password instead perhaps
of having to include both numbers and mixed case. link|flag

answered Nov 6 '08 at 22:59 staticsan

<div class="vspace">

</div>

------------------------------------------------------------------------

Is there any disadvantage to using the user's email address as a userid?

Only if You expect the userid will be a valid email address. It does
provide the opportunity for a fairly strong password ( @ and .,
length \> 10, mixed case). If you wish to capture an email address
capture it in a separate field labeled Email Id.

jcinacio has it right about salting the hash to store the passwords -
makes it very hard for the internal snoop(someone with read access to
your databases) to run a dictionary of hash codes against your system.

edited Nov 6 '08 at 23:48

answered Nov 6 '08 at 23:43 kloucks

<div class="vspace">

</div>

</div>
