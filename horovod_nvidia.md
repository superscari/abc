centos7.8 3.10.0-1127

0、yum install epel-release -y
yum install gcc gcc-c++ bison flex -y
yum install kernel-devel-`uname -r`
yum install pciutils


1、NVIDIA-Linux-x86_64-450.80.02.run

https://www.cnblogs.com/gollong/p/12655424.html
disable the Nouveau kernel driver.


2、cuda10.1

重要：Please make sure that
 -   PATH includes /usr/local/cuda-10.1/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-10.1/lib64, or, add /usr/local/cuda-10.1/lib64 to /etc/ld.so.conf and run ldconfig as root

###To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-10.1/bin
###To uninstall the NVIDIA Driver, run nvidia-uninstall

###Please see CUDA_Installation_Guide_Linux.pdf in /usr/local/cuda-10.1/doc/pdf for detailed information on setting up CUDA.
###Logfile is /var/log/cuda-installer.log

strings /etc/ld.so.cache |grep cuda   验证

##CUDNN7.6.5.32-1 要注册



3、nvidia-smi 验证一下

4、ofed
yum install gtk2 atk cairo gcc-gfortran tcsh lsof tcl tk
提示：
./mlnxofedinstall 安装失败出现内核不匹配，则可以生成新的安装包，解压后重新安装即可：
 ./mlnx_add_kernel_support.sh  -m ../MLNX_OFED_LINUX-5.0-1.0.0.0-rhel7.8-x86_64 --make-tgz --skip-repo
 
 
systemctl enable openibd        ################## 可能并不好用
/etc/init.d/openibd restart     ###################可能服务器每次重启都要运行

5、yum install docker-ce   

链接：https://docs.docker.com/engine/install/centos/  ---安装docker步骤：：：
yum remove docker docker-client docker-client-latest  docker-common docker-latest  docker-latest-logrotate docker-logrotat docker-engine
yum install -y yum-utils
yum-config-manager  --add-repo  https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io
systemctl restart docker && systemctl enable docker


https://www.jianshu.com/p/32ad4f448fe5  ---安装nvidia container：：：
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.repo |  tee /etc/yum.repos.d/nvidia-container-runtime.repo
yum install nvidia-container-runtime -y

systemctl restart docker

验证：：
 docker run -it --rm --gpus all ubuntu nvidia-smi 
 或者
 docker run -it --rm --gpus all ubuntu nvidia-smi -L

6、docker-compose 1.26.2
yum install docker-compose


systemctl stop firewalld
systemctl disable firewalld

7、
docker save horovod_including_data > horovod_including_data.tar
docker load < horovod_including_data.tar

参考   https://github.com/horovod/horovod/blob/master/docs/docker.rst

单机：  horovodrun -np 4 -H localhost:4 python keras_mnist_advanced.py


## Running on multiple machines 
