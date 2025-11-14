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
- libcollada (needs boost libraries)

LIBJPEG
Download libjpeg from [sourceforge](https://gnuwin32.sourceforge.net/packages/jpeg.htm).
Run ``` nmake /f makefile.vs setup-v16 ``` to create Visual Studio project file and then compile the project.

LIBPNG
Download zlib from [zlib.net](https://zlib.net) and compile it using CMake and Visual Studio (Remember to build both release and debug versions and also copy zconf.h from the cmake build folder)
Download libpng from [sourceforge](https://gnuwin32.sourceforge.net/packages/libpng.htm) and compile it.

LIBCOLLADA
Download boost from [boost.org](https://www.boost.org/releases/latest) 
Clone collada-dom from [github repo](https://github.com/rdiankov/collada-dom)
