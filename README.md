# Instructions for compiling Julia language with OpenBLAS (GNU/Linux)

**DEPENDENCES**: make, cmake, gcc, gcc-fortran.

**Compiling OpenBLAS**

Initially download the [**Julia**](https://julialang.org/) and [**OpenBLAS**](https://www.openblas.net/) (**Open** Optimized **BLAS** Library) source codes in [**OpenBLAS**](https://www.openblas.net/). In the file directory, perform the following steps.
```
tar -zxvf OpenBLAS*
cd OpenBLAs*
make -j $nproc
sudo make install
export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/
```
or

```
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS*
make -j $nproc
sudo make install
export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/

```
**Note**: This will make the compilation run faster using all the features of your CPU. To know the number of cores, do: ```nproc```.

