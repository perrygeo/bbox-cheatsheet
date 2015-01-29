# Bounding Boxes, a case study in accidental complexity?

The humble bounding box. On the surface there's not much to it; a square defined by four bounding coordinates (west, south, east, north). This bounding box is a core construct in map rendering, spatial indexes and many spatial analysis techniques.

<img src="images/bbox.png">

Very simple and useful. Here's one way I can represent that as a data structure, also simple.

    bounding_box = (west, south, east, north)

I've chosen to call it a "Bounding Box" and define it's edges by a tuple of cardinal directions in (west, south, east, north) order. But all of these decisions are a matter of personal preference; it is but one of *many* ways that software can represent a bounding box. It turns out to be the cause of some complexity and a potential source for major bugs in geospatial applications.

## Fundamental issues
First, it's worth looking at Douglas Caldwell's
[Unlocking the Mysteries of the Bounding Box](http://www.stonybrook.edu/libmap/coordinates/seriesa/no2/a2.htm) to which this post is a sequel. He covers many of the fundamental issues with the bounding box representation. To summarize a few key points:

* Content Quandry: When collecting field observations, are you considering the extent of the observed data or the full extent that was searched?
* Global Gotchas: Dateline and other projection edge effects can invalidate your bbox. See Alaska and New Zealand and their dateline issue
* Projection Problems: bounding boxes themselves cannot be reprojected accurately without line densification.

Those are fundamental problems. Making the bounding box more complex to cover these edge cases is not worth the tradeoff in lost simplicity.  In other words, the bounding box is so simple and so useful that we rely on it nonetheless.

## Accidental complexity

As we often do, software developers have layered on an additional set of accidental complexity by implementing bounding boxes in a dazzling variety of ways.

### Naming the thing itself
I'm calling it a *bounding box* in this article. But in conversation and code, I
occasionally use *bbox*, *extent*, *envelope*, *bounds*, *minimum bounding rectangle*,
*MBR* and possibly others that I'm forgetting at the moment.
For the most part, these concepts are synonymous in everyday use. But
it's a source of ambiguity and distracts our mental effort from the problem at hand,
even if very slightly.

### The interface

How many ways can we describe the interface to this humble bounding box?

`GetBounds()` vs. `get_bounds()` ... ok I get that; it's just a difference in style
between programming languages. And of course syntax changes are to be expected. But do we also need ....

* `setExtent`
* `getMinX`
* `width`, `height`
* `ST_Extent`
* object method calls
* functions
* ordered list/tuple access by index
* parsing delimited strings
* etc?

Ultimately this isn't much of a drawback as it's pretty easy to understand after
a minute or two studying the docs. It just requires a little additional mental effort
when shifting between software.

### Notation for coordinates:
Just consider, for example, the x-coordinate for the lower left corner of the bounding rectangle. I'd call it `west`. Other acceptable notation includes

* x1
* minx
* xmin
* w
* left

This tends to take some mental effort to keep straight, again mostly when moving data and talking between software.



### Coordinate order:
The biggest source of potential bugs when working with bounding boxes is the coordinate ordering. Take a look at this bounding box and *quickly* tell me, which coordinate is `north`?

```bash
Extent: (-20037508.342789, -12294969.949537) - (19975801.618313, 18921895.237730)
```

It takes a few seconds to think about, doesn't it? And it requires knowledge of common conventions in your toolkit. In this case, this is the extent of a shapefile as reported by `ogrinfo`. I know that it reports in the following order:

```bash
Extent: (west, south) - (east, north)
```

But, because the notation is so different between software environments, it can be easy to make mistakes in manipulating bounding boxes if you aren't careful with your assumptions about coordinate order.
And it's difficult to identify the error
at first glance unless you happen to have the different bounding box representations memorized.

## The Bounding Box Cheatsheet

It's truly astounding how many different ways we've invented to communicate about
fundamentally the same concept. Ideally there would be some common representation, though standardization at this scale is not very likely. At least there should be a  good reference to quickly compare bounding boxes between software packages. This will reduce simple bugs due to the many quirks of the various implementations. That's what I hope to compile here:

In an attempt to build a comprehensive reference guide to bounding box implementations
in GIS software, head on over to in the <a href="reference.md">Bounding Box Cheatsheet</a>

No doubt you could probably build an abstraction layer for bounding coordinates in
your programming environment of choice. This project stops short of a software implementation but hopefully provides good supporting data should anyone want to write one.

## Contributing

I'm looking for some help to make this a complete reference for the GIS software community. If you have any additions, corrections or suggestions for how to organize and visualize this information, file an issue or a pull request on github.





