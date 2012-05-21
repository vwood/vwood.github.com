---
layout: post
title: Emacs, Compile on save
tags: [emacs]
---

{{ page.title }}
================
<p class="meta">21 May 2012</p>

Compiling in emacs can be done with the `M-x compile` command. The defaults settings aren't so good. So I'll share how to fix them.

Replace compile with [smart-compile](http://www.emacswiki.org/emacs/SmartCompile) or [mode-compile](http://emacswiki.org/emacs/ModeCompile) as fast as you can. These both use the filename or major mode of a buffer to pick a compile command. Getting rid of the prompts about options or commands is the first step to getting automatic compilation (and faster feedback). Getting mode-compile is as easy as using [el-get](https://github.com/dimitri/el-get): `(el-get 'sync '(mode-compile))`

Unfortunately `M-x mode-compile` still asks for arguments to the compiler. We can specify nothing everytime it asks for information like this:

~~~
(defun mode-compile-quiet ()
  (interactive)
  (flet ((read-string (&rest args) ""))
    (mode-compile)))
~~~

Now we can use mode-compile-quiet as an alternative to mode-compile. I prefer not to explicitly call M-x commands and so I would normally give it a keybinding. However, what I really want is for it to compile whenever I've made an edit so perhaps there is a hook[^1] that happens after changes like `after-change-functions` that can be set per buffer.

What I ended up doing though, is compiling after saving. This means the file is already saved and I don't have to save to a temporary file, and I save habitually anyway. So `after-save-hook` it is. We should toggle the setting so it can be turned off. It's important to also send a message to show the current setting.

~~~
;; C-c C-% will set a buffer local hook to use mode-compile after saving
(global-set-key '[(ctrl c) (ctrl %)]
                (lambda () 
                  (interactive)
                  (if (member 'mode-compile-quiet after-save-hook)
                      (progn
                        (setq after-save-hook 
                            (remove 'mode-compile-quiet after-save-hook))
                        (message "No longer compiling after saving."))
                    (progn
                      (add-to-list 'after-save-hook 'mode-compile-quiet)
                      (message "Compiling after saving.")))))
~~~

Finally, since I use tons of workgroups I prefer to name the compilation buffer something more specific than `*compilation*`. This allows us to have multiple compile buffers - it names it after the buffer from which you call compile (or mode-compile):

~~~
;; Name compilation buffer after the buffer name
(setq compilation-buffer-name-function 
      (lambda (mode) (concat "*" (downcase mode) ": " (buffer-name) "*")))
~~~

With the above in place it is easy to have the code you are working on in one window, and the result of compilation in another window. Whenever you save the compilation gets updated. Did I mention any printing to stdout or stderr gets placed in the compilation buffer as well? That makes it pretty useful.

If you're working on a program that outputs to a separate file it can be useful to have it also output `-*- mode: auto-revert -*-` on the first line[^2]. This turns on auto-revert-mode which will reload the contents of the buffer whenever the file changes.

If you are using files a lot, the compilation buffer can be annoying, so we have two options, the first is to prevent the `*compilation*` buffer from appearing at all. It will still be created, but it will have to be switched to explicitly (which allows us to still examine the output if you want):

~~~
;; Prevent compilation buffer from showing up
(defadvice compile (around compile/save-window-excursion first () activate)
  (save-window-excursion ad-do-it))
~~~

Or, we could allow the buffer to show up, but if compilation is successful then go back to the previous window configuration. This means you can't see the output, but here it is:

~~~
;; Bury the compilation buffer when compilation is finished and successful.
(add-to-list 'compilation-finish-functions
             (lambda (buffer msg)
               (when 
                 (bury-buffer buffer)
                 (replace-buffer-in-windows buffer))))
~~~

Finally it is useful to scroll to the first error output in the buffer. This also scrolls to the bottom of the output if no errors occur, which is also useful:

~~~
(setq compilation-scroll-output 'first-error)
~~~

All of the code is a part of [my .emacs directory](https://github.com/vwood/.emacs.d). Both solutions to preventing the compilation buffer are there because the defadvice overrides the second one.

[^1]: Hooks are callbacks that occur at certain events in Emacs.
[^2]: or second if you specify a `#!` line in the first.
