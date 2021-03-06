# Instructions for compiling Julia language with OpenBLAS (GNU/Linux)

**IMPORTANT**: Be sure to remove all versions of [**BLAS**](http://www.netlib.org/blas/), [**LAPACK**](http://www.netlib.org/lapack/) and [**OpenBLAS**](https://www.openblas.net/) installed in `/usr/lib` and `/usr/lib64`.

### DEPENDENCES: 

* git
* make
* cmake
* gcc
* gcc-fortran
* patch 

**Important**: I'll be at all times assuming that the project [**Julia**](https://julialang.org/) has been cloned into the directory `$HOME/Downloads`. Also, I will consider the `/opt` directory as the installation directory for the [**OpenBLAS**](https://www.openblas.net/) library and of the [**Julia**](https://julialang.org/) language. You can choose a directory of your choice.

## Compiling OpenBLAS

Initially download the [**Julia**](https://julialang.org/) and [**OpenBLAS**](https://www.openblas.net/) (**Open** Optimized **BLAS** Library) source codes in [**OpenBLAS**](https://www.openblas.net/). In the file directory, perform the following steps.


**Note**: 

1. Simply invoking make (or gmake on BSD) will detect the CPU automatically. To set a specific target CPU, use `make TARGET=xxx`, e.g. `make TARGET=HASWELL`. The full target list is in the file **TargetList.txt**.
2. This will make the compilation run faster using all the features of your CPU. To know the number of cores, do: ```nproc```. The default installation directory is `/opt/OpenBLAS`.

```
cd $HOME/Downloads
tar -zxvf OpenBLAS*
cd OpenBLAs*
make -j $(nproc) 
sudo make install PREFIX=/opt/OpenBLAS
```
or

```
cd $HOME/Downloads
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS*
git checkout v0.3.5
make -j $(nproc) 
sudo make install PREFIX=/opt/OpenBLAS
```

### Modifying the directory /opt/OpenBLAS (Soft Links)

In order for the [**Julia**](https://julialang.org/) language compilation  to proceed correctly with the link to the [**OpenBLAS**](https://www.openblas.net/)library installed in the `/opt/` directory, we have to create some [**soft links**](https://en.wikipedia.org/wiki/Symbolic_link). The `/opt/OpenBLAS/lib` directory and symbolic links should be of the following form:

![soft_link_julia](https://raw.githubusercontent.com/prdm0/tempfiles/master/soft_link_julia.png)

Some of the soft links had already been created with the library installation [**OpenBLAS**](https://www.openblas.net/). To create the remaining symbolic links, do the following:

```
cd /opt/OpenBLAS/lib
sudo ln -sf libopenblas_haswellp-r0.3.5.so libblas.so
sudo ln -sf libopenblas_haswellp-r0.3.5.so libcblas.so
sudo ln -sf libopenblas_haswellp-r0.3.5.so liblapack.so
sudo cp -a lib* /usr/lib64
sudo cp -a lib* /usr/lib
```

**Note**: 

1 - Make sure that there is no installed version of blas, lapack and OpenBLAS in `/usr`. The command `cp -a lib *` will copy the compiled OpenBLAS library files along with the symbolic links created so they can be used throughout the system.

2 - Note that the **libopenblas_haswellp-r0.3.5.so** file may have a different name on your machine because of the version of [**OpenBLAS**](https://www.openblas.net/) and computer architecture. Usually it has a name in the form **libopenblas_xxx**. If this is the case, make the necessary change in file name.



## Cloning the Julia Project

Initially do the [**Julia**](https://julialang.org/) project clone on [**GitHub**](https://github.com/JuliaLang/julia). That way, with git installed and configured, do:

```
cd $HOME/Downloads && git clone git://github.com/JuliaLang/julia.git
cd julia
```

After downloading all the project files [**Julia**](https://julialang.org/) cloned to the computer, go to the version you want to compile, for example the version **v1.1.0**. To know the versions, list all the tags of the language versions of the cloned project (`git tag -l`).

```
git checkout v1.1.0
```

## Creating the `Make.user` file

Subsequently, have the [**OpenBLAS**](https://www.openblas.net/) library in the `/opt/OpenBLAS/lib/` directory be added to the environment variable `LD_LIBRARY_PATH`. In Linux, the `LD_LIBRARY_PATH` environment variable is a set of colon-separated directories where libraries should be searched first, before the default set of directories. This will cause the [**Julia**](https://julialang.org/) compilation  to consider the [**OpenBLAS**](https://www.openblas.net/) library of the `/opt/OpenBLAS/lib/`. In the cloned directory, create the `Make.user` file with the following content:

```
cd $HOME/Downloads/julia
USE_SYSTEM_XXX=1
MARCH=native
LDFLAGS=-Wl,-rpath,/usr/lib64
LDFLAGS+=-Wl,-rpath,/usr/lib
LDFLAGS+=-Wl,-rpath,/opt/OpenBLAS/lib
OPENBLAS_DYNAMIC_ARCH=0
USE_SYSTEM_BLAS=1
USE_SYSTEM_LAPACK=1
```
or

```
cd $HOME/Downloads/julia
echo "USE_SYSTEM_XXX=1
MARCH=native
LDFLAGS=-Wl,-rpath,/usr/lib64
LDFLAGS+=-Wl,-rpath,/usr/lib
LDFLAGS+=-Wl,-rpath,/opt/OpenBLAS/lib
OPENBLAS_DYNAMIC_ARCH=0
USE_SYSTEM_BLAS=1
USE_SYSTEM_LAPACK=1" > Make.user
```

**Note**: Other paths of libraries of interest can be added to the `Make.user` file by doing `LDFLAGS+=-Wl,-rpath,/path/of/library`.

Now, under the cloned directory of [**Julia**](https://julialang.org/), under the version of interest, compile the language doing:

``` 
cd $HOME/Downloads/julia
make -j $(nproc)

echo "prefix=/opt/julia" >> Make.user
cd /opt 
sudo mkdir julia 
cd $HOME/Downloads/julia
sudo make install
sudo ln -sf /opt/julia/bin/julia /usr/bin
julia
```

**Note**: In my tests, the procedure of compiling [**OpenBLAS**](https://www.openblas.net/) separately and later compiling the [**Julia**](https://julialang.org/) language provided greater computational efficiency.

## Atom Editor/IDE for Julia language

To program in Julia I recommend using the Atom programming editor. Atom, through the **uber-juno** plugin, can become [**Juno**](http://junolab.org/), a programming IDE for the Julia language. To use Juno, install the [**Atom**](https://atom.io/) and later, in an Atom section, simultaneously press `Ctrl +,`. This will cause a settings tab to open. Then click in **+Install** and type in the **uber-juno** search and install it.

**Note**: Since there is a soft link to [**Julia**](https://julialang.org/) in **/usr/bin**, the atom will identify with ease, thus ensuring that we will use the version of Julia that was compiled following the previous steps.

Some advantages of Atom / Juno as IDE for programming in Julia language are:

1. It is Open Source;
2. A large community developing tools and improvements;
3. It was created by the GitHub community, thus having an easy integration with the git version system;
4. There are many easy-to-install themes;
5. There are many useful packages for programmers.

<p align="center">
<img src="https://raw.githubusercontent.com/prdm0/tempfiles/master/juno.png" height="400" width="800">
</br>
Juno IDE for programming in Julia language              
</p>

In addition to the **uber-juno** pugin, other useful pugins may be installed. Such plugins are listed below:

1. "**Ask Stack**": Plugin to search Stack Overflow. Make `Ctrl + Alt + a`;
2. "**Git-Plus**": Features function for git usage;
3. "**File-Icons**": Add icons in the files in the tree view;
