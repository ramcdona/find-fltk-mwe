# Build and Installation Instructions for find_fltk_mwe


### INTRODUCTION

  This is a somewhat overcomplicated MWE based on FLTK's test/tabs.fl
  being built with the SuperProject pattern based on CMake's
  ExternalProject_add capability.

  ExternalProject allows you to embed the download/patch/configure/build/install/etc.
  process for a dependency in a CMake project.  The external project
  can use a build process (not just CMake) and / or a compiler that differ
  from the host project.

  The SuperProject pattern structures a project into two sub-projects,
  the Libraries and the Main project.  There is a third project
  (the SuperProject) that bundles them together.

  If you are a casual user of a project, who just wants to compile a
  particular version for a particular computer, you probably want to use
  the SuperProject.

  If you are a developer of the project, you probably want to manually
  use the separate Library and Main projects.  It gives you more control
  and ability to drill down.  Also, it will more easily allow you to
  assume the Libraries do not change every time you compile.

  Although this MWE only has one dependency (FLTK), this pattern is usually
  used by projects with many dependencies.  My project builds 18 dependencies
  this way.

  You can set a variable to choose to skip the embedded dependency and instead
  rely on a system provided version.

    - `USE_SYSTEM_FLTK`

  Instead of a system provided version, you can also use this option to point
  your project build at a locally installed version of a given dependency.
  This is perfect for situations where you need to develop on your project
  and a dependency at the same time.

  The Library project communicates to the Main project through the `LIBRARY_PATH`
  variable.  The SuperProject communicates this automatically.  `LIBRARY_PATH`
  contains the path to a `Libraries_Config.cmake` file that contains install
  paths for all the libraries.

  The install paths for each of the libraries is stored in a `XXX_INSTALL_DIR`
  variable.  Override this variable if you want to point at a custom location
  for a given library.

    - `FLTK_INSTALL_DIR`

### Building separate projects (MacOS)

```
mkdir find_fltk_mwe
cd find_fltk_mwe
git clone find_fltk_mwe
mkdir build
cd build
mkdir Libraries
mkdir find_fltk_mwe
cd Libraries
cmake ../../find_fltk_mwe/Libraries
make
cd ..
cd find_fltk_mwe
cmake -DLIBRARY_PATH=../Libraries ../../find_fltk_mwe
make
```

### Building SuperProject (MacOS)

```
mkdir find_fltk_mwe
cd find_fltk_mwe
git clone find_fltk_mwe
mkdir build
cd build
cmake ../../find_fltk_mwe/SuperProject
make
```
