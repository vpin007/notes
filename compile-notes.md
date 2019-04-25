Following this guide.
https://github.com/graft-community/docs/blob/master/Graft%20Network%20Windows%20Compile.md

Exceptions. *********************************************
Do not install mingw-w64-x86_64-boost, it will install Boost 1.70.0 and it will not compile.
Instead pull older version from msys2 archive then manually install.

````bash
    wget http://repo.msys2.org/mingw/x86_64/mingw-w64-x86_64-boost-1.69.0-2-any.pkg.tar.xz
````
Install using 

````bash
    pacman -U mingw-w64-x86_64-boost-1.69.0-2-any.pkg.tar.xz
````

If you already installed newer boost you will have to remove it first.
````bash
pacman -R mingw-w64-x86_64-boost
````
********************************************************


Go to https://www.msys2.org/ and download the x86_64 version of MSYS2.

Run the installer and make sure you install to c:\msys64.


Leave "Run MSYS2 64bit now" checked and click finish.

Type pacman -Syu then Hit Enter

Then type y.
````bash
    pacman -Syu
````


Close the terminal window by clicking the X.  Might take a little while to finally close.

Now launch the "MSYS2 MingGW 64-bit" shortcut.

Type pacman -Syu then Hit Enter.

Then type y.

````bash
    pacman -Syu
````

After it finishes, close it and relaunch the "MSYS2 MingGW 64-bit" shortcut.

I'll paste in the command "pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake" then hit Enter.
Then type y.

````bash
    pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake
````

I'm going to manually get a specific version of boost.

````bash
    wget http://repo.msys2.org/mingw/x86_64/mingw-w64-x86_64-boost-1.69.0-2-any.pkg.tar.xz
````

Then install it with "pacman -U mingw-w64-x86_64-boost-1.69.0-2-any.pkg.tar.xz"

````bash
    pacman -U mingw-w64-x86_64-boost-1.69.0-2-any.pkg.tar.xz
````

I'll paste in the command "pacman -S base-devel" then hit Enter when asked, then y.

````bash
    pacman -S base-devel
````
 Next "pacman -S mingw-w64-x86_64-unbound", then y.
````bash
    pacman -S mingw-w64-x86_64-unbound
````

Next install git using "pacman -S git", then y.

````bash
    pacman -S git
````
Now we will clone the graft-network repo using "git clone --recurse-submodules https://github.com/graft-project/GraftNetwork.git".


````bash
    git clone --recurse-submodules https://github.com/graft-project/GraftNetwork.git
````
Then we change to the GraftNetwork directory with "cd GraftNetwork"
````bash
    cd GraftNetwork
````

Then we start the build with "make release-static-win64".
````bash
make release-static-win64
````
After compiling the executables will be found in C:/msys64/GraftNetwork/build/release/bin.
