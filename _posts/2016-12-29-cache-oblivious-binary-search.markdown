---
layout: post
title: "Cache Oblivious Binary Search"
date: 2016-12-29 12:00:00
---

The goal of [Cache Oblivious algorithms](https://en.wikipedia.org/wiki/Cache-oblivious_algorithm)
is to efficiently use data in a CPU cache and reduce unnecessary swapping of
data between slow to fast memory. The big win of a cache oblivious algo is that
this efficiency works at any level of a memory hierarchy, e.g. from RAM to CPU
cache or from Hard Disk to RAM.

We attempt to implement a cache oblivious binary search using ideas from the
paper: "Cache Oblivious Search Trees via Binary Trees of Small Height" by
Brodal, Fagerberg, and Jacob. Included is an implementation to convert a
binary search tree from an implicit inorder layout to implicit van Emde Boas
layout.

Initial results are promising, cache misses for a van Emde Boas based search
are lower than inorder.

[<img src="https://raw.githubusercontent.com/jlas/sample-code/master/datastructures/binsearch/experiments/media/cache_misses.png">](https://raw.githubusercontent.com/jlas/sample-code/master/datastructures/binsearch/experiments/media/cache_misses.png)

However, the running time of the van Emde Boas based search is higher. We
suspect this may be due to the non-trivial overhead of traversing a vEB tree.

[<img src="https://raw.githubusercontent.com/jlas/sample-code/master/datastructures/binsearch/experiments/media/time_elapsed.png">](https://raw.githubusercontent.com/jlas/sample-code/master/datastructures/binsearch/experiments/media/time_elapsed.png)

Link to code: [github.com/jlas/sample-code/tree/master/datastructures/binsearch](https://github.com/jlas/sample-code/tree/master/datastructures/binsearch)
