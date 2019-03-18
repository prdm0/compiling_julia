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

Inicialmente faça o clone do projeto [**Julia**](https://julialang.org/) no [**GitHub**](https://github.com/JuliaLang). Dessa forma, com o git instalado e configurado, faça:

```
cd ~/Downloads/ && git clone git://github.com/JuliaLang/julia.git
cd julia
```

Após o download de todos os arquivos do projeto [**Julia**](https://julialang.org/) ter sido clonado para o computador, vá para a versão que deseja compilar, por exemplo a versão **v1.1.0**. Para conhecer as versões, liste todas as tags das versões da linguagem do projeto clonado (`git tag -l`). 

```
git tag -l
git checkout v1.1.0
export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/
make -j 8
```


