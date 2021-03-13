# build with [Bazel](https://docs.bazel.build/versions/master/bazel-overview.html)
## what is Bazel?
What is Bazel?
Bazel is an open-source build and test tool similar to Make, Maven, and Gradle. It uses a human-readable, high-level build language. Bazel supports projects in multiple languages and builds outputs for multiple platforms. Bazel supports large codebases across multiple repositories, and large numbers of users.
## how to install Bazel?
* 安装编译bazel的依赖工具

  ```makefile
  sudo apt-get install build-essential openjdk-8-jdk python zip unzip
  ```
  
* 下载安装包：

  ```makefile
  wget https://github.com/bazelbuild/bazel/releases/download/3.2.0/bazel-3.2.0-dist.zip --no-check-certificate
  ```
  
* 解压

   ```makefile
   unzip  bazel-3.2.0-dist.zip -d bazel-3.2.0-dist
   cd bazel-3.2.0-dist
   ```

* 编译&安装

  ```makefile
  env EXTRA_BAZEL_ARGS="--host_javabase=@local_jdk//:jdk" bash ./compile.sh
  sudo cp output/bazel /usr/local/bin/
  ```
  
  * 如果联网需要代理，需要使用cntlm配置代理，
e1x1p1o1r1t h1t1t1p1_proxy=http1:1//127.0.0.1:xxxx
  
  ## build with Bazel
  以[tensorflow lite](https://github.com/tensorflow/tensorflow.git)为例，clone 工程到本地目录。
  配置工程目录下的configure文件，'./configure'。根据自己的硬件配置，开关相应的选项。
  [操作引导]（https://github.com/tensorflow/tensorflow/tree/master/tensorflow/lite/tools/benchmark）
  ```
  bazel build -c opt \
  --config=android_arm64 \
  tensorflow/lite/tools/benchmark:benchmark_model
  ```
  *如果出现网站鉴权问题，可以配置bazel如下*
  ```
  alias bazel="bazel --host_jvm_args=-Djavax.net.ssl.trustStore='/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts' --host_jvm_args=-Djavax.net.ssl.trustStorePassword='changeit'"
  ```
  同时将对应需要鉴权的网站加入到cacerts文件，相关操作可以[参考](https://blog.csdn.net/wangjunjun2008/article/details/37662851)
  
  ## 如何使用Bazel编译[c++,java,android](https://docs.bazel.build/versions/master/bazel-overview.html#how-do-i-get-started)  
  
