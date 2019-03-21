# Instructions for compiling Julia language with OpenBLAS (GNU/Linux)

**DEPENDENCES**: git, make, cmake, gcc, gcc-fortran.

**Important**: I'll be at all times assuming that the project [**Julia**](https://julialang.org/) has been cloned into the directory `~/Downloads`. Also, I will consider the `/opt` directory as the installation directory for the [**OpenBLAS**](https://www.openblas.net/) library and of the [**Julia**](https://julialang.org/) language. You can choose a directory of your choice.

## Compiling OpenBLAS

Initially download the [**Julia**](https://julialang.org/) and [**OpenBLAS**](https://www.openblas.net/) (**Open** Optimized **BLAS** Library) source codes in [**OpenBLAS**](https://www.openblas.net/). In the file directory, perform the following steps.


**Note**: 

1. Simply invoking make (or gmake on BSD) will detect the CPU automatically. To set a specific target CPU, use `make TARGET=xxx`, e.g. `make TARGET=HASWELL`. The full target list is in the file **TargetList.txt**.
2. This will make the compilation run faster using all the features of your CPU. To know the number of cores, do: ```nproc```. The default installation directory is `/opt/OpenBLAS`.

```
cd ~/Downloads
tar -zxvf OpenBLAS*
cd OpenBLAs*
make TARGET=HASWELL -j $(nproc)
sudo make install
```
or

```
cd ~/Downloads
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS*
git checkout v0.3.5
make TARGET=HASWELL -j $(nproc)
sudo make install
```

## Cloning the Julia Project

Initially do the [**Julia**](https://julialang.org/)  project clone on [**GitHub**](https://github.com/JuliaLang/julia). That way, with git installed and configured, do:

```
cd ~/Downloads/ && git clone git://github.com/JuliaLang/julia.git
cd julia
```

After downloading all the project files [**Julia**](https://julialang.org/) cloned to the computer, go to the version you want to compile, for example the version **v1.1.0**. To know the versions, list all the tags of the language versions of the cloned project (`git tag -l`).

```
git tag -l
git checkout v1.1.0
```

Subsequently, have the [**OpenBLAS**](https://www.openblas.net/) library in the `/opt/OpenBLAS/lib/` directory be added to the environment variable `LD_LIBRARY_PATH`. In Linux, the `LD_LIBRARY_PATH` environment variable is a set of colon-separated directories where libraries should be searched first, before the default set of directories. This will cause the [**Julia**](https://julialang.org/) compilation  to consider the [**OpenBLAS**](https://www.openblas.net/) library of the `/opt/OpenBLAS/lib/`.


## Creating the `Make.user` file

In the cloned directory, create the `Make.user` file with the following content:

```
USE_SYSTEM_XXX=1
MARCH=native
LDFLAGS=-Wl,-rpath,/opt/OpenBLAS/lib/
OPENBLAS_DYNAMIC_ARCH=0
```
or

```
cd ~/Downloads/julia
echo "USE_SYSTEM_XXX=1
MARCH=native
LDFLAGS=-Wl,-rpath,/opt/OpenBLAS/lib/
OPENBLAS_DYNAMIC_ARCH=0" > Make.user
```

**Note**: Other paths of libraries of interest can be added to the `Make.user` file by doing `LDFLAGS+=-Wl,-rpath,/path/of/library`.

Now, under the cloned directory of [**Julia**](https://julialang.org/), under the version of interest, compile the language doing:

``` 
cd ~/Downloads/julia
make -j $(nproc)
echo "prefix=/opt/julia" >> Make.user
cd /opt && sudo mkdir julia 
cd ~/Downloads/julia && make install
sudo ln -s /opt/julia/bin/julia /usr/local/bin
julia
```

#### Configuring without creating the `Make.user` file:

1. To check the contents of the `LD_LIBRARY_PATH` variable, do `echo $LD_LIBRARY_PATH`.
2. If the directory where the [**OpenBLAS**](https://www.openblas.net/) library was installed is not present, do `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/OpenBLAS/lib/`.
3. You can always start a bash terminal section with `LD_LIBRARY_PATH` containing the library installation path [**OpenBLAS**](https://www.openblas.net/). To do this, do: `echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/OpenBLAS/lib/" >> ~/.bashrc`. This step is not necessary if you performed step 2. You are advised to use step 2 instead than changing the contents of the `~/.bashrc` file.
4. Make `OPENBLAS_DYNAMIC_ARCH=0` and `MARCH=native` directly in the `Make.inc` file.


