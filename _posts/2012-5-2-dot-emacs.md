---
layout: post
title: .emacs
categories: [lisp, emacs]
published: false
---

{{ page.title }}
================
<p class="meta">2 May 2012</p>

Emacs is a very configurable editor. I once wanted to change a really annoying behaviour of Eclipse, I was told that in order to do so I would have to write a plugin. In order to do this I would have to learn the Eclipse APIs, create a plugin project (not a java project) and then figure out what part of Eclipse I needed to change.

In Emacs all I need to do is write some lisp. As little as a single function, or adding a single hook. All the documentation, all of the source is available *in* the editor. Emacs lacks the huge learning wall before you can configure it.

I still miss the automatic handling of imports. There are packages that do this, or so I'm told, but I have not been able to install them successfully. So it's not all gravy on this side of things.

So I've stuck with emacs, and configuring it. The one thing that eventually people want to do is to place all their configuration files under source control. The idea being that this configuration takes time and effort to develop, and then any machine can quickly become what you are used to.

Git
---
Git is my weapon of choice for this, although any VCS with free hosting would work well enough. The one caveat with storing a .emacs in source control is that you don't want the .emacs in source control. Since the .emacs file belongs in $HOME, this would mean your $HOME directory would be in source control which is bad for naming of the repository (Would it match every username?), the amount of files you want to ignore and possible conflicts with nested repositories (although git seems to be fine with this.)

I suggest instead you place everything into a .emacs.d/ directory. Placing a init.el file inside .emacs.d/ will be loaded by Emacs just like the .emacs file is. Although any .emacs will take precedence, so if you encounter problems that may be why.

I suggest any extra local configuration be loaded at the end of the init.el file, and that it be placed in a subdirectory of the .emacs.d/ directory. At the end of init.el you can `(load "~/.emacs.d/local/init.el")` to load it.

Packages
--------
I do suggest that any packages not be stored in the actual repository. 

Snippets, Workgroups, and Other Config
--------------------------------------

Operating Systems and Emacs Versions
------------------------------------
