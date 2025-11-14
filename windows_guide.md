# Build guide for Windows systems
Based on [this old Steam guide](https://steamcommunity.com/groups/mwmulti/discussions/1/343788552545812491) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup) and [this old OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup&oldid=6337)

## Requirements
- Visual Studio (Boost requires additional MSVC v141 - VS 2017 C++ x64/x86 build tools (v14.16)) check [this github issue](https://github.com/boostorg/boost/issues/914)
- Git
- CMake

## Required libraries

### OpenSceneGraph
For OpenSceneGraph, you'll need to compile a few libraries beforehand. Namely:


- libjpeg
- libpng (needs zlib aswell)
- libcollada (needs boost libraries and libxml2)

__LIBJPEG__\
Download libjpeg from [sourceforge](https://gnuwin32.sourceforge.net/packages/jpeg.htm).
Run ``` nmake /f makefile.vs setup-v16 ``` to create Visual Studio project file and then compile the project.

__LIBPNG__\
Download zlib from [zlib.net](https://zlib.net) and compile it using CMake and Visual Studio. (Remember to build both release and debug versions and also copy zconf.h from the cmake build folder)
Download libpng from [sourceforge](https://gnuwin32.sourceforge.net/packages/libpng.htm) and compile it.

__LIBCOLLADA__\
Download boost from [boost github](https://github.com/boostorg/boost/releases) and compile it. Remember to check shared build in CMake.
Download libxml from [GNOME gitlab](https://gitlab.gnome.org/GNOME/libxml2/-/releases) and compile it.
Clone collada-dom from [github repo](https://github.com/rdiankov/collada-dom) 
Change in dae.h line 28 and daeUtils.cpp line 21 ```#include <boost/filesystem/convenience.hpp>``` to ```#include <boost/filesystem/operations.hpp>``` and compile it.

__OPENSCENEGRAPH__\
Clone OpenMW-optimized OpenScenegraph from [this repo](https://github.com/OpenMW/osg) and open the source in CMake
Add these: ```-DBUILD_OSG_PLUGINS_BY_DEFAULT=0 -DBUILD_OSG_PLUGIN_OSG=1 -DBUILD_OSG_PLUGIN_DAE=1 -DBUILD_OSG_PLUGIN_DDS=1 -DBUILD_OSG_PLUGIN_TGA=1 -DBUILD_OSG_PLUGIN_BMP=1 -DBUILD_OSG_PLUGIN_JPEG=1 -DBUILD_OSG_PLUGIN_PNG=1 -DBUILD_OSG_PLUGIN_FREETYPE=1 -DBUILD_OSG_DEPRECATED_SERIALIZERS=0``` cmake arguments to avoid compilation of unneeded plugins.
Uncheck USE_3RDPARTY_BIN to make osg use our built libraries.
Plug in all the libraries and include folders for the requirements you built earlier. 


