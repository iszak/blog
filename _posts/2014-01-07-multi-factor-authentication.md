---
layout: post
title:  "Multi-factor authentication"
date:   2014-01-07 1:37:00
categories: multi-factor authentication lastpass google 1password
---
I recently enabled multi-factor authentication for both my Google account and [LastPass][1] account, it's amazing that I've held out for so long given the associated risk with losing either accounts. However after investigating multi-factor authentication solutions again, the backup options really sealed the deal, I don't know if they recently added support for them or I simply didn't invest enough time previously.

Google provides the ability to generate a collection of random numbers which can be used once as an authentication code, as well as the ability to use multiple phone numbers incase your number is inaccessible. For the applications which don't yet support multi-factor authentication but uses a Google account, you can generate an application specific code.

LastPass uses Google for multi-factor authentication however supports many other provides including [YubiKey][2], [Toopher][3], [Duo Security][4], [Transakt][5] all of which I've never heard of before. It also allows you to generate a one time password which will grant access to your account if you forget your password. Finally you can export your passwords to CSV and print it off if you're using randomly generated passwords which you don't remember.

In case you don't use multi-factor authentication I highly recommend it, I also recommend using LastPass as the idea of using unique passwords for multiple sites becomes cumbersome so I usually end up defaulting to five passwords with varying levels of entropy. In case you don't like the idea of storing your passwords in the "cloud" I hear [1Password][2] is also very good.

[1]: https://lastpass.com/
[2]: https://www.yubico.com/
[3]: https://www.toopher.com/
[4]: https://www.duosecurity.com/
[5]: https://gettransakt.com/
[6]: https://agilebits.com/onepassword
