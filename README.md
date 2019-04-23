# Hunter Package #

This fork of **Google Test** is used by the [hunter package manager](https://github.com/ruslo/hunter).
This repository is used for package versions `1.8.0-hunter-p2` or later. Older package versions use
the repository [https://github.com/hunter-packages/gtest](https://github.com/hunter-packages/gtest).

## Modifications ##

* Add an install target that also generates `<target>Config.cmake` files which allows installing the project
as a relocatable config file based CMake package. [https://cmake.org/cmake/help/v3.10/manual/cmake-packages.7.html#id1](https://cmake.org/cmake/help/v3.10/manual/cmake-packages.7.html#id1) 

* Add interface compile definitions for the dll export macros when building as shared library with **MSVC**.
This reliefes the consumers from manually setting the `GTEST_LINKED_AS_SHARED_LIBRARY=1` compile definition for their
targets.

## Usage Example ##

````cmake
... 

hunter_add_package( GTest )
find_package( GTest 1.8.0-hunter-p8 CONFIG REQUIRED )

add_executable( MyTestExe test.cpp )
target_link_libraries( MyTestExe PRIVATE GTest::gtest )

...
````

The package defines the following targets. `Gtest::gtest`, `Gtest::main`, `GMock::gmock` and `GMock::main`.
Note that the names of the `main` targets have been changed compared to the original **Google Test** project.
Link to one of the `main` targets when you want to use the default implementation of the `main()`
function that is provided by **Google Test**.

## Caveats ##

* Between package versions `1.7.0-hunter-11` and `1.8.0-hunter-p2` the package changed its repository
because **GTest** and **GMock** were merged into a single repository. This means that the `GMock::...` targets
are only available for versions later than `1.7.0-hunter-11`.

* When using the package via [Git submodule](https://docs.hunter.sh/en/latest/user-guides/hunter-user/git-submodule.html),
the compilation of the package may fail when copying the license file. This happens for versions later than `1.7.0-hunter-11`.
To fix the problem set the path to the license file in your local `config.cmake` by using `hunter_config( GTest GIT_SUBMODULE <mySubmodulePath> CMAKE_ARGS HUNTER_INSTALL_LICENSE_FILES=googletest/LICENSE)`.

* If you do not need the **GMock** package, you can use `hunter_config(GTest VERSION <your_version> CMAKE_ARGS BUILD_GMOCK=OFF BUILD_GTEST=ON )` to only build
the **GTest** package.

# Google Test #

[![Build Status](https://travis-ci.org/google/googletest.svg?branch=master)](https://travis-ci.org/google/googletest)
[![Build status](https://ci.appveyor.com/api/projects/status/4o38plt0xbo1ubc8/branch/master?svg=true)](https://ci.appveyor.com/project/GoogleTestAppVeyor/googletest/branch/master)

**Future Plans**:
* 1.8.x Release - the 1.8.x will be the last release that works with pre-C++11 compilers. The 1.8.x will not accept any requests for any new features and any bugfix requests will only be accepted if proven "critical"
* Post 1.8.x - work to improve/cleanup/pay technical debt. When this work is completed there will be a 1.9.x tagged release
* Post 1.9.x googletest will follow [Abseil Live at Head philosophy](https://abseil.io/about/philosophy)


Welcome to **Google Test**, Google's C++ test framework!

This repository is a merger of the formerly separate GoogleTest and
GoogleMock projects. These were so closely related that it makes sense to
maintain and release them together.

Please see the project page above for more information as well as the
mailing list for questions, discussions, and development.  There is
also an IRC channel on [OFTC](https://webchat.oftc.net/) (irc.oftc.net) #gtest available.  Please
join us!

Getting started information for **Google Test** is available in the
[Google Test Primer](googletest/docs/primer.md) documentation.

**Google Mock** is an extension to Google Test for writing and using C++ mock
classes.  See the separate [Google Mock documentation](googlemock/README.md).

More detailed documentation for googletest (including build instructions) are
in its interior [googletest/README.md](googletest/README.md) file.

## Features ##

  * An [xUnit](https://en.wikipedia.org/wiki/XUnit) test framework.
  * Test discovery.
  * A rich set of assertions.
  * User-defined assertions.
  * Death tests.
  * Fatal and non-fatal failures.
  * Value-parameterized tests.
  * Type-parameterized tests.
  * Various options for running the tests.
  * XML test report generation.

## Platforms ##

Google test has been used on a variety of platforms:

  * Linux
  * Mac OS X
  * Windows
  * Cygwin
  * MinGW
  * Windows Mobile
  * Symbian

## Who Is Using Google Test? ##

In addition to many internal projects at Google, Google Test is also used by
the following notable projects:

  * The [Chromium projects](http://www.chromium.org/) (behind the Chrome
    browser and Chrome OS).
  * The [LLVM](http://llvm.org/) compiler.
  * [Protocol Buffers](https://github.com/google/protobuf), Google's data
    interchange format.
  * The [OpenCV](http://opencv.org/) computer vision library.
  * [tiny-dnn](https://github.com/tiny-dnn/tiny-dnn): header only, dependency-free deep learning framework in C++11.

## Related Open Source Projects ##

[GTest Runner](https://github.com/nholthaus/gtest-runner) is a Qt5 based automated test-runner and Graphical User Interface with powerful features for Windows and Linux platforms.

[Google Test UI](https://github.com/ospector/gtest-gbar) is test runner that runs
your test binary, allows you to track its progress via a progress bar, and
displays a list of test failures. Clicking on one shows failure text. Google
Test UI is written in C#.

[GTest TAP Listener](https://github.com/kinow/gtest-tap-listener) is an event
listener for Google Test that implements the
[TAP protocol](https://en.wikipedia.org/wiki/Test_Anything_Protocol) for test
result output. If your test runner understands TAP, you may find it useful.

[gtest-parallel](https://github.com/google/gtest-parallel) is a test runner that
runs tests from your binary in parallel to provide significant speed-up.

[GoogleTest Adapter](https://marketplace.visualstudio.com/items?itemName=DavidSchuldenfrei.gtest-adapter) is a VS Code extension allowing to view Google Tests in a tree view, and run/debug your tests.

## Requirements ##

Google Test is designed to have fairly minimal requirements to build
and use with your projects, but there are some.  Currently, we support
Linux, Windows, Mac OS X, and Cygwin.  We will also make our best
effort to support other platforms (e.g. Solaris, AIX, and z/OS).
However, since core members of the Google Test project have no access
to these platforms, Google Test may have outstanding issues there.  If
you notice any problems on your platform, please notify
[googletestframework@googlegroups.com](https://groups.google.com/forum/#!forum/googletestframework). Patches for fixing them are
even more welcome!

### Linux Requirements ###

These are the base requirements to build and use Google Test from a source
package (as described below):

  * GNU-compatible Make or gmake
  * POSIX-standard shell
  * POSIX(-2) Regular Expressions (regex.h)
  * A C++98-standard-compliant compiler

### Windows Requirements ###

  * Microsoft Visual C++ 2015 or newer

### Cygwin Requirements ###

  * Cygwin v1.5.25-14 or newer

### Mac OS X Requirements ###

  * Mac OS X v10.4 Tiger or newer
  * Xcode Developer Tools

## Contributing change

Please read the [`CONTRIBUTING.md`](CONTRIBUTING.md) for details on
how to contribute to this project.

Happy testing!
