---
title: "[Research] Visually understanding DoF with the thin lens model"
date: 2021-02-28 13:32:32 +0900
header:
    image: /assets/images/gallery/008.jpg
    caption: "Photo by Junyong Lee"
tags: [Research, thinlensmodel]
toc: true
categories: [Research, Camera]
---
**_Please contact me if there is any inaccurate content in this article._**  
  
I believe the thin lens model is the most straightforward way to understand how the depth of field (DoF) is formed within a camera.
We may simplify a complex lens model using the thin lens model for DoF to easily be expressed with several rays hitting the sensor of a camera.

---

# 1. Thin Lens Model

<p align="center"><img src="https://codeslake.github.io/assets/images/thinlensmodel/thinlensmodel.png" alt="drawing" width="80%"/></p>

The figure above explains the thin lens model.
In the figure, consider the left side of the lens as an object space, and the right side as an the inner space of a camera,
in which the focal plane (camera sensor) is located.
Here, $S\_1$ is the focal distance (*i.e.*, the distance between the lens and the focused object),
$F$ is the focal length of a lens,
$D$ is the effective aperture (*i.e.*, the diameter of diaphragm of a lens), where 
$D=\\frac{F}{N}$ ($N$ is f-number of the aperture).
Lastly, $f\_1$ is the distance between the lens and the focal plane ($f\_1=\\frac{fS\_1}{S\_1-f}$).
  
When $S\_1$ is in focus, the ray started from an object at $x$ creates the circle of confusion (CoC, *a.k.a.* bokeh) in the object space, as well as on the focal plane with the diameter size of $C(x)$ and $c(x)$, respectively.
The relation between $C(x)$ and $c(x)$ is defined as:

$ c(x)=m\_1 \\cdot m\_2 \\cdot C(x), \\tag{1} $
$ C(x)=D(\\frac{|x-S\_1|}{x}). \\tag{2} $

Here, $m\_1$ and $m\_2$ are magnification factors, where $m\_1=\\frac{I\_{width}}{S\_{width}}$ and $m\_2=\\frac{f\_1}{S\_1}=\\frac{f}{S\_1-f}$.
$I\_{width}$ and $S\_{width}$ represent the width of an image and the camera sensor, respectively.

If we let $m\_1=1$ and replace group of camera parameters ($f$, $f\_1$, and $N$) as $\\alpha$, Eq.1 can be reformulated as:

  
$ c(x)=\\alpha\\frac{\|x-S\_1\|}{x}. \\tag{3} $

  
If we draw the thin lens model with Eqs.1,2,3, we can visually check the change of CoC by playing with $x$ and $\\alpha=\\{S\_1, N, F\\}$:

<p align="center" style="position:relative;padding-bottom:56.25%;"><iframe src="https://www.desmos.com/calculator/rdejm3vlea?embed"  style="border: 1px solid #ccc;width:100%;height:100%;position:absolute;left:0px;top:0px;" width="100%" height="100%" frameborder=0></iframe></p>

---

# 2. Understanding Depth of Field with the Thin Lens Model
An image with deep DoF contains more sharp regions than an image with shallow DoF.
The meaning of deep DoF is that the focus range is wide, whereas the focused range is narrow for shallow DoF.
If there are two objects, $a$ and $b$, and let's say a camera is focused at $b$.
We can say DoF became shallow if CoC of $a$ got larger, or the other way around, if we modified the distance between $a$ and $b$ or changed camera parameters.

What are the factors making DoF shallow? If we google it, the answer narrows down to 4 factors.
1. when the f-number of the aperture is small,
2. when we use a lens with a high focal length,
3. when the distance between the focused object and a camera is small,
4. when the distance between the focused object and the defocused object is large.

Let's check if it is really true, using the thin lens graph above.

## 2-1. When the f-number of the aperture is small
Play with aperture ($N$) in the graph.
When $N$ gets larger, the diameter of CoC gets smaller. 
However, when $N$ gets smaller, the diameter of CoC gets larger.
In terms of DoF, when $N$ gets larger, DoF becomes deeper, and vice versa.

## 2-2. When we use a lens with a high focal length
Play with the focal length ($F$) in the graph.
When focal length is larger, CoC also gets larger.
If we look at the graph, there are two factors affecting CoC: $f\_1$ and $D$.
When $F$ increases, $f\_1$, $D$ and CoC get larger (i.e., DoF gets sallower).
When $F$ decreases, $f\_1$, $D$ and CoC get smaller (i.e., DoF gets deeper).

## 2-3. When the distance between the focused object and a camera is small
If we modify $S\_1$ in the graph, we may notice the change of $f\_1$.
When $S\_1$ increases, $f\_1$ gets smaller (i.e., DoF gets shallower).
When $S\_1$ decreases, $f\_1$ gets larger (i.e., DoF gets deeper).

## 2-4. When the distance between the focused and defocused objects is large
Play with $x$ in the graph.
When $x$ gets further apart from $S\_1$, CoC gets larger (i.e., DoF gets shallower).
When $x$ gets closer to $S\_1$, CoC gets smaller (i.e., DoF gets deeper).
  
---

# Reference

1.  [Circle of Confusion (Wiki)](https://en.wikipedia.org/wiki/Circle_of_confusion)
