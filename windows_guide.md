# Build guide for Windows systems
Based on [this old Steam guide](https://steamcommunity.com/groups/mwmulti/discussions/1/343788552545812491) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup) and [this old OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup&oldid=6337)

## Requirements
- Visual Studio (Boost requires additional MSVC v141 - VS 2017 C++ x64/x86 build tools (v14.16)) check [this github issue](https://github.com/boostorg/boost/issues/914)
- Git
- CMake

## Required libraries

Some of the libraries used here have very outdated sources which confuses CMake and it produces errors like: 

>CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
>Compatibility with CMake < 2.8.12 will be removed from a future version of CMake.
>Update the VERSION argument <min> value or use a ...<max> suffix to tell
>CMake that the project does not need compatibility with older versions.


These are harmless on modern CMake versions and can be fixed by editing the CMakeLists.txt file and increasing the minimum compatible version to a larger version closer to the version you are using.
   

### OpenSceneGraph
For OpenSceneGraph, you'll need to compile a few libraries beforehand. Namely:


- libjpeg
- libpng (needs zlib aswell)
- libcollada (needs boost libraries and libxml2)

__LIBJPEG__\
Download libjpeg from [sourceforge](https://gnuwin32.sourceforge.net/packages/jpeg.htm).
Run ``` nmake /f makefile.vs setup-v16 ``` to create Visual Studio project file and then compile the project.

__LIBPNG__\
Download zlib from [zlib.net](https://zlib.net) and compile it using CMake and Visual Studio. (Remember to build both release and debug versions and also copy zconf.h from the cmake build folder)\
Download libpng from [sourceforge](https://gnuwin32.sourceforge.net/packages/libpng.htm) and compile it.

__LIBCOLLADA__\
Download boost from [boost github](https://github.com/boostorg/boost/releases) and compile it. Remember to check shared build in CMake.\
Download libxml from [GNOME gitlab](https://gitlab.gnome.org/GNOME/libxml2/-/releases) and compile it.\
Clone collada-dom from [github repo](https://github.com/rdiankov/collada-dom) \
Change in dae.h line 28 and daeUtils.cpp line 21 ```#include <boost/filesystem/convenience.hpp>``` to ```#include <boost/filesystem/operations.hpp>``` and compile it.

__OPENSCENEGRAPH__\
Clone OpenMW-optimized OpenScenegraph from [this repo](https://github.com/OpenMW/osg) and open the source in CMake
Add these: ```-DBUILD_OSG_PLUGINS_BY_DEFAULT=0 -DBUILD_OSG_PLUGIN_OSG=1 -DBUILD_OSG_PLUGIN_DAE=1 -DBUILD_OSG_PLUGIN_DDS=1 -DBUILD_OSG_PLUGIN_TGA=1 -DBUILD_OSG_PLUGIN_BMP=1 -DBUILD_OSG_PLUGIN_JPEG=1 -DBUILD_OSG_PLUGIN_PNG=1 -DBUILD_OSG_PLUGIN_FREETYPE=1 -DBUILD_OSG_DEPRECATED_SERIALIZERS=0``` cmake arguments to avoid compilation of unneeded plugins.
Uncheck USE_3RDPARTY_BIN to make osg use our built libraries.
Plug in all the libraries and include folders for the requirements you built earlier. 

### Crabnet
Clone [Crabnet](https://github.com/TES3MP/CrabNet) repo which is a fork of [Raknet](https://github.com/facebookarchive/RakNet) and compile it

### LZ4
Clone [LZ4](https://github.com/lz4/lz4) repo. Open build\VS2022\lz4.sln and compile liblz4-dll solution.

### Qt5
Qt5 depends on Perl and Ruby.\
Download Strawberry Perl from [their website](https://strawberryperl.com/)\
Download Ruby installer from [rubyinstaller.com](https://rubyinstaller.org/downloads/)\
Download the latest Qt5 from [Qt5_archive](https://download.qt.io/archive/qt/)\
