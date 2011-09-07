jQuery-ColumnView
=================

A simple script to transform a hierarchical HTML list into a Miller
Column UI (AKA, Mac OS X Finder style, NeXT style column view).

This plugin was expanded to include:
- using custom events instead of callbacks
- support a dblclick event
- loading of additional content with a custom callback (for example to load subtrees using ajax)
- navigating to a specific element
- clicking on nodes programmatically
- enabling/disabling adding CSS automatically

Compatibility
-------------

The plugin works fine with jQuery 1.6.2. It requires jQuery 1.5.x or
newer because of its use of deferreds.

Usage
-----

See demo.html for examples of usage.

Ongoing Debate
--------------

Keep track of comments by non-github users at
[my blog](http://christianyates.com/blog/jquery/finder-column-view-hierarchical-lists-jquery).
