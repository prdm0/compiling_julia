# Instructions for compiling Julia language with OpenBLAS (GNU/Linux)

**DEPENDENCES**: git, make, cmake, gcc, gcc-fortran.

**Compiling OpenBLAS**

Initially download the [**Julia**](https://julialang.org/) and [**OpenBLAS**](https://www.openblas.net/) (**Open** Optimized **BLAS** Library) source codes in [**OpenBLAS**](https://www.openblas.net/). In the file directory, perform the following steps.
```
tar -zxvf OpenBLAS*
cd OpenBLAs*
make -j $nproc
sudo make install
```
or

```
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS*
make -j $nproc
sudo make install
```
**Note**: This will make the compilation run faster using all the features of your CPU. To know the number of cores, do: ```nproc```.

Initially do the [**Julia**](https://julialang.org/)  project clone on [**GitHub**](git://github.com/JuliaLang). That way, with git installed and configured, do:

```
cd ~/Downloads/ && git clone git://github.com/JuliaLang/julia.git
cd julia
```

After downloading all the project files [**Julia**] (https://julialang.org/) cloned to the computer, go to the version you want to compile, for example the version **v1.1.0**. To know the versions, list all the tags of the language versions of the cloned project (`git tag -l`).

```
git tag -l
git checkout v1.1.0
```

Subsequently, have the [**OpenBLAS**] (https://www.openblas.net/) library in the `/opt/OpenBLAS/lib/` directory be added to the environment variable `LD_LIBRARY_PATH`. In Linux, the `LD_LIBRARY_PATH` environment variable is a set of colon-separated directories where libraries should be searched first, before the default set of directories. This will cause the [** Julia **] compilation (https://julialang.org/) to consider the [**OpenBLAS**] library (https://www.openblas.net/) of the `/opt/OpenBLAS/lib/ `.

```
echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/OpenBLAS/lib/" >> ~/.bashrc
export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/
```
Now, under the cloned directory of [**Julia**] (https://julialang.org/), under the version of interest, compile the language doing:

```
make -j $nproc
```
