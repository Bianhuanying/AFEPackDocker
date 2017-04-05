FROM ubuntu:14.04

MAINTAINER FSSlc, liuchang011235 AT gmail DOT com

# install essential packages
RUN \
mkdir -p /root/Pkg && \
# use aliyun's mirror for better download speed
sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
apt-get update && \
apt-get install -y make automake autoconf wget build-essential libboost-dev \
mpi-default-dev mpi-default-bin openmpi-bin nano libtbb-dev # libhypre-dev trilinos-all-dev petsc-dev libdeal.ii-dev --no-install-recommends

## set some env varibles
COPY env.txt /root/Pkg/env.txt 
COPY deal.ii_6.3.1.modified.tar.gz /root/Pkg/deal.ii_6.3.1.modified.tar.gz

## prepare deal.II AFEPack easymesh 
RUN \
cd /root/Pkg && \
wget http://dsec.pku.edu.cn/~rli/AFEPack-snapshot.tar.gz && \
wget http://dsec.pku.edu.cn/~rli/source_code/easymesh.c.gz && \
tar -xzf ./AFEPack-snapshot.tar.gz -C /root/Pkg/  && \
tar -xzvf ./deal.ii_6.3.1.modified.tar.gz -C /root/Pkg && \
gunzip easymesh.c.gz && \
## compile dealii v6.3.1
cd /root/Pkg/deal.II && ./configure && make base lac -j8 && \
## compile and install AFEPack
cat /root/Pkg/env.txt >> /root/.bashrc && . /root/.bashrc && \
cd /root/Pkg/AFEPack && \
aclocal && autoconf && automake --add-missing && \
env EXTRA_INCDIR="-I/root/Pkg/deal.II/base/include -I/root/Pkg/deal.II/lac/include" EXTRA_LIBDIR="-L/root/Pkg/deal.II/lib" ./configure && \
# make -j8 && make install
cd ./template/ && make -j8 && cd ../library/ && make -j8 && make install && \
cd ../example/ && make -j8 && \
## compile and install easymesh
cd /root/Pkg/ && \
gcc -o easymesh easymesh.c -lm && \
mv ./easymesh /usr/local/bin/ && \
## do some clean work
rm easymesh.c AFEPack-snapshot.tar.gz env.txt deal.ii_6.3.1.modified.tar.gz && \
rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]