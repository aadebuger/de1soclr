

de1soc 测试指南
#quartius 的版本
  pro 版本不支持 ，需要标准版本
#linux 核心
用 terasic 的ubuntu 核心， sw 设置成00000， 能启动，但是发现没有opencl 环境 

#opencl bsp 里面的核心
ssh连不上， 后来还是用mac minicom 连起来，serial hardware flow control 必须设置成no , 
这可能是10-20年再次用串口连接linux 核心了 , 这个linux 版本是 yocto

#run opencl
跑source init_opencl.sh 里面的例子都能跑起来，发现vector_add 1000,000居然7.2ms 比arria10 也就差3倍，感觉不可思议。


#如何测试自己的代码呢
intel fpga opencl hello world 有arm 的例子， 当时费了半天劲用第三方的arm gcc 编译成功了，
scp  复制回来后，
提示 ./hello_world: /usr/lib/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by ./hello_world) 
想了想 ，死了这个心了，文档里是用eds 编译的，我估计arm gcc 版本可能不同吧

#编译linux 版本 hello world
后来想，arm 也能编译，就将intel fpga example 里的linux 64 的例子scp 过来
跑make, 提示INTELSDK 不存在，就将init_opencl.sh 第一行的那个export 配置下
make ok, 有点慢。
跑起来，ERROR: Unable to find Intel(R) FPGA OpenCL platform.
一看这个信息，感觉很熟悉，so 文件肯定没问题， 后来想起来，当初commond 里面的那个代码 是不是有个平台名字检查的问题，
platform = findPlatform("Intel(R) FPGA SDK for OpenCL(TM)");
改成收购前的公司名字。 ok 跑起来

#交叉编译
每次一开始我就特别不喜欢交叉编译，读书的时候，z8000 就是在模拟器跑的，不过，一熟悉，还是很喜欢的。
要能使cmake 工作，才敢改更多的c++ 代码。
#pyopencl 
如果能让pyopencl 跑在 de1soc 确实不错,应该难度不大，
但是在yocto 下，我还是不做了，没任何必要。


#linux 版本
 我喜欢ubuntu版本，但是确实一旦部署，ubuntu 是有点不方便，开发方便
#fpga cnn 
 在看看如何跑cnn 在de1soc 
#armrte 能否跑在ubuntu arm 16.04 
  要测试，可能还要再买个32g ssd

#intel,terasic 的linux 感觉有点乱

#懒得换的环境，要不用grpc 提供opencl 服务给其他机器


