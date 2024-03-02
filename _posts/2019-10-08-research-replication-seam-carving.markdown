---
layout: post
title: "Research Replication: Seam Carving"
date: 2019-10-08
---

Seam Carving is a novel technique for doing content-aware image resizing aka "image retargeting". The idea is to remove seams of low-energy pixels one at a time until the image is truncated to the desired dimensions. In the papers cited below we are introduced to the concept and its applications.

- Shamir, A., & Avidan, S. (2007) Seam Carving for Content-Aware Image Resizing. [http://www.faculty.idc.ac.il/arik/SCWeb/imret/index.html](http://www.faculty.idc.ac.il/arik/SCWeb/imret/index.html)
- Rubinstein, M., Shamir, A., & Avidan, S. (2008) Improved Seam Carving for Video Retargeting. [http://www.faculty.idc.ac.il/arik/SCWeb/vidret/index.html](http://www.faculty.idc.ac.il/arik/SCWeb/vidret/index.html)

We present an implementation of the Seam Carving technique in pure JavaScript running in the browser. The implementation of the algorithm is summarized:

- Calculate image gradient in x and y direction
- Use dynamic programming to calculate minimum seam energies over the entire image
- Extract the minimum energy seam, this removes exactly 1 pixel along one dimension
- Repeat until the desired dimension is achieved

<p></p>

<iframe width="640" height="360" src="https://www.youtube.com/embed/dwXijLV_-Us?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

In the results below we see that a typical scaling operation will uniformly flatten scene objects, while retargeting keeps the size of certain elements intact.

<div style="width:51%;float:left">Scaled</div>
<div>Retargeted</div>
[<img src="/images/beach_scaled.png" width="49%">](/images/beach_scaled.png)
[<img src="/images/beach_carved.png" width="49%" style="float:right">](/images/beach_carved.png)

<p>Original</p>
[<img src="/images/beach.png">](/images/beach.png)
