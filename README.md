# EmailValidator
A lightweight and customizable helper class to validate any e-mail address using the HTML living standards RegEx and/or ASP.NET Core built-in validators in C#

## Introduction
Validating an email address is a classic requirement of almost any web application. Most of the modern client-side and server-side web frameworks provide native methods to fulfill this need: however, as many web developers know all too well, the methodologies used for validating email addresses do not always return the same result - and what is considered "valid" for them does not always correspond to the desired result for our specific case.

To better understand such concept, consider the following email addresses:

* Abc\@def@example.com
* Fred\ Bloggs@example.com
* Joe.\\Blow@example.com
* "Abc@def"@example.com
* "Fred Bloggs"@example.com
* customer/department=shipping@example.com
* $A12345@example.com
* !def!xyz%abc@example.com
* _somename@example.com

[alert type="info"]The above samples are taken from RFC 3696, Application Techniques for Checking and Transformation of Names, written by the author of the SMTP protocol (RFC 2821) as a human readable guide to SMTP.[/alert]

As strange as it might seem, all the above e-mail addresses are "valid": or at least they were, according to RFC 2822 section 3.4.1, until it was obsoleted by  RFC 5322. However, even RFC 5322  takes as "valid" email addresses using a syntax that are widely considered to be simultaneously too strict (before the "@" character), too vague (after the "@" character), and too lax (allowing comments, whitespace characters, and quoted strings).

Here's another small list of "valid" email address as per RFC 5322:

* joe.blow@[IPv6:2001:db8::1]
* joe.blow(comment)@example.com
* joe.blow(comment)@(another comment with spaces)example.com
* "Joe..Blow"@example.com

... and so on. As we can see, we're still far from what we need for practical use.

## HTML5 Living Standards to the rescue
An "almost perfect" solution came with the release of the HTML living standards, which introduced a definition based upon a "willful violation" of RFC 5322 to overcome the above issues. The new definition was even backed up with a JavaScript and Perl-compatible regular expression that can be used to properly implement it:

/^[a-zA-Z0-9.!#$%&'*+\/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/

The above Regex is good enough to cut out most of the "odd address" above - and that's the reason why I've used it for a lot of my personal and business apps, as well as suggesting it to my fellow developers or colleagues. The only real issue I have with it  with th  it still allows the following:

admin@localhost
abc@cba

As a matter of fact, there is nothing wrong with the above e-mail addresses: such "dot-less" format is definitely valid and do have sense in most scenarios - for example, if we need to support "intranet" e-mail addresses or similar scenarios. However, when implementing a web-based service for external users, we might want to exclude those "dot-less" e-mail address from the valid ones.

## An arguably better fix
For that very reason, I've ended up implementing my own C# helper class that can be used to validate e-mail addresses with or without dots.

As you can see by looking at the code, the validation process relies upon the **IsValidEmailAddress** static function, which accepts the following parameters:

* **email** : the e-mail address to check / validate
* **useRegEx** : TRUE to use the HTML5 living standard e-mail validation RegEx, FALSE to use the built-in validator provided by .NET (default: FALSE)
* **requireDotInDomainName** : TRUE to only validate e-mail addresses containing a dot in the domain name segment, FALSE to allow "dot-less" domains (default: FALSE)

That's basically it: the above code is released under MIT license, meaning that you're free to use it for any project or use it to develop your own e-mail validatior function.

## Conclusion
That's it, at least for now: if you like the above code, feel free to give us a feedback in the comments section of this post.

## References
* [EMail Address Validation in C# and ASP.NET Core](https://www.ryadel.com/en/email-address-validation-c-sharp-asp-net-core/)
