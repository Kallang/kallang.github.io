---
layout: post
title:  "iOS Inter-App Communication"
date:   2014-05-31 11:04:36
categories: iOS
---

There are a few options for sharing between apps:

* `Keychain`, entitlements required.
* `UIPasteboard` for copy and paste.
* `UIDocumentInteractionController ` for exporting documents to other applications, using UTIs to advertise what types your app can receive through this technique.
* `UIActivity` for advertising your ability to handle data from other apps, but not as a document. Built-in activities include things like posting to social networks, adding to the camera roll, etc.
* `OpenURL` can communicate with other app with HTTP-GET like method, such as mail://to=kallang@outlook.com

References:

* [New Skepticism About the Old iOS Inter-App Communication Problem][inter-app-blog]

[inter-app-blog]: http://www.subfurther.com/blog/2014/05/29/new-skepticism-about-the-old-ios-inter-app-communication-problem/
