---
layout: post
title: "Building Bio++ and Egglib"
date: 2013-02-06 13:19
comments: true
categories: [python, bioinformatics]
---




# Building Bio++ and egglib from source

It took a little finagling to build the Python module [egglib](http://egglib.sourceforge.net/) for population genetic analyses.  Here's how I did it.


Platform on which this was tested:

- OS X 10.7
- Apple Xcode command line tools
- Many tools install via [Homebrew](http://mxcl.github.com/homebrew/)


### Pre-requisites for Bio++

Install the following via brew:

* cmake -- `brew install cmake`
* doxygen -- `brew install doxygen`

### Building and installing Bio++

##### Create a temporary directory to build the libraries

```
mkdir -p ~/tmp/bpp
```  

##### Download and unzip the libraires

We just need three of the Bio++ libraries:

 1. bpp-core
 2. bpp-seq 
 3. bpp-popgen
 
Use `curl` from the command line to download the libraries:

```
cd ~/tmp/bpp
curl -O http://biopp.univ-montp2.fr/repos/sources/bpp-core-2.0.3.tar.gz
curl -O http://biopp.univ-montp2.fr/repos/sources/bpp-seq-2.0.3.tar.gz
curl -O http://biopp.univ-montp2.fr/repos/sources/bpp-popgen-2.0.3.tar.gz
```

And then unzip each library:

```
tar xvzf bpp-core-2.0.3.tar.gz
... etc ...
```

##### Building individual Bio++ libraries

Build the libraries in the following order:

1. bpp-core

        cd ~/tmp/bpp/bpp-core-2.0.3
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local
        make             # you may get warnings but OK as long as no errors
        make install     # if you didn't get any errors during make

2. bpp-seq

        cd ~/tmp/bpp/bpp-seq-2.0.3
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local
        make            
        make install     

3. bpp-popgen

        cd ~/tmp/bpp/bpp-popgen-2.0.3
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local
        make             
        make install    


### Pre-requisites for egglib

These aren't required but we might as well install them.

First, let's "tap" the Homebrew science packages:

```
brew tap homebrew/science
```

Then install all the following via brew:

* gsl -- `brew install gsl`
* clustalw -- `brew install clustal-w`
* muscle -- `brew install muscle`
* paml -- `brew install paml`
* phyml -- `brew install phyml`
* primer3 -- `brew install primer3`
* phylip -- `brew install phylip`

### Building and installing egglib

Download the following source packages from the [egglib sourceforge site](http://sourceforge.net/projects/egglib/files/current/):

* egglib-cpp-2.1.5.tar.gz
* egglib-py-2.1.5.tar.gz

Move these to a temporary directory (e.g. `~/tmp/egglib`) and unzip them (`tar xvzf ....`).

##### Build the egglib c++ library

```
cd egglib-cpp-2.1.5
./configure
make
make install  # assuming make return with no errors
```


##### Build the egglib Python library

When I tried to build the egglib Python library, `setup.py` wasn't finding the appropriate version of gcc for some reason. Here's how I fixed that (note the addition of the `CC` environment variable before the `setup.py` call):

```
cd egglib-py-2.1.5
CC=/usr/bin/gcc python setup.py build
CC=/usr/bin/gcc python setup.py install  # assuming previous step was error free
```


### Test your installation of egglib

Confirm that egglib import's correctly:

```
$ python
Python 2.7.3 (v2.7.3:70274d53c1dd, Apr  9 2012, 20:52:43) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import egglib
>>> quit()
```