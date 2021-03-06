---
layout: posts
title: "Calling Fortran code from C++ on Visual Studio 2015"
author_profile: true
---

*Author Note: This post was originally hosted on my previous webpage. Some information are updated in December 2020*

In case you *have to* use some Fortran libraries on Windows, there are a few *free* options:

1. Bash on Ubuntu on Windows (a.k.a. WSL Windows Subsystem for Linux)
2. MING-W64 + Visual Studio
3. Cygwin environment
4. PGI Fortran Compiler in their free community version (now Nvidia)
5. As of Dec 2020, it seems that Intel's compilers are now free (?).

This post is about option 2. However, option 1 is obviously the most portable way..

Anyway, here I describe how to call Fortran code from C++ using Visual Studio. I will be focusing on MinGW gfortran as that is a popular free Fortran compiler on Windows platform.

System Requirements:
- Visual Studio 2015 (C++) (e.g., free community version)
- MinGW64 gfortran (64bit, using the one from Msys2 here. Make sure you are using the one from `mingw64` not `msys`)


My Fortran code is just a simple function in a module (`testfor.f90`):
```fortran
module testfor
    use, intrinsic :: iso_c_binding
    implicit none
contains
    subroutine testingfor(x) bind(c, name="testingfor")
        implicit none
        double precision, intent(in) :: x
 
        print *, "# calling testfor in Fortran!!"
        print *, "x = ", x 
    end subroutine
end module testfor
```

For simplicity I put this file in the same folder as my C++ codes. Note that I am using Fortran 2003 standard **iso_c_binding**, which is a standardized way for C/Fortran interface. The `bind` syntax allows you to customize the function name to be called in C++. I used the same name `testingfor` in the C++ function below. If you are using the old (pre-2003) style, the naming convention for a function in a module may be a little complicated. The new standard helps clean up the code a little.

Next, my C++ main function is very simple (`main.cpp`):
```c
extern "C" {
    void testingfor(double* x);
}
 
int main(int argc, char* argv[]) {
    double y = 1.0;
 
    testingfor(&y);
}
```

The `extern C` syntax acts as an interface. Note that the input argument for the Fortran function must be a **pointer** as Fortran is *pass-by-reference* only.

The main dish: In Windows, we need an "import" library (`.lib`) and a dynamic library (`.dll`). The former is used in the compile-time and the latter is for run-time. To generate these files, we need to the following steps (in the Windows' `command prompt`):

```bash
gfortran -c testfor.f90
 
dllwrap.exe --export-all-symbols testfor.o -lgfortran --output-def testfor.def -o testfor.dll

lib.exe /machine:x64 /def:testfor.def
```

Above commands work in a command prompt if you have set the *path* correctly. In short, `gfortran` and `dllwrap` are the ones in `msys2`, and `lib.exe` is a Windows command. The first command is simply to compile our Fortran module into an **object** file (`testfor.o`). Second command creates a definition file (`.def`) and a dll file (`.dll`). The `dllwrap.exe` comes with MinGW as for as I know. We may ignore the following error:
```
dllwrap.exe: no export definition file provided.
Creating one, but that may not be what you want
```
The last line with `lib.exe` in the previous code block is to create the import *lib* file with the *def* file. Now we have what we want:  `testfor.lib` and `testfor.dll`.

The final step is to set up the Visual Studio to include the Fortran library files. I don’t go over the details here. In short, we need to add `testfor.lib` to the resource folder and let VS know you need it to compile. This can be done by adding `testfor.lib` into `Project-> Properties-> Configuration Properties-> Linker-> Input-> Additional Dependencies`. Note that the resultant `exe` file does require the dynamic library `testfor.dll` in the `PATH` in order to run. Here is the result:

![Command Prompt]({{ site.url }}{{ site.baseurl }}/assets/images/fortranwindows.png){: .align-center}

Trouble-shooting

- In some cases (pointed by some readers), there are also some DLL files from msys2 that need to be included. They can be found by running `ldd` against some executable generated by gfortran in `msys2` shell. For examples,
  -  libgfortran-5.dll, libquadmath-0.dll, libgcc_s_seh-1.dll, libwinpthread-1.dll


Hopefully this is useful.
Reference: [http://www.cs.cmu.edu/~barbic/arpack.html](http://www.cs.cmu.edu/~barbic/arpack.html)