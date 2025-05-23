---
title: XMDS
---

# Installing XMDS locally

This section will walk you through building XMDS as user. This guide has been adapted from [the documentation](http://www.xmds.org/installation.html).

## Loading and installing prerequisites
1.  **FFTW, MPI, MLK** and a **C++ compiler**. To load these, do 
	``` bash
	module load libs/fftw
	module load libs/mkl
	```
	Loading FFTW will load MPI and a compiler as prerequisites.

2. **Python 3.** We also require Python 3:
	```
	module load apps/python
	```
	Make sure the following packages are installed: **numpy, setuptools, lxml, h5py, pyparsing, cheetah3**. 	If any are missing, install using:
	```bash
	pip3 install <package> --user
	```
	The `--user` option is required as some packages will attempt to install system wide which will fail due to lack of permissions.

4. **HDF5.** This is another c library, however it may not be pre-installed (available in the module list). If that is thew case, we need to install it from source.  Download the [HDF5 source code](https://www.hdfgroup.org/downloads/hdf5/source-code/) choosing the `tar.gz` file from the list, then move this file to your home directory using something like `scp`. I suggest you make a directory called `~/software` or something if you don't have one already. Move into the directory containing the tar ball and extract it using 
	```bash
	tar -xf hdf5-X.X.X.tar.gz
	```
	with the Xs replaced with the version number you are installing. 

	Now we need to compile HDF5. Move into the extracted directory. Then run 
    ```
    ./configure --prefix=$HOME/.local
    ```
    This may take a few seconds. If you install your binaries, libraries, and header files in some other directory then change `--prefix` appropriately. If you don't know what   these means, then just use tha above.  Then do:
    ```
    make
    ```
    which may take a while, so go make a cup of tea or something similar. Once this completes, run 
    ```
    make install
    ```
    to install HDF5.

This should be all the prerequisites required for installing XMDS.
