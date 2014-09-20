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

    # Animating the constant (Lion & Mountain Lion)
    [[myConstraint animator] setConstant:10.0];
    # Animation using CoreAnimation (Mountain Lion & iOS 6)
    NSAnimationContext runAnimationGroup:^(NSAnimationContext *ctx) {
      [ctx setAllowsImplicitAnimation:YES]
      ...
      [view layoutSubtreeIfNeeded];
    }

    [UIView animationWithDuration:2.0 animations:^{
      ...
      [view layoutIfNeeded];
    }];

# Helpful debugging defaults

    # Double all localized strings
    NSDoubleLocalizedStrings YES
    # Simulate right to left
    AppleTextDirection YES
    NSForceRightToLeftWritingDirection YES
    # Draw view alignment rects
    NSViewShowAllAlignmentRects YES
    UIViewShowAllAlignmentRects YES


# Post script

 You can learn a few keyboard shortcuts:

 1. `Ctrl+Shift+Click` in Interface Builder to show all classes.
 2. `CMD+Shift+O` to quick open, and use `Option+Shift+Return` in the search result list to select how to open the new corresponding file.
