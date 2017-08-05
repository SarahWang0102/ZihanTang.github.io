---
layout: post
title:  How to install QuantLib-Python on windows 10 with Visual Studio 2017 Community"
date:   2017-08-05 08:41:56 +0800
categories: tool setup
---

# Background
Setup package on Windows platform is a pain for most guys, especially for python beginners, you will encounter numerous bugs and errors. Recently I installed QuantLib 1.10 on Windows and suffers a brunch of Windows specific issues. The installation guide on QuantLib's website(http://quantlib.org/install/vc10.shtml) is out of date so that I decided to write this installation guide with solution of issues I met.

Here is the basic setup.
  - OS: Windows 10 64 bit
  - Python: conda with Python 3.6
  - QuantLib version: 1.10
  - Boost: 1.64
  - VS: Visual Studio 2017 Community

Basic steps are same as the official guide here http://quantlib.org/install/vc10.shtml, but I'll also record some windows10 or VS 2017 specific steps.

# Boost installation

Download boost binaries from its website http://sourceforge.net/projects/boost/files/boost-binaries/. Since we are install latest QuantLib with latest Visual Studio, I recommend to use latest stable binaries.

 - In my case, since I'm using Windows 10 64 bit and Visual Studio 2017, I choose to download this binary `boost_1_64_0-msvc-14.1-64.exe`. You should select the correct binary matching your setup.
 - Install boost from binary and remember the installation location. In my case, I install boost under `C:\ql\boost_1_64_0`.
 - We should record two paths that will be used in future steps. The boost path `C:\ql\boost_1_64_0` and the boost lib path `C:\ql\boost_1_64_0\lib64-msvc-14.1`

# QuantLib installation from source
Once we finished boost installation then we could begin QuantLib installation from source.

- Download QuantLib 1.10 from SourceForge http://sourceforge.net/projects/quantlib/files/QuantLib.
- Download and install Visual Studio 2017 Community if you don't have them on your laptop. You should also install C++ support and Python support since we will use them latter.
- Extract QuantLib source file to your local machine. In my case, they are under `C:\ql\QuantLib-1.10`. Open solution file  `QuantLib.sln` with Visual Studio 2017.
- Select RELEASE 64 from configuration menu.
- Setup SDK version to 10.xxxxxxx, which is not specified in the official installation guide.
- Setup INCLUDE directories under VC++ Directories in property page. Add boost path,`C:\ql\boost_1_64_0`, to the end of INCLUDE directories.
- Setup LIBRARY directories under VC++ Directories in property page. Add boost lib path, `C:\ql\boost_1_64_0\lib64-msvc-14.1`, to the end of LIBRARY directories.
- Shoot a click on build solution and it will build the QuantLib lib and run test suites for you.

# QuantLib-Python installation
Configure python environment is so stupid. I used conda to manage python environment
- Download QuantLib-SWIG 1.10 from SourceForge http://sourceforge.net/projects/quantlib/files/QuantLib.
- Download and install conda from source, remember to install conda on a non-system location, for instance `C:\Conda` for example. Installing conda under Program-File brought `sudo` similar issues.
- Install Python support from Visual Studio 2017.
- Open 64 native prompt provided by visual studio 2017.
- Setup env variables, it's different with official guide.
``` shell
set MSSdk=1
set DISTUTILS_USE_SDK=1
set QL_DIR=C:\ql\QuantLib-1.10
set INCLUDE=%INCLUDE%;C:\ql\boost_1_64_0
```
- activate your conda env `activate root`
- build `python setup.py build`
- test `python setup.py test`
- install `python setup.py install`
