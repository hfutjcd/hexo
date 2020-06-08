# build with Bazel
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
  
  * 如果联网需要代理，需要使用cntlm配置代理，export http_proxy=http://127.0.0.1:xxxx
