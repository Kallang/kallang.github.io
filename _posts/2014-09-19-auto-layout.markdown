---
layout: post
title:  "Auto Layout"
date:   2014-09-19 21:08:01
categories: ios
---
# How to learn auto layout?

 I think the best way is to watch the WWDC 2012 session videos as follows:

 1. 202 - Introduction to Auto Layout for iOS and OSX.mov
 2. 228 Best Practices for Mastering Auto Layout
 3. 232 Auto Layout By Example

# Useful lldb Commands

    po [[UIWindow keyWindow] _autolayoutTrace]
    p [view hasAmbiguousLayout]
    [view exerciseAmbiguityInLayout]
    po [view constraintsAffectingLayoutForAxis:UILayoutConstraintAxisHorizontal/Vertical]

# Animation with CoreAnimation

    // Adjust your constraints first
    // Then call then following code within an animation block
    [view layoutIfNeeded]
