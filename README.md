# Building Caffe on Compute Canada Servers



First, you need to download the latest version of Coffee. Go to your home directory and run the following command:

    git clone https://github.com/BVLC/caffe.git
    
Then

    cd caffe/
You need to create the build directory:

    mkdir build
Ok, let's start defining and modifying the **Makefile.config**. inside the Caffe root directory:

    cp Makefile.config.example Makefile.config
  Now we want to modify the Makefile.config file. Start editing it by 
  

    nano Makefile.config
Inside this file, you should change the following items based on your username on Compute Canada Servers. Therefore, everywhere you see **your_username**, change it.

 1. uncomment `USE_CUDNN := 1 (remove # before it)`
 2. uncomment `OPENCV_VERSION := 3`
 3. uncomment `CUSTOM_CXX := g++`
 4. set `CUDA_DIR := /cvmfs/soft.computecanada.ca/easybuild/software/2017/Core/cudacore/10.1.243`
 5. remove following lines:
    	-gencode arch=compute_20,code=sm_20 \
    	-gencode arch=compute_20,code=sm_21 \


 6. Change `BLAS := mkl`
 7. For Python, we have two options. First, using python 2.7 and the second is using Python 3.x. There is no difference between them, but in my experience, I would suggest using Python 3.x. I assume you have installed Anaconda. If you did not install Anaconda, no worry, I explain two ways:
 8. Without Anaconda:
	 - comment these two lines:
	 `PYTHON_INCLUDE := /usr/include/python2.7 \
		/usr/lib/python2.7/dist-packages/numpy/core/include`
	- set: 
    `PYTHON_LIBRARIES := boost_python35-mt python3.5m`
    `PYTHON_INCLUDE := /cvmfs/soft.computecanada.ca/easybuild/software/2017/Core/python/3.5.4/include/python3.5m/ \
    /home/your_username/.local/lib/python3.5/site-packages`
	 - set:
    `PYTHON_LIB := /cvmfs/soft.computecanada.ca/easybuild/software/2017/Core/python/3.5.4/lib`
    - uncomment `WITH_PYTHON_LAYER := 1`
 9. With Anaconda:
	  - The only difference is you should put a correct address to the python files and libs inside Anaconda directory.
## Before Building Caffe
Before we start building Caffe, we need to build **protobuf** library. Although both beluga and cedars have this library, we need a specific version that is not installed on them. We need _**protobuf 3.0 alpha**_. Follow these steps:

 1. inside your home directory, create a folder. for example, `mkdir local_pkgs`. Go to this folder.
 2. `wget https://github.com/protocolbuffers/protobuf/releases/download/v3.0.0-alpha-1/protobuf-cpp-3.0.0-alpha-1.tar.gz`
 3. `tar -xf protobuf-cpp-3.0.0-alpha-1.tar.gz`
 4. Go into the extracted source directory and execute `./configure --prefix=<your home directory>/local_pkgs`
 5. Execute `make -j<number of parallel threads>`
 6. Execute `make install`
 7. Go to home directory. `nano ~/.bashrc`. Set the PATH environment variable to include `<your home directory>/local_pkgs/bin`. You can do it like this: `export PATH=$PATH:/home/your_username/local_pkgs/bin`
 8. Set the LD_LIBRARY_PATH environment variable to include `<your home directory>/local_pkgs/lib`. Iike the step 7, `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/your_username/local_pkgs/lib`
 9. You need also add anaconda lib path to the LD_LIBRARY_PATH. Similar to the step 8:
 10. `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/your_username/anaconda3/lib`

## Preparing the environment to start building Caffe
In this section, we list the essential libraries you need to include before building Caffe. Be careful about the order of loading libraries and their versions to avoid inconsistency.

 

    module purge
    module load nixpkgs/16.09
    module load gcc/7.3.0
    module load opencv/3.4.3
    module load python/3.5.4
    module load leveldb
    module load hdf5
    module load cuda/10.1
    module load cudnn/7.6.5
    module load boost/1.68.0
    module load cudnn/7.6.5
    pip install --user numpy
    
   ## Building Caffe
Now, it's time to build Caffe. Inside the Caffe's root directory:

    make clean
    make all
    make pycaffe
    make distribute
    make test
    make runtest
If you succeed in all the tests then you've successfully installed Caffe in your system ! One good reason to smile !
    
