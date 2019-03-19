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

Subsequently, have the [**OpenBLAS**](https://www.openblas.net/) library in the `/opt/OpenBLAS/lib/` directory be added to the environment variable `LD_LIBRARY_PATH`. In Linux, the `LD_LIBRARY_PATH` environment variable is a set of colon-separated directories where libraries should be searched first, before the default set of directories. This will cause the [**Julia**](https://julialang.org/) compilation  to consider the [**OpenBLAS**](https://www.openblas.net/) library of the `/opt/OpenBLAS/lib/ `.

No diretório colonado, crie o arquivo `Make.user` com o conteúdo abaixo:

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

**Note**: Outros caminhos de bibliotecas de interesse podem ser adicionador ao arquivo `Make.user` fazendo `LDFLAGS+=-Wl,-rpath,/path/of/library`


Now, under the cloned directory of [**Julia**](https://julialang.org/), under the version of interest, compile the language doing:

```
cd ~/Downloads/julia
make -j $nproc
```

#### Configurando sem criar o arquivo `Make.user`:

1. Para verificar o conteúdo da variável `LD_LIBRARY_PATH` faça `echo $LD_LIBRARY_PATH`.
2. Se o diretório de onde foi instalado biblioteca OpenBLAS não encontra-se presente, faça `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/OpenBLAS/lib/`.
3. É possível sempre iniciar uma seção do terminal bash com `LD_LIBRARY_PATH` contendo o caminho de instalação da biblioteca OpenBLAS. Para isso, faça: `echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/OpenBLAS/lib/" >> ~/.bashrc`. Esse passo não é necessário caso tenha realizado o passo 2. Aconselha-se o uso do passo 2 ao invés de alterar o conteúdo do arquivo `~/.bashrc`.
4. Faça `OPENBLAS_DYNAMIC_ARCH=0` e `MARCH=native` diretamente no arquivo `Make.inc`.


