# Instructions for compiling Julia language with OpenBLAS (GNU/Linux)

**DEPENDENCES**: git, make, cmake, gcc, gcc-fortran.

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
sudo ln -sf /opt/julia/bin/julia /usr/local/bin
julia
```

## Arch Linux distribution and derived Linux distributions

For users of the [**Arch Linux**](https://www.archlinux.org/) distribution or derived distributions, if you do not want to compile the [**Julia**](https://julialang.org/) language do:

```
yaourt -S openblas-lapack --noconfirm
sudo pacman -S julia
```

