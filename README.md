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
