## xatlas

[![Appveyor CI Build Status](https://ci.appveyor.com/api/projects/status/github/jpcy/xatlas?branch=master&svg=true)](https://ci.appveyor.com/project/jpcy/xatlas) [![Travis CI Build Status](https://travis-ci.org/jpcy/xatlas.svg?branch=master)](https://travis-ci.org/jpcy/xatlas) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A cleaned up version of [thekla_atlas](https://github.com/Thekla/thekla_atlas).

Mesh charting, parameterization and atlas packing. Suitable for generating unique texture coordinates for baking lightmaps.

[![](https://user-images.githubusercontent.com/3744372/43034067-5c09c1da-8d18-11e8-8490-25770f05e8e0.png)](https://user-images.githubusercontent.com/3744372/43034066-53a62dee-8d18-11e8-9767-0b38ed3fa2d3.png)

## Changes from thekla_atlas
* Smaller code size - from about 18 KLOC to 10 KLOC
* Easier to integrate and build - a single source/header file pair instead of around 120 files and 10 directories.
* Atlas resolution option for outputting multiple atlases.
* Flexible data description API for input meshes.
* Better tolerance of bad input geometry. Zero length edges and zero area faces are ignored.
* Support for packing multiple atlases/parameterizations into a single atlas.

## How to use

### Generate an atlas (simple API)

1. Create an empty atlas with `xatlas::Create`.
2. Add one or more meshes with `xatlas::AddMesh`.
3. Call `xatlas::Generate`. Meshes are segmented into charts, which are parameterized and packed into an atlas.

The `xatlas::Atlas` instance created in the first step now contains the result: each input mesh added by `xatlas::AddMesh` has a corresponding new mesh with a UV channel. New meshes have more vertices (the UV channel adds seams), but the same number of indices.

Cleanup with `xatlas::Destroy`.

[Example code here.](https://github.com/jpcy/xatlas/blob/master/extra/example.cpp)

### Generate an atlas (tools/editor integration API)

Instead of calling `xatlas::Generate`, the following functions can be called in sequence:

1. `xatlas::ComputeCharts`: meshes are segmented into charts.
2. `xatlas::ParameterizeCharts`: charts are flattened into 2D parameterizations.
3. `xatlas::PackCharts`: charts are packed into one or more atlases.

All of these functions take a progress callback. Return false to cancel.

You can call any of these functions multiple times, followed by the proceeding functions, to re-generate the atlas. E.g. calling `xatlas::PackCharts` multiple times to tweak options like unit to texel scale and resolution.

See the [viewer](https://github.com/jpcy/xatlas/tree/master/extra) for example code.

### Pack multiple atlases into a single atlas

1. Create an empty atlas with `xatlas::Create`.
2. Add one or more meshes with `xatlas::AddUvMesh`.
3. Call `xatlas::PackCharts`.

[Example code here.](https://github.com/jpcy/xatlas/blob/master/extra/example_uvmesh.cpp)

## TODO

* Segmentation: improve chart merging by using similar metrics to chart growing
* Segmentation/Parameterization: detect geometry with zero Gaussian curvature (e.g. a cylinder) and unwrap as a single chart
* Viewer: better lightmap baking
* Viewer: chart picking in scene and texture views

## Technical information / related publications

[Ignacio Castaño's blog post on thekla_atlas](http://the-witness.net/news/2010/03/graphics-tech-texture-parameterization/)

P. Sander, J. Snyder, S. Gortler, and H. Hoppe. [Texture Mapping Progressive Meshes](http://hhoppe.com/proj/tmpm/)

K. Hormann, B. Lévy, and A. Sheffer. [Mesh Parameterization: Theory and Practice](http://alice.loria.fr/publications/papers/2007/SigCourseParam/param-course.pdf)

P. Sander, Z. Wood, S. Gortler, J. Snyder, and H. Hoppe. [Multi-Chart Geometry Images](http://hhoppe.com/proj/mcgim/)

D. Julius, V. Kraevoy, and A. Sheffer. [D-Charts: Quasi-Developable Mesh Segmentation](https://www.cs.ubc.ca/~vlady/dcharts/EG05.pdf)

B. Lévy, S. Petitjean, N. Ray, and J. Maillot. [Least Squares Conformal Maps for Automatic Texture Atlas Generation](https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf)

O. Sorkine, D. Cohen-Or, R. Goldenthal, and D. Lischinski. [Bounded-distortion Piecewise Mesh Parameterization](https://igl.ethz.ch/projects/parameterization/BDPMP/index.php)

Y. O’Donnell. [Precomputed Global Illumination in Frostbite](https://media.contentapi.ea.com/content/dam/eacom/frostbite/files/gdc2018-precomputedgiobalilluminationinfrostbite.pdf)

## Used by

[Godot Engine](https://github.com/godotengine/godot)

[Wicked Engine](https://github.com/turanszkij/WickedEngine)

[Bakery - GPU Lightmapper](https://assetstore.unity.com/packages/tools/level-design/bakery-gpu-lightmapper-122218)

## Related projects

[Microsoft's UVAtlas](https://github.com/Microsoft/UVAtlas) - isochart texture atlasing.

[simpleuv](https://github.com/huxingyi/simpleuv/) - Automatic UV Unwrapping Library for Dust3D.

[Ministry of Flat](http://www.quelsolaar.com/ministry_of_flat/) - Commercial automated UV unwrapper.

[Lightmapper](https://github.com/ands/lightmapper) - Hemicube based lightmap baking. The example model texture coordinates were generated by [thekla_atlas](https://github.com/Thekla/thekla_atlas).

[aobaker](https://github.com/prideout/aobaker) - Ambient occlusion baking. Uses [thekla_atlas](https://github.com/Thekla/thekla_atlas).

[seamoptimizer](https://github.com/ands/seamoptimizer]) - A C/C++ single-file library that minimizes the hard transition errors of disjoint edges in lightmaps.

## Models used

[Gazebo model](https://opengameart.org/content/gazebo-0) by Teh_Bucket

[Tunnel scene](https://lmhpoly.com/unity-tutorial-volumetric-lighting/) by LMHPoly
