---
layout: post
title: Reactive Programming
categories: [python]
---

{{ page.title }}
================
<p class="meta">29 April 2012</p>

Reactive programming is often the first programming people encounter. Spreadsheets contain this style of programming using cells. Cells can contain simple values, or they can have formulas that depend on other cells.

What's different is the way values change. When a cell gets updated, the cells that depend on it will be updated. This is a neat little paradigm.

The idea is you create cell objects, which hold values (or functions and arguments). These propagate updates to the cells that depend on them.

For example:

<pre>a = cell(5)
b = cell(10)
c = cell(lambda x, y: x + y, a, b)</pre>
Now the value of c is 15.
<pre>a.set(10)</pre>
Now the value of c is 20.

Propagating changes as soon as they occur can be useful. It can speed up the feedback cycle and minimise the time it takes to iterate.

But we do not have to propagate the changes when they happen. We can be lazy and do work only when the result is needed. If some computation takes a while to complete, all we need to do is flag all the children recursively as being dirty. Then we know when to perform the calculation.

Of course there can be a danger with lazily performing computations that use a lot of resources. So keep that it mind.

<script src="https://gist.github.com/648874.js?file=cell.py"></script>

<a href="http://pypi.python.org/pypi/Trellis" title="Trellis">Trellis</a> supports this paradigm in Python, and <a href="http://common-lisp.net/project/cells/" title="Cells">Cells</a> does the same in Common Lisp.

<div class="footnote">In retrospect, I really should have used the parent / child relationship for slot naming. I do maintain that 'i_depend_on' is much easier to read than dependancies, however.</div>
