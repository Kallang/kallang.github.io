---
layout: post
title:  "Localizing iOS Project with Multiple Targets"
date:   2014-09-30 16:27:06
categories: ios
---

It's a common practice that we place multiple targets in one single Xcode project, in this way we can easily reuse the code and resources that are common.

  But how to localize the multiple targets, such as the `Bundle Display Name` and other strings?

# Localize the `Bundle Display Name`

1. Create a new file `en.lproj/InfoPlist.strings`, add it to the `target` you want to localize.
2. Click it in the project navigator, and check each language you want to localize.
3. Now you can add `CFBundleDisplayName = "You_App_Display_Name";` into each file.


# Localize the `App`

1. Create a new file `en.lproj/Localizable.strings`, add it to the `target` you want to localize.
2. Click it in the project navigator, and check each language you want to localize.
3. Now you can add localized strings into each file.
