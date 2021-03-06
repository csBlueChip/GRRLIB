﻿GRRLIB
======

[![Build Status](https://travis-ci.org/GRRLIB/GRRLIB.svg?branch=master)](https://travis-ci.org/GRRLIB/GRRLIB)
[![Join the chat at https://gitter.im/GRRLIB/GRRLIB](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/GRRLIB/GRRLIB?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Table of Contents
-----------------

- [Introduction](#introduction)
 - ...What is it?
- [Developing for the Wii](#developing-for-the-wii)
 - ...How do I even start?
- [Downloading GRRLIB](#downloading-grrlib)
 - ...Where do I get it from?
- [Installing GRRLIB](#installing-grrlib)
 - ...How do I get it to a useable state?
- [Using GRRLIB](#using-grrlib)
 - ...What essentials do I need to know to get going?
- [Upgrading to v4.1.0 From Previous Versions of GRRLIB](#upgrading-to-v410-from-previous-versions-of-grrlib)
 - ...I upgraded and now my programs won't compile properly!?
- [Using GitHub](#using-github)
 - ...What is this Git thing that the L337 devs keep talking about?
- [Credits](#credits)
 - ...Who do I thank for all this free stuff?
- [Licence](#licence)
 - ...When you say "free" do you actually mean something else?


Introduction
------------

GRRLIB is a C/C++ 2D/3D graphics library for Wii application developers.  It is
essentially a wrapper which presents a friendly interface to the Nintendo GX
core.

This document is written to be viewed with equal clarity in either a web browser
or a text editor.

As of v4.1.0, GRRLIB is supplied as a standard C/C++ library (aka. archive)
called 'libgrrlib'.  Because GRRLIB processes JPEG and PNG images, it requires
the installation of the 'libjpeg' and 'libpngu' libraries.  'libpngu', in turn,
requires 'libpng' and 'libpng' requires 'libz'.  GRRLIB has FileIO functions
to allow real-time loading and saving of graphical data, and thus requires
'libfat'.  GRRLIB also has the possibility to use TrueType fonts, so
'libfreetype' is also required.

```
libgrrlib          <- 2D/3D graphics library
├── libfat         <- File I/O
├── libjpeg        <- JPEG image processor
├── libpngu        <- Wii wrapper for libpng
│   └── libpng     <- PNG image processor
│       └── libz   <- Zip (lossless) compression (for PNG compression)
└── libfreetype    <- TrueType font processor
```


Developing for the Wii
----------------------

Do not progress until you have installed and configured devkitPro.  Guides are
and assistance are available at http://devkitpro.org/index.php

If you have just performed a clean (re)install on your Windows PC, be sure to
reboot before you continue.


Downloading GRRLIB
------------------

You are invited to use "the latest Git master version" of GRRLIB at all times.

The Git repository is located at: https://github.com/GRRLIB/GRRLIB

There is a simple guide to "Using GitHub" later in this document.

This document will presume that you have downloaded "the latest Git master
version" to a directory called  C:\grr

Installing GRRLIB
-----------------

This guide is for Windows.  If you are using Linux, I am going to presume you
are smart enough to convert these instructions.
```
GRRLIB      is supplied as source code
libjpeg     is supplied as source code
libpngu     is supplied as source code
libpng      is supplied as source code
libz        is supplied as source code
libfreetype is supplied as source code
libfat      is supplied with devkitPro (i.e. preinstalled)
```

The easy way is to install GRRLIB and all the required libraries in a single
command:
```bash
  c:
  cd \grr\GRRLIB
  make clean all install
```

This process may take some time depending on the speed of your PC.

If you used the method above the following steps are not required, GRRLIB is
installed and you are ready to start developing Wii homebrew games.

If you want, you could install the libz, libpng, libpngu, libjpeg and
libfreetype libraries in a single command:
```bash
  c:
  cd \grr\GRRLIB\lib 
  make clean all install
```

Each library could also be installed individually:

To install libz
```bash
  c:
  cd \grr\GRRLIB\lib\zlib
  make clean all install
```

To install libpng
```bash
  c:
  cd \grr\GRRLIB\lib\png
  make clean all install
```

To install libpngu
```bash
  c:
  cd \grr\GRRLIB\lib\pngu
  make clean all install
```

To install libjpeg
```bash
  c:
  cd \grr\GRRLIB\lib\jpeg
  make clean all install
```

To install libfreetype
```bash
  c:
  cd \grr\GRRLIB\lib\freetype
  make clean all install
```

To install libgrrlib:
```bash
  c:
  cd \grr\GRRLIB\GRRLIB
  make clean all install
```


Using GRRLIB
------------

After everything is installed, simply put
```c
    #include <grrlib.h>
```
at the top of your .c/.cpp file and use the functions as required

You will also need to add
```make
  -lgrrlib -lfreetype -lfat -ljpeg -lpngu -lpng -lz
```
to the libs line in your makefile

...Remember the order of the libraries is critical.  You may (need to) insert
other libraries in the middle of the list, you may need to add others to the
start, or even the end - but do _not_ change the order of the libraries shown
here.

You do NOT need to place /anything/ in your application directory.

If you would like to see a working example of this, you can look at the example
found in: C:\grr\examples\template\source


Upgrading to v4.1.0 From Previous Versions of GRRLIB
----------------------------------------------------

Older versions of GRRLIB, required a line such as:
```c
  #include "../../../GRRLIB/GRRLIB/GRRLIB.h"
```
...to be placed at the top of each C file which uses GRRLB.
Because GRRLIB is now installed as a system library, this must be replaced with:
```c
  #include <grrlib.h>
```

Older versions of GRRLIB required the 'GRRLIB.h' and 'GRRLIB.c" files to be
present in every project which uses GRRLIB.
Because GRRLIB is now installed as a system library, these files are no longer
required and must be erased.
*WARNING* Be careful if you have edited (either of) these files.

Older versions of GRRLIB "passed 'structs'" and therefore Textured Images were
defined with:
```c
  GRRLIB_texImg  tex1, tex2;
```
Because GRRLIB now "passes 'pointers'" these definitions should be changed to:
```c
  GRRLIB_texImg  *tex1, *tex2;
```

With older versions of GRRLIB if the programmer (you) required access to the
mode and frame information, you were required to add one or more of the
following three lines:
```c
  extern GXRModeObj  *rmode;
  extern void        *xfb[2];
  extern u32         fb;
```
Because GRRLIB now does this for you automatically, these lines must be removed
from your code.


Using GitHub
------------

GitHub allows the developers to submit changes to the code in such a way that
these changes can be easily monitored, quickly merged together with other
changes. and (if necessary) reverted.

It also allows the power-users to gain access to the latest (often "in-test")
features.

To obtain the "cutting edge" codebase go to https://github.com/GRRLIB/GRRLIB/tree/master

 1. Create a directory to hold the code (e.g. C:\grr)
 2. Click the *Download ZIP* button if you don't have the proper tools installed on your PC or Clone the following URL https://github.com/GRRLIB/GRRLIB.git

All official releases can be found here https://github.com/GRRLIB/GRRLIB/releases


Credits
-------

#### Project Leader
* NoNameNo

#### Documentation
* Crayon
* BlueChip

#### Lead Coder
* NoNameNo

#### Support Coders
* Crayon
* Xane
* DragonMinded
* BlueChip
* elisherer

#### Advisors
* RedShade
* Jespa


Licence
-------

GRRLIB is released under the [MIT Licence](https://github.com/GRRLIB/GRRLIB/blob/master/LICENCE.TXT).
If we had chosen the GPL licence you would be +forced+ (legally required) to release your source code.
But in the spirit of "free as in FREE" we have left you with the +option+ to release your
source code.

We do +request+ that you tell others about us by naming our library (GRRLIB) in
the credits of your game/application.  And, if you +choose+ to do that, we
encourage you to use our logo to achieve it; You can find our logo here:
C:\grr\grrlib_logo.png
and here:
https://raw.githubusercontent.com/GRRLIB/GRRLIB/master/grrlib_logo.png
