---
layout: post
title: .emacs
categories: [lisp, emacs]
---

{{ page.title }}
================
<p class="meta">2 May 2012</p>

[Emacs](http://www.gnu.org/software/emacs/) is a very configurable editor. I once wanted to change a really annoying behaviour of Eclipse, and I was told that in order to do so I would have to write a plugin. In order to do this I would have to learn the Eclipse APIs, create a plugin project (not a java project) and then figure out what part of Eclipse I needed to change.

In Emacs all I need to do is write some lisp. As little as a single function, or adding a single hook. All the documentation, all of the source is available *in* the editor. Emacs lacks the huge learning wall before you can configure it.

I still miss the automatic handling of Java imports (Though this is really a failing of Java). There are packages that do this, or so I'm told, but I have not been able to install them successfully. So it's not all gravy on this side of things.

So I've stuck with emacs, and configuring it. The one thing that eventually people want to do is to place all their configuration files under source control. The idea being that this configuration takes time and effort to develop, and then any machine can quickly become what you are used to.

Git
---
Git is my weapon of choice for this, although any VCS with free hosting would work. The one caveat with storing a .emacs in source control is that you don't want the .emacs in source control. Since the .emacs file belongs in $HOME, this would mean your $HOME directory would be in source control which is bad for naming of the repository (Would it match every username?), the amount of files you want to ignore and possible conflicts with nested repositories (although git seems to be fine with this.)

I suggest instead you place everything into a .emacs.d/ directory. Placing a init.el file inside .emacs.d/ will be loaded by Emacs just like the .emacs file is. Although any .emacs will take precedence, so if you encounter problems that may be why.

I suggest any extra local configuration be loaded at the end of the init.el file, and that it be placed in a subdirectory of the .emacs.d/ directory. At the end of init.el you can `(load "~/.emacs.d/local/init.el")` to load it.

Packages
--------
I do suggest that any packages not be stored in the actual repository. You can add them as Git submodules (essentially a link to another repository), but that means you have to handle installation and they have to be in a Git repository.

Far better is to use either [ELPA](http://tromey.com/elpa/), or [el-get](https://github.com/dimitri/el-get) to handle the packages. Either allows a short amount of elisp inside the init.el to load any number of packages (less in ELPA).

Snippets, Workgroups, and Other Config
--------------------------------------
Configuration doesn't just end at elisp. [YaSnippet](https://github.com/capitaomorte/yasnippet) and [Workgroups](https://github.com/tlh/workgroups.el) are two that are nice to accumulate in an emacs configuration. 

This elisp loads stored workgroups, if they exist. All you have to do is save the workgroups at ~/.emacs.d/workgroups and they will be restored on start up:

~~~~
(let ((wg-location "~/.emacs.d/workgroups"))
    (when (file-exists-p wg-location) (wg-load wg-location)))
~~~~



Operating Systems and Emacs Versions
------------------------------------
`emacs-major-version` and `system-type` are important variables to allow your config to work around any inconsistency between operating systems or emacs versions. In particular the system-type is useful for configuring cygwin on Windows, should you have to deal with that.

Don't start from scratch
------------------------
There is no reason to start from scratch. There are a large number of emacs configurations available to begin from. Several of the default settings of Emacs are a bit puzzling. Of course I doubt anyone would ever completely agree with someone else's configuration, but it's a better start than default Emacs.
