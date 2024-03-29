---

title: Types of XSS
layout: col-sidebar
author:
contributors:
tags: xss
auto-migrated: 1
permalink: /Types_of_Cross-Site_Scripting

---

{% include writers.html %}

# Background

This article describes the many different types or categories of
cross-site scripting (XSS) vulnerabilities and how they relate to each
other.

Early on, two primary types of [XSS](attacks/xss/) were identified,
Stored XSS and Reflected XSS. In 2005, [Amit Klein defined a third type
of XSS](http://www.webappsec.org/projects/articles/071105.shtml), which
Amit coined [DOM Based XSS](attacks/DOM_Based_XSS). These 3 types of
XSS are defined as follows:

### [Reflected XSS](attacks/xss/#reflected-xss-attacks) (AKA Non-Persistent or Type I)

Reflected XSS occurs when user input is immediately returned by a web
application in an error message, search result, or any other response
that includes some or all of the input provided by the user as part of
the request, without that data being made safe to render in the browser,
and without permanently storing the user provided data. In some cases,
the user provided data may never even leave the browser (see DOM Based
XSS below).

### [Stored XSS](attacks/xss/#stored-xss-attacks) (AKA Persistent or Type II)

Stored XSS generally occurs when user input is stored on the target
server, such as in a database, in a message forum, visitor log, comment
field, etc. And then a victim is able to retrieve the stored data from
the web application without that data being made safe to render in the
browser. With the advent of HTML5, and other browser technologies, we
can envision the attack payload being permanently stored in the victim’s
browser, such as an HTML5 database, and never being sent to the server
at all.

### [DOM Based XSS](attacks/DOM_Based_XSS) (AKA Type-0)

DOM Based XSS (or as it is called in some texts, “type-0 XSS”) is an XSS attack wherein the attack payload is executed as a result of modifying the DOM “environment” in the victim’s browser used by the original client side script, so that the client side code runs in an “unexpected” manner. That is, the page itself (the HTTP response that is) does not change, but the client side code contained in the page executes differently due to the malicious modifications that have occurred in the DOM environment.

# Types of Cross-Site Scripting

For years, most people thought of these (Stored, Reflected, DOM) as
three different types of XSS, but in reality, they overlap. You can have
both Stored and Reflected DOM Based XSS. You can also have Stored and
Reflected Non-DOM Based XSS too, but that’s confusing, so to help
clarify things, starting about mid 2012, the research community proposed
and started using two new terms to help organize the types of XSS that
can occur:

  - Server XSS
  - Client XSS

## Server XSS

Server XSS occurs when untrusted user supplied data is included in an
HTTP response generated by the server. The source of this data could be
from the request, or from a stored location. As such, you can have both
Reflected Server XSS and Stored Server XSS.

In this case, the entire vulnerability is in server-side code, and the
browser is simply rendering the response and executing any valid script
embedded in it.

## Client XSS

Client XSS occurs when untrusted user supplied data is used to update
the DOM with an unsafe JavaScript call. A JavaScript call is considered
unsafe if it can be used to introduce valid JavaScript into the DOM.
This source of this data could be from the DOM, or it could have been
sent by the server (via an AJAX call, or a page load). The ultimate
source of the data could have been from a request, or from a stored
location on the client or the server. As such, you can have both
Reflected Client XSS and Stored Client XSS.

With these new definitions, the definition of DOM Based XSS doesn’t
change. DOM Based XSS is simply a subset of Client XSS, where the source
of the data is somewhere in the DOM, rather than from the Server.

Given that both Server XSS and Client XSS can be Stored or Reflected,
this new terminology results in a simple, clean, 2 x 2 matrix with
Client & Server XSS on one axis, and Stored and Reflected XSS on the
other axis as depicted in Dave Witchers’ DOM Based XSS talk \[2\]:

![Chart Server XSS vs Client XSS](assets/images/Server-XSS_vs_Client-XSS_Chart.png)

## Recommended Server XSS Defenses

Server XSS is caused by including untrusted data in an HTML response.
The easiest and strongest defense against Server XSS in most cases is:

  - Context-sensitive server side output encoding

The details on how to implement Context-sensitive server side output
encoding are presented in the OWASP [XSS (Cross Site Scripting)
Prevention Cheat
Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
in great detail.

Input validation or data sanitization can also be performed to help
prevent Server XSS, but it’s much more difficult to get correct than
context-sensitive output encoding.

## Recommended Client XSS Defenses

Client XSS is caused when untrusted data is used to update the DOM with
an unsafe JavaScript call. The easiest and strongest defense against
Client XSS is:

  - Using safe JavaScript APIs

However, developers frequently don’t know which JavaScript APIs are safe
or not, never mind which methods in their favorite JavaScript library
are safe. Some information on which JavaScript and jQuery methods are
safe and unsafe is presented in Dave Wichers’ DOM Based XSS talk
presented at OWASP AppSec USA in 2012 XSS \[2\]

If you know that a JavaScript method is unsafe, our primary
recommendation is to find an alternative safe method to use. If you
can’t for some reason, then context sensitive output encoding can be
done in the browser, before passing that data to the unsafe JavaScript
method. OWASP’s guidance on how do this properly is presented in the
[DOM based XSS Prevention Cheat
Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html). Note that this
guidance is applicable to all types of Client XSS, regardless of where
the data actually comes from (DOM or Server).

### References

\[1\] “DOM Based Cross Site Scripting or XSS of the Third Kind” (WASC
writeup), Amit Klein, July 2005

<http://www.webappsec.org/projects/articles/071105.shtml>

\[2\] “Unraveling some of the Mysteries around DOM Based XSS” (OWASP AppSec
USA), Dave Wichers, 2012

<https://owasp.org/www-pdf-archive/Unraveling_some_Mysteries_around_DOM-based_XSS.pdf>

### Related OWASP Articles

  - [Cross-site Scripting
    (XSS)](attacks/xss/)
  - [Stored
    XSS](attacks/xss/#stored-xss-attacks)
    (AKA Persistent or Type I XSS)
  - [Reflected
    XSS](attacks/xss/#reflected-xss-attacks)
    (AKA Non-Persistent or Type II XSS)
  - [DOM Based XSS](attacks/DOM_Based_XSS)
  - [XSS (Cross Site Scripting) Prevention Cheat
    Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
  - [DOM based XSS Prevention Cheat
    Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)

### Acknowledgements

A number of us in the industry have been discussing the new terms Server
XSS and Client XSS since mid 2012 and we all agree that these terms help
bring more clarity and order to the XSS terminology landscape.

  - Dave Wichers
  - Arshan Dabirsiaghi
  - Stefano Di Paolo
  - Mario Heiderich
  - Eduardo Alberto Vela Nava
  - Jeff Williams
