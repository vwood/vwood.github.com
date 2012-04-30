---
layout: post
title: Channels
categories: [threads]
---

{{ page.title }}
================
<p class="meta">30 April 2012</p>

If I must use multiple threads, I communicate with channels. Channels allow different threads to produce and consume objects in a structured fashion.

This is also a good demonstration of the difference between a mutex and a semaphore. The mutex in the following is used to prevent multiple threads removing items at the same time, or adding them at the same time. It would not be necessary if there is only one consumer and one producer.

The two semaphores allow a thread to wait until there is something for it to do. One semaphore is for consumers to wait on, the other is for producers. After consuming or producing they then use the other semaphore to allow the other type to act.

<script src="http://gist.github.com/659733.js"> </script>

Here is an example. If you have two functions of type <code>void \*() (void \*)</code> called <code>producer</code> and <code>consumer</code> the producer can add a char by calling <code>channel_add(channel, char)</code> and the producer uses <code>channel_get(channel, ptr_to_char)</code>


<pre>
pthread_t prod, cons;
channel_t channel;

/* Channel with a single char */
channel_init(&amp;channel, 1, sizeof (char));

pthread_create(&amp;prod, NULL, producer, (void *) &amp;channel);
pthread_create(&amp;cons, NULL, consumer, (void *) &amp;channel);

pthread_join(prod, NULL);
pthread_join(cons, NULL);

channel_destroy(&channel);
</pre>

<div class="footnote">If I can't use threads then I run multiple non-communicating processes.</div>

