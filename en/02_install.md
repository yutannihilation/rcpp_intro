# Installation

Before using Rcpp, you need to install c/c++ compiler.

##Install C/C++ compiler

- **Windows** : Install [Rtools](https://cran.r-project.org/bin/windows/Rtools/index.html).
- **Mac** : Install Xcode command line tools. Execute command `xcode-select --install` in Terminal.app
- **Linux** : Install gcc and related libraries. For example In Ubuntu Linux `sudo apt-get install r-base-dev`

If you installed the compiler in a location different from the usual, create the following file under the user's home directory. Then set environment variables in the file.

* `.R/Makevars` for Linux, Mac
* `.R/Makevars.win` fir Windows

```
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7
CPLUS_INCLUDE_PATH=/opt/local/include:$CPLUS_INCLUDE_PATH
LD_LIBRARY_PATH=/opt/local/lib:$LD_LIBRARY_PATH
CXXFLAGS= -g0 -O2 -Wall
MAKE=make -j4
```


## Install Rcpp

You can install Rcpp by executing following code.

```r
install.packages("Rcpp")
``
