FROM ubuntu:16.04

MAINTAINER FSSlc, liuchang011235 AT gmail DOT com

# install essential packages
RUN \
  mkdir -p /root/Pkg && \
  # use aliyun's mirror for better download speed
  sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get install -y make automake autoconf wget build-essential \
  libdeal.ii-dev mpi-default-dev nano # libhypre-dev trilinos-all-dev petsc-dev --no-install-recommends

## set some env varibles
COPY env.txt /root/Pkg/env.txt 

## prepare AFEPack easymesh
RUN \
cd /root/Pkg && \
wget http://dsec.pku.edu.cn/~rli/AFEPack-snapshot.tar.gz && \
wget http://dsec.pku.edu.cn/~rli/source_code/easymesh.c.gz && \
tar -xzf ./AFEPack-snapshot.tar.gz -C /root/Pkg/  && \
gunzip easymesh.c.gz && \
## compile and install AFEPack
cat /root/Pkg/env.txt >> /root/.bashrc && . /root/.bashrc && \
cd /root/Pkg/AFEPack && \
ln -s /usr/lib/x86_64-linux-gnu/libdeal.ii.g.so.8.1.0 /usr/lib/x86_64-linux-gnu/libdeal_II.g.so && \
ln -s /usr/lib/x86_64-linux-gnu/libdeal.ii.so.8.1.0  /usr/lib/x86_64-linux-gnu/libdeal_II.so && \ 
aclocal && autoconf && automake --add-missing && \
env EXTRA_INCDIR="-I/usr/include/deal.II/" EXTRA_LIBDIR="-L/usr/lib/x86_64-linux-gnu/" ./configure && \
# make -j8 && make install
cd ./template/ && make -j8 && cd ../library/ && make -j8 && make install && \
cd ../example/ && make -j8 && \
## compile and install easymesh
cd /root/Pkg/ && \
gcc -o easymesh easymesh.c -lm && \
mv ./easymesh /usr/local/bin/ && \
## do some clean work
rm easymesh.c AFEPack-snapshot.tar.gz env.txt && \
rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]