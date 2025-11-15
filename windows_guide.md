# Build guide for Windows systems
Based on [this old Steam guide](https://steamcommunity.com/groups/mwmulti/discussions/1/343788552545812491) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup) and [this old OpenMW build guide](https://wiki.openmw.org/index.php?title=Development_Environment_Setup&oldid=6337) and [this OpenMW build guide](https://wiki.openmw.org/index.php?title=User:Gblues).

## Requirements
- Visual Studio (Boost requires additional MSVC v141 - VS 2017 C++ x64/x86 build tools (v14.16)) check [this github issue](https://github.com/boostorg/boost/issues/914)
- Git
- CMake
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
For OpenSceneGraph, you'll need to compile a few libraries beforehand. Namely:


- libjpeg
- libpng (needs zlib aswell)
- libcollada (needs boost libraries and libxml2)

__LIBJPEG__\
Download libjpeg from [sourceforge](https://gnuwin32.sourceforge.net/packages/jpeg.htm).
Run ``` nmake /f makefile.vs setup-v16 ``` to create Visual Studio project file and then compile the project.

__LIBPNG__\
Download zlib from [zlib.net](https://zlib.net) and compile it using CMake and Visual Studio. (Remember to copy zconf.h from the cmake build folder)\
Download libpng from [sourceforge](https://gnuwin32.sourceforge.net/packages/libpng.htm) and compile it.

__LIBCOLLADA__\
Download boost from [boost github](https://github.com/boostorg/boost/releases) and compile it. Remember to check shared build and zlib in CMake.\
Download libxml from [GNOME gitlab](https://gitlab.gnome.org/GNOME/libxml2/-/releases) and compile it.\
Clone collada-dom from [github repo](https://github.com/rdiankov/collada-dom) \
Change in dae.h line 28 and daeUtils.cpp line 21 ```#include <boost/filesystem/convenience.hpp>``` to ```#include <boost/filesystem/operations.hpp>``` and compile it.

__OPENSCENEGRAPH__\
Clone OpenMW-optimized OpenScenegraph from [this repo](https://github.com/OpenMW/osg) and open the source in CMake.\
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
Download the latest Qt5 from [Qt5_archive](https://download.qt.io/archive/qt/)

Create a separate build folder for Qt5. Consider downloading [jom](https://wiki.qt.io/Jom) to make the building phase use more than one core. Use this command to skip the building of unneeded qt5 components``` [path_to_qt5_src]\configure -prefix D:\tes3mp\bin\qt5 -opensource -nomake tests -nomake examples -skip qtwebengine -skip qtdeclarative -skip qtsvg -skip qtmultimedia -skip qtquickcontrols -skip qtquickcontrols2 -skip qtserialport -skip qtspeech -skip qtlocation ```

### FFmpeg
Building FFmpeg on Windows requires MSYS2. Check this [ffmpeg document](https://www.ffmpeg.org/platform.html#Windows) and this [Qt document](https://doc.qt.io/qt-6/qtmultimedia-building-ffmpeg-windows.html) for more information. Clone Ffmpeg repo from Github using ```git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg```

Normal installation routine is:

1. Open VS x64 Development Prompt
2. Navigate to your msys installation folder
3. Rename ```usr/bin/link.exe``` to something else, so it won't clash with the VS linker
4. Run ```msys2-shell -use-full-path```
5. Install necessary packages:
```
pacman -Syu
pacman -S make
pacman -S diffutils
pacman -S nasm
```
6. Navigate to FFmpeg folder (I just symlinked the tes3mp folder to home)
7. Make a new FFmpeg build folder
8. run ```[path_to_ffmpeg_source]/configure --prefix=[path_to_ffmpeg_bin] --disable-doc --enable-shared --disable-programs --toolchain=msvc --arch=x86_64 --target-os=win64 --extra-cflags="-I[path_to_zlib]/zlib/include/" --extra-ldflags="-LIBPATH:[path_to_zlib]../bin/zlib/Release ```
   Don't worry if the ARCH is x86 after configuring, all of the x86 architectures get merged to one x86 in configure (source: [stackoverflow](https://stackoverflow.com/questions/57281055/ffmpeg-compilation-with-arch-x86-64-always-compiles-with-x86)). You can check afterwards if the compiled dlls are 64 bit using this [notepad++ trick](https://www.greytrix.com/blogs/sageaccpacerp/2022/01/18/how-to-check-dll-is-32-bit-or-64-bit/).
9. ```make && make install```

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
- BUILD_STATIC

### MyGUI
Requires Freetype and SDL2

__Freetype__
Download Freetype from [sourceforge](https://sourceforge.net/projects/freetype/files/) and compile it.\
__SDL2__
Clone the 2.32.x release branch from [Github](https://github.com/libsdl-org/SDL/tree/release-2.32.x) ```  git clone https://github.com/libsdl-org/SDL.git -b release-2.32.x ``` and compile it.
