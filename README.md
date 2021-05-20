# Build Caffe on Compute Canada Servers



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
 4. set `CUDA_DIR := /cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc7.3/cuda/10.0.130`
 5. remove following lines:
    -gencode arch=compute_20,code=sm_20 \
    	-gencode arch=compute_20,code=sm_21 \
    	-gencode arch=compute_30,code=sm_30 \
    	-gencode arch=compute_35,code=sm_35 \

6. Change `BLAS := mkl`
7. For Python, we have two options. First, using python 2.7 and the second is using Python 3.x. There is no difference between them, but in my experience, I would suggest using Python 3.x. I assume you have installed Anaconda. If you did not install Anaconda, no worry, I explain two ways:
 - Using Anaconda:
	 - comment these two lines:
	 - `PYTHON_INCLUDE := /usr/include/python2.7 \
/usr/lib/python2.7/dist-packages/numpy/core/include`
	- set: 
    ANACONDA_HOME := /home/your_username/anaconda3 
	 - set:
    PYTHON_LIBRARIES := boost_python37-mt python3.7m
    PYTHON_INCLUDE := $(ANACONDA_HOME)/include/python3.7m \
/home/your_username/anaconda3/envs/env_name/lib/python3.7/site-packages/numpy/core/include
    - comment: `PYTHON_LIB := /usr/lib`
    - uncomment: `PYTHON_LIB := $(ANACONDA_HOME)/lib`
    - uncomment `WITH_PYTHON_LAYER := 1`
  - Without Anaconda:
	  - The only difference is you should put a correct address to the python files and libs.
