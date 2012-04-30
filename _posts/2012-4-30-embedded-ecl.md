---
layout: post
title: Embedded ECL
---

{{ page.title }}
================
<p class="meta">30 April 2012</p>

Common lisp is one of my favourite programming languages. It's also proof that language designers are human. It has a serious library problem. While ways of using C libraries exist, none of them are simple. I often conclude that I should write a C program that is extend-able via Lisp*. 

ECL is yet another open-source solution that suffers from a documentation problem. Not as large as some libraries, but it is somewhat lacking. So I created a minimal example of embedded usage. I would have thought this was the main use-case of ECL. The examples I found were far from minimal, and not written for Linux.

<script src="https://gist.github.com/662109.js"> </script>

Callbacks into C need to be written specifically for ECL, using the functions to turn 'cl_object' into c types, and back. All callbacks need to be of the form <pre>cl_object function([cl_object ...])</pre> These are registered into ECL using cl_def_c_function, which needs to be told the number of arguments. In the example code this is wrapped up in a macro. If packages are to be used the lisp call to export the functions would be best added to the macro. The example includes a tiny repl to try these out. Currently errors are not reported (which may be good or bad depending on application.)

Unfortunately I've found no way of limiting the calls you can make inside the lisp. This is the first step to providing a secure sandbox. The best way would be to write another eval in the lisp itself, as this is trivial.

<div class="footnote">*Other conclusions include lua, using SBCL and 'save-lisp-and-die' to create an executable image, she-bang scripts, and writing my own lisp.</div>
