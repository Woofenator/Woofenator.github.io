---
layout: single
title: Why you should implement your own Authentication
date: 2021-04-09T17:00:00+0300
categories: security
tags: api programming backend typescript authentication security
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/posts/2021-04-08.jpg
  caption: Photo by <a href="https://unsplash.com/@flyd2069?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">FLY:D</a> on <a href="https://unsplash.com/s/photos/lock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  show_overlay_excerpt: false
---

Among the ranks of Backend developers, no idea has received more scrutiny and warnings than implementing your own authentication system. Keeping with the tradition of making counterpoint laden articles of half-truths and personal anecdotes for no other reason than publicity, I shall now attempt to provide my personal view on the issue that's been so ingrained into modern day programming as to have become a mantra.

## Why not to do it

Just to prove I'm not an absolute madman, I shall begin this opinion piece with some sanity and disclaimers. In no way, shape or form am I advocating for the crazy few who decide to engineer their own authentication systems from absolute scratch. Neither am I advocating for those that think they are smarter than some pretty bonkers mathematicians and decide to make their own encryption/hashing algorithms. Both of those are very bad ideas for the simple reason that the tools and standards out there have been tested, re-tested, broken, bypassed and improved more times than reasonably necessary, and most of them have stood the test of time.

A good example of a well defined and tested system is the [Kerberos Protocol](https://en.wikipedia.org/wiki/Kerberos_(protocol)). It has been driving Microsoft's Active Directory Authentication for decades now, and probably will keep driving it for decades to come (mostly because Microsoft is extra dedicated to Backwards Compatibility, even going so far as introducing "bugs" to keep Excel backwards compatible for [spreadsheets created before 1980](https://en.wikipedia.org/wiki/Bug_compatibility#:~:text=Microsoft%20Excel%20has%20always%20had,Lotus%201%2D2%2D3.) in a different program entirely). That's not to say that the Original version of Kerberos has stood the test of time, it could probably be bypassed with spending around 15 minutes on google, but Kerberos has been developed and iterated on for decades, and because of its wide adoption, especially in exceptionally critical systems, it has to have handled edge cases we couldn't even imagine, let alone reason about. And yet it stands, evolves and becomes better over time, with the most recent version (as of writing), Version 5 Release 1.18.3, coming out in November of 2020. That is some serious longevity. It has had so many hands touch it, retouch it, break it apart and put it back together, that an Authentication system meant to replace it would have to stand up to 30 years of scrutiny (Version 5 itself was released in 1993), and no single developer can match that.

What I'm trying to say is: Lots and lots of hella smart people have both worked on and attempted to break open various Authentication protocols and the different password storage methods since the dawn of the internet, and the chances of one single person coming up with the next sliced bread are slim-to-none. However, at this point in time it means the web is the safest it's ever been from computer based attacks. It's gotten so good that the best thing a hacker could do these days is not attack a system and look for vulnerabilities, but attack the people using the system - finding previously used passwords in password dumps, abusing password resets, support staff, phishing, and as such gain access to where they shouldn't have any.

## Counterpoint

With that huge pile of words out of the way, I would like to present my idea of why one should implement an Authentication protocol on their own, instead of relying on pre-built packages and black-box solutions. It's a matter of understanding and insight into how the systems we use in our day-to-day lives function, to be able to build and expand on them, as well as use them to their fullest. For example, implementing [OAuth2](https://tools.ietf.org/html/rfc6749) for a personal project of mine, gave me understanding of the following:

- [JWT](https://jwt.io/) Authentication tokens
- What is and isn't supposed to go into JWT Tokens
- Ways to handle refresh tokens
- Token lifetime best practices
- One-Time Passwords
- How easy it is to use BCrypt

As it stands, I don't think that any other action as a developer (aside from diving into the topic by reading documentation... yuck!) would have given me such insight into these things. And a lot of this knowledge can be applied in other parts of a system as well to make them more performant. For example, had I not known what can go into a JWT token, I would have made an API that does the naive thing and implements a Passport JWT consumer, then checks if the user it somehow extracts exists in our database, which is a pretty standard thing to do these days (at least as far as I have seen of what my colleagues do).

With my newfound knowledge, I know that, with an asymmetrically signed token, I can be confident that the token has come from my API, I can check the issuer to know which system issued it, I can check its purpose to know what it was issued for, its scopes to know what the token has access to, as well as arbitrarily encode the user type in the token to restrict access to admin/premium/special sections of the API, and all without making a single call to the database.

Another part of this knowledge is that implementations like Passport.js aren't a black box to me anymore. I understand exactly what should be happening behind the curtain (even though I could check the source code, but ain't nobody got time for that) and where it can go tits up sometimes and how to triage it. It also helps when mocking said implementations in test suits, specifically in understanding the expected behavior and bypassing it to test my own code instead.

And that, in my opinion, is the biggest reason to write an implementation on your own - understanding. Understanding where your system goes wrong, where its weak points are how to patch them up (or, at least, how to submit an issue to a popular Authentication library). So go ahead, spread your wings into the wide and scary world of Security because, at the very least, you'll come out with an understanding and appreciation for what happens in the black boxes we use every day.
