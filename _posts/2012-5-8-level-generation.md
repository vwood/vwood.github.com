---
layout: post
title: Level Generation
tags: [games]
published: false
---

{{ page.title }}
================
<p class="meta">8 May 2012</p>

Simple level generation.

Totally random.

Different layers. (Back to front... Top to bottom.)
(i.e. background/foreground, layers of geology)
Heightmap (global layer state - displacement)

Constraints in locality (tiles, rewrite rules)

There have been no machine learning techniques yet - excepting Cellular automata which don't really count.

Perlin Noise is really a combination of different layers
(Fractal generation is more directly a hierarchy of layers in that the top layer determines what the bottom layer will look like.)

2D Hidden Markov Machines (Or FSA)
Have to use log-probabilities

Hierarchical/Layered HMM? Every other layer goes the other way? Undirected graph model?

Hand crafted FSA/MRF

Gibbs sampling

Biomes / Different levels of detail
