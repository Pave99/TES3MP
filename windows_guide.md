# Build guide for Windows systems
Based on [this old Steam guide](https://steamcommunity.com/groups/mwmulti/discussions/1/343788552545812491) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup) and [this old OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup&oldid=6337) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=User:Gblues).

## Requirements
- Visual Studio 2019
- Git
- An old(ish) CMake, preferably a 3.x
- A lot of time

## Required libraries

Some of the libraries used here have very outdated sources which confuses CMake and produces errors like: 

>CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
>Compatibility with CMake < 2.8.12 will be removed from a future version of CMake.
>Update the VERSION argument <min> value or use a ...<max> suffix to tell
>CMake that the project does not need compatibility with older versions.


These are harmless on modern CMake versions and can be fixed by editing the CMakeLists.txt file and increasing the minimum compatible version to a larger version, closer to the version you are using.

Unless otherwise stated you'll need to build both Release and Debug versions of the library.

### OpenSceneGraph
For OpenSceneGraphs dependencies, you can use the (now archived) prebuilt dependency archive for Visual Studio 2017 x64 referenced [here](https://openscenegraph.github.io/OpenSceneGraphDotComBackup/OpenSceneGraph/www.openscenegraph.com/index.php/download-section/32-third-party.html). It can be downloaded from [Internet Archive](https://web.archive.org/web/20240221120943if_/https://download.osgvisual.org/3rdParty_VS2017_v141_x64_V11_full.7z).

Clone OpenMW-optimized OpenScenegraph from [this repo](https://github.com/OpenMW/osg) and open the source in CMake.\
Add these: ```-DBUILD_OSG_PLUGINS_BY_DEFAULT=0 -DBUILD_OSG_PLUGIN_OSG=1 -DBUILD_OSG_PLUGIN_DAE=1 -DBUILD_OSG_PLUGIN_DDS=1 -DBUILD_OSG_PLUGIN_TGA=1 -DBUILD_OSG_PLUGIN_BMP=1 -DBUILD_OSG_PLUGIN_JPEG=1 -DBUILD_OSG_PLUGIN_PNG=1 -DBUILD_OSG_PLUGIN_FREETYPE=1 -DBUILD_OSG_DEPRECATED_SERIALIZERS=0``` cmake arguments to avoid compilation of unneeded OSG plugins.\
Make sure to adjust the 3rdparty argument to the right folder. CMake doesn't resolve all arguments correctly by itself, you'll need to point at least freetypes include and lib arguments and collada to the 3rdparty folder.\
Not all libraries are found in the 3rdparty folder, but make sure CMake finds Freetype, COLLADA, JPEG, PNG and TIFF.

### Crabnet
Clone [Crabnet](https://github.com/TES3MP/CrabNet) repo which is a fork of [Raknet](https://github.com/facebookarchive/RakNet) and compile it

### LZ4
Clone [LZ4](https://github.com/lz4/lz4) repo. Use build\cmake as the CMake source folder and compile it.

### Qt5
Download Qt5 5.12.12 binaries from [qt.io](https://download.qt.io/new_archive/qt/5.12/5.12.12/). You'll need to create a free account to install it. During component selection make sure to check MSVC 2017 64bit.

### FFmpeg
Download FFmpeg 4.4.1 binaries from [github](https://github.com/GyanD/codexffmpeg/releases?q=4.4&expanded=true), specifically ```ffmpeg-4.4.1-full_build-shared.7z```

### Bullet
Clone Bullet [repo](https://github.com/bulletphysics/bullet3)\
In CMake uncheck the following options:
- BUILD_BULLET2_DEMOS
- BUILD_BULLET3
- BUILD_CPU_DEMOS
- BUILD_EXTRAS
- BUILD_OPENGL3_DEMOS
- BUILD_UNIT_TESTS
And check these:
- INSTALL_LIBS
- USE_MSVC_RUNTIME_LIBRARY_DLL
- USE_DOUBLE_PRECISION

### MyGUI
Download MyGUI 3.4.1 from [github releases](https://github.com/MyGUI/mygui/releases).
Point freetype to the osg 3rdparty folder.\
Change MYGUI_RENDERSYSTEM to 1 and unselect all MYGUI_BUILD checkboxes. ([source](https://wiki.openmw.org/index.php?title=User:Gblues#MyGUI))


