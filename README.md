# clustr

Construct polygons from tagged points.

## Overview

Clustr takes a text file containing longitude/latitude points, tagged with a bit of text, and attempts to generate minimal polygons that "cover" those points, using an "alpha" parameter to determine the notion of "coverage". The polygons are written to an ESRI Shapefile, suitable for use in GIS software.

Most famously Clustr was used to generate the [Flickr Alpha Shapes](http://code.flickr.net/2008/10/30/the-shape-of-alpha/).

## Building clustr

You will need to have the GDAL library (http://www.gdal.org/) installed, with OGR support enabled, for writing the Shapefiles. You will also need the CGAL library (http://www.cgal.org/) installed.

Depending on your system configuration, you may need to edit the Makefile to point to the location of the GDAL and CGAL libraries and C/C++ headers. If you are using a Ubuntu machine then all you need to do is: 

```
apt-get install libgdal-dev libcgal-dev
```

To build clustr, simply run 'make'. The 'clustr' executable is self-contained (aside from shared libraries) and does not rely on any supporting files.

## Using clustr

```
$ clustr [-a <n>] [-p] [-v] <input> <output>

   -h, -?      this help message
   -v          be verbose (default: off)
   -a <n>      set alpha value (default: use optimal value)
   -p          output points to shapefile, instead of polygons
```

If <input> is missing or given as "-", stdin is used.

  * The input file should be formatted as: <tag> <lon> <lat>\n

  * The input file *must* be sorted. Piping it through sort(1) will
    suffice.

  * Tags must not contain spaces.

If <output> is missing, output is written to "clustr.shp". The output is
always an ESRI Shapefile.

The -p option simply converts the input file to a Shapefile containing the
input points. This can be useful for comparing the polygon output of clustr
to its input dataset in a GIS browser.

The -a option sets the value of "alpha", which in turn determines the
convexity of the output polygons. Larger values make more convex polygons,
smaller values make less convex polygons. Smaller values will also filter
out outlying points, which may be what you want.

Omitting -a or setting it to zero will cause clustr to use the CGAL
library's idea of "optimal alpha", which, if there are significant outliers
in the input file, may not actually be what you want. In general, we
recommend starting with a value of 0.001.

## Known limitations

At present, clustr does not handle polygon holes correctly.

## Bugs

[Yes.](https://github.com/straup/Clustr/issues) Help and suggestions are welcome.

## See also

* http://code.flickr.net/2008/10/30/the-shape-of-alpha/
* http://www.aaronland.info/weblog/2009/05/21/things/#shapefiles

## History

Clustr was written by Schuyler Erle <schuyler@nocat.net>

## License

Copyright (c) 2008 Yahoo! Inc. Clustr is licensed under the [GNU General Public License, version 2](https://www.gnu.org/licenses/gpl-2.0.html)
