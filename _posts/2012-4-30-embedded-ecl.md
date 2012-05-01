---
layout: post
title: Embedded ECL
categories: [lisp]
---

{{ page.title }}
================
<p class="meta">30 April 2012</p>

Common lisp is one of my favourite programming languages. It's also proof that language designers are human. It has a serious library problem. While ways of using C libraries exist, none of them are simple. I often conclude that I should write a C program that is extendable via Lisp [^1]. 

ECL is yet another open-source solution that suffers from a documentation problem. Not as large as some libraries, but it is lacking. So I created a minimal example of embedded usage. I would have thought this was the main usecase of ECL. The examples I found were far from minimal, and not written for Linux.

<script src="https://gist.github.com/662109.js"> </script>

Callbacks into C need to be written specifically for ECL, using the functions to turn 'cl_object' into c types, and back. All callbacks need to be of the form <code>cl_object function([cl_object ...])</code>. These are registered into ECL using cl_def_c_function, which needs to be told the number of arguments. In the example code this is wrapped up in a macro. If packages are to be used the lisp call to export the functions would be best added to the macro. The example includes a tiny repl to try these out. Currently errors are not reported (which may be good or bad depending on application.)

[^1]:Other conclusions include lua, using SBCL and 'save-lisp-and-die' to create an executable image, she-bang scripts, and writing my own lisp.
