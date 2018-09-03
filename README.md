# Caffe-Installation-Guide
Caffe Installation Guide
'''
CAFFE WITH OPENCV BUILD INSTALLATION GUIDE :


Prerequisites
Run the following commands :
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

sudo apt-get install doxygen (Optional)


# Assuming Opencv3 already built and located at /usr/local/opencv3 

cd  to/dependency/directory/
git clone http://github.com/BVLC/caffe.git
cd caffe
cp Makefile.config.example Makefile.config

Edit makefile.config in gedit (or any editor of choice)

Uncomment (remove #)
CPU_ONLY := 1
BLAS := atlas
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/local/opencv3/include /usr/local/opencv3/include/opencv /usr/include/hdf5 /usr/include/hdf5/serial  (line number 96)
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/local/opencv3/lib /usr/lib/x86_64-linux-gnu/hdf5/serial
(optional) Pkg_config:=1

BUILDING PROTOBUF FROM SOURCE :
cd to/dependency/directory/
wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
tar zxvf protobuf-2.5.0.tar.gz
cd protobuf-2.5.0
wget https://github.com/google/shipshape/blob/master/third_party/proto/src/google/protobuf/stubs/atomicops_internals_generic_gcc.h 
mv atomicops_internals_generic_gcc.h src/google/protobuf/stubs/ 
vim src/google/protobuf/stubs/atomicops_internals_generic_gcc {edit navigate to line 184, add the following lines: }
#elif defined(GOOGLE_PROTOBUF_ARCH_S390)
#include <google/protobuf/stubs/atomicops_internals_generic_gcc.h>
Edit file protobuf-2.5.0/src/google/protobuf/stubs/platform_macros.h, navigate to line 59, add the following lines:
#elif defined(__s390x__)
#define GOOGLE_PROTOBUF_ARCH_S390 1
#define GOOGLE_PROTOBUF_ARCH_64_BIT 1
./configure
Make -j4
make check
sudo make install
Sudo ldconfig OR export LD_LIBRARY_PATH=/usr/local/lib
protoc --version
Note: Protobuf should report version libprotoc 2.5.0

If not protoc 2.5.0

Then go to src/directory of protobuf 3.
Look here : https://stackoverflow.com/questions/35896335/how-can-i-uninstall-protobuf-3-0-0
The remove command does not work, because the instructions I followed on protocol buffer page uses make to build the tool - you only use remove when installing with apt-get
Go to source/directory of protobuf 3 and run sudo make uninstall
If still protoc version = 3.x.x
rm `which protoc`
apt-get install protobuf-compiler
Go to src/directory/protobuf 2.5
Make -j4
make check
sudo make install
Sudo ldconfig OR export LD_LIBRARY_PATH=/usr/local/lib
protoc --version
IT should be 2.5.x

Continue caffe installation :
make clean
make all -j4
make all
make test
make runtest
**Important : If you do not have include/caffe/proto in your caffe installation directory then do the following: Go to caffe installation directory
protoc src/caffe/proto/caffe.proto --cpp_out=.
mkdir include/caffe/proto
mv src/caffe/proto/caffe.pb.h include/caffe/proto

'''

