[![Build Status](https://travis-ci.org/fortyninemaps/karta.svg?branch=master)](https://travis-ci.org/fortyninemaps/karta)
[![Coverage Status](https://coveralls.io/repos/github/fortyninemaps/karta/badge.svg?branch=master)](https://coveralls.io/github/fortyninemaps/karta?branch=master)

![Karta](https://raw.githubusercontent.com/fortyninemaps/karta/gh-pages/images/karta_logo.png)

*Karta* is a package for spatial analysis in Python. It simplifies geospatial
data processing by providing efficient generic classes for vector and raster
data sources, as well as a selection of analysis functions.

For example, read or create vector geometries:

```python
point = Point((-130.0, 52.0), crs=LonLatWGS84)
line = read_geojson("linedata.json")
polygon = Polygon([(-515005.78, -1301130.53),
                   (-579174.89, -1282271.94),
                   (-542977.83, -1221147.82),
                   (-437864.05, -1251641.55),
                   (-438160.72, -1252421.48),
                   (-437961.28, -1285314.00)],
                   crs=NSIDCNorth)
```
Perform simple queries:
```python
point2 = Point((-25.0, 48.0), crs=LonLatWGS84)
point.distance(point2)          # Distance in geographical units
line.intersects(polygon)        # True or False
ch = polygon.convex_hull()      # Returns a new polygon
ch.to_shapefile("poly.shp")
```
Load and manipulate raster data:
```python
grid = read_gtiff("landsat_scene.tif")  # Leverages GDAL
grid.profile(line)              # Collect data along a line
grid.resample(500.0, 500.0)     # Return a grid resampled at a new resolution
```

The latest release is on PyPI (see [Installation](#installation)). Suggestions,
bug reports, test cases, and pull requests are welcome.

The latest stable release is 0.7.4. The repository master branch is considered a
moderately-unstable development branch.

## Documentation

See the [online manual](https://karta.fortyninemaps.com/kartadocs/introduction.html),
the [tutorial](http://www.fortyninemaps.com/kartadocs/_static/tutorial.html), or read the
[API documentation](http://www.fortyninemaps.com/kartadocs/reference.html).

The manual can be built offline using [Sphinx](http://sphinx-doc.org/). Building
the documentation requires [numpydoc](https://github.com/numpy/numpydoc).

## Package Overview

- **karta.crs**: coordinate reference systems and interface for geodetic
  calculations

- **karta.vector.geometry**: geometry classes `Point`, `Line`, `Polygon` and their
  corresponding `Multipart` types with associated methods such as length, area,
  intersections, membership testing, convex hulls, and affine transformations

- **karta.raster.grid**: `RegularGrid` class for raster data, with methods for
  clipping, resizing, and sampling

- **tests**: unit tests, to be run with `python tests/runtests.py`

## Formats

*Karta* provides a basic working interface to several of common file formats.
Currently implemented are:

- vector
    - GeoJSON (r,w)
    - ESRI Shapefiles (via GDAL) (r,w)
    - GPX (GPS eXchange) (r,w)
- raster
    - GeoTiff (via GDAL) (r,w)
    - ESRI ASCII Grid (r,w)

*Karta* implements the Python [`__geo_interface__`
attribute](https://gist.github.com/sgillies/2217756) for vector geometries. This
permits data to be exchanged between *Karta* and external modules that also
implement `__geo_interface__` (e.g.
[shapely](https://github.com/Toblerity/Shapely),
[fastkml](https://fastkml.readthedocs.org/en/latest/)).

## Installation

The easiest way to install in production is via `pip`. Installation requires a
recent version of `setuptools`:

    pip install -U setuptools

Then, to install the latest release from PyPI:

    pip install karta

### Building from source

Building from source requires Cython:

    pip install Cython

Then, clone the repository and install:

    git clone https://github.com/fortyninemaps/karta.git karta
    pip install karta/

## Dependencies

- numpy >= 1.7
- gdal >= 1.10
- pyproj >= 1.9
- blosc >= 1.2
- C99-compliant compiler

*Karta* supports Python 2.7 and Python 3.4+.

