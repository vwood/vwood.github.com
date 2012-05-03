---
layout: post
title: Asynchronous Channels
categories: [threads]
---

{{ page.title }}
================
<p class="meta">30 April 2012</p>

I've found a need for the <a href="http://vwood.org/dotplan/index.php?/archives/2-Channels.html" title="channels">channels</a> I created.  I plan on having a GUI library in one thread, handling windows, and a REPL in another. So I'm using channels to communicate.  The REPL could be in a separate window handled by the GUI library, but that's no fun. Interfaces should be responsive. There's no reason to be unable to interact with the window just because the REPL is doing something.

So here is the new version with non-blocking versions of add and get: <script src="https://gist.github.com/702933.js"> </script>

By using sem_trywait instead of sem_wait, we become non-blocking. This means the main loop of the GUI with end up checking for events and processing them, then attending to the channel. Like so:

<pre>while (True) {
    if (channel_tryget(c, item))
        process_item(item);
    while (gtk_events_pending())
        gtk_main_iteration();
}</pre>
