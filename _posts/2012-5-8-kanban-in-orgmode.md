---
layout: post
title: Orgmode Export and Kanban
tags: [lisp, emacs]
published: false
---
<style type="text/css">
* { margin: 0; padding: 0; }
html, body { height: 100%; }
h1, h2, h3, h4, h5, h6 { font-size: 18px; font-family: sans-serif; font-weight: normal; }
h1, h2, h3 { letter-spacing: -1px; font-weight: bold; }
h1 { margin-bottom: 1em; font-size: 24px; font-weight: bolder; }
.title { font-size: 32px; }
#content { text-align: justify; width: 52em; margin: 3em auto 2em auto; line-height: 1.5em; }
.outline-1 { padding-left: 2em; margin-bottom: 2em; }
.outline-text-1 { padding-left: 2em; }
.outline-2 { padding-left: 2em; margin-bottom: 1em; }
.outline-text-2 { padding-left: 2em; }
.outline-3 { padding-left: 2em; margin-bottom: 1em; }
.outline-text-3 { padding-left: 2em; }
.outline-4 { padding-left: 2em; }
.done, .todo { font-weight: bold; letter-spacing: -1px; }
li { margin-left: 1em; }
table { border-collapase: collapse; }
table, th, td { border: 1px solid white; border-left: 8px solid white; border-right: 8px solid white; }
td { padding: 4px; }
th { background-color: #F90; }
tr:nth-child(2n) { background-color: #FF8; }
</style>

{{ page.title }}
================
<p class="meta">8 May 2012</p>

I read a post on using [orgmode with Kanban](http://www.agilesoc.com/2011/08/08/emacs-org-mode-kanban-pomodoro-oh-my/). I consider Kanban to be a simple method that helps track tasks and ensure that progress gets made.

Tasks are tracked on a board like this:

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption></caption>
<colgroup><col class="left" /><col class="left" /><col class="left" /><col class="left" />
</colgroup>
<thead>
<tr><th scope="col" class="left">Backlog</th><th scope="col" class="left">Analysis</th><th scope="col" class="left">Implementing</th><th scope="col" class="left">Done</th></tr>
</thead>
<tbody>
<tr><td class="left"></td><td class="left"></td><td class="left"></td><td class="left"><a href="#Configure==orgmode==for==kanban">Configure orgmode for kanban</a></td></tr>
<tr><td class="left"></td><td class="left"></td><td class="left"></td><td class="left"><a href="#Write==a==post==on==kanban==&amp;==orgmode">Write a post on kanban &amp; orgmode</a></td></tr>
<tr><td class="left"><a href="#World==Peace">World Peace</a></td><td class="left"></td><td class="left"></td><td class="left"></td></tr>
</tbody>
</table>

I use css rules in emacs to get the layout...

Links are made using [[link]] and <<anchor>> pairs.

`C-c C-e t` will import a default template into the file. It does this at the point, so `M-<` first to the start of the file is a good idea. These contain a large number of options which can mostly be removed:

~~~~
#+TITLE:     TODO.org
#+AUTHOR:    
#+EMAIL:     vwood@COMPUTER
#+DATE:      2012-05-09 Wed
#+DESCRIPTION: 
#+KEYWORDS: 
#+LANGUAGE:  en
#+OPTIONS:   H:3 num:t toc:t \n:nil @:t ::t |:t ^:{} -:t f:t *:t <:t
#+OPTIONS:   TeX:t LaTeX:t skip:nil d:nil todo:t pri:nil tags:not-in-toc
#+INFOJS_OPT: view:nil toc:nil ltoc:t mouse:underline buttons:0 path:http://orgmode.org/org-info.js
#+EXPORT_SELECT_TAGS: export
#+EXPORT_EXCLUDE_TAGS: noexport
#+LINK_UP:   
#+LINK_HOME: 
#+XSLT: 
~~~~

I usually trim this down to something like:

~~~~
#+TITLE:     TODO
#+LANGUAGE:  en
#+OPTIONS:   H:3 num:nil toc:nil \n:t @:t ::t |:t ^:{} -:t f:t *:t <:t 
#+OPTIONS:   skip:nil d:nil todo:t pri:nil 
~~~~

Changing the options num and toc to remove the numbered headings and table of contents respectively.

With this, you can then `C-c C-e b` to export to html and then use `browse-url` to open it in a browser.

Change some settings for sanity

Automatic export on changes

Automatic entries in table?
Automatic TODO/DONE? - find the source of whatever does links in orgmode.

Shifting entries functions
