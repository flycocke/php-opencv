# PHP-OPENCV - PHP extension for Opencv

本插件是：opencv的php扩展

## 项目地址
- [原项目地址](https://github.com/hihozhou/php-opencv)
- [本项目地址](https://github.com/flycocke/php-opencv)

## 对应的版本关系

- OpenCV 3.4.5
- Php-opencv 3.3.0
- PHP7.0

注意：php7.0的，php-opencv 用 3.3.0，opencv 3.4.5。至少我测试是这样

- OpenCV 4.0.0+
- Php-opencv 最新版本，不要3.3.0
- PHP7.1+


## 新增功能说明
### 为了方便获取视频的时长和宽高，特别增加以下方法。以后增加方法也会注明在这里

```
$capture = new VideoCapture($videoPath);//创建视频对象
$rate = $capture->getFrameRate(); //帧率. 
$fraNum = $capture->getFramesNum(); //视频文件的帧数.
$width = $capture->getWidth(); //视频文件宽度.
$height = $capture->getHeight(); //视频文件的高度.
```

### 对应修改了原作者的文件也说明下

- /source/opencv2/opencv_videoio.cc 

实在不会php7内核代码，随便乱写的，只要实现功能了我就没处理了。

## 安装
本安装说明都是依我的安装经验回忆做的记录，有什么错误，请网友指正。
本安装说明分两个部分，一个macos catalina（我的是10.15.3），一个centos7（目前7.2到7.5都是成功的，如果不成功，看提示，后面我会提部分错误解决方法）。
在国内，很多资源纯粹用wget是下载不下来了，我搞了好久（墙）才搞定部分的缺失资源。

## mac 的opencv安装

```
brew install opencv
```
你也可以，看 哪个命令用得上
```
brew install opencv4
```
以上的安装如果能通过，至少你省略了安装ffmpeg等插件过程，毕竟有依赖嘛。如果这个命令提示资源不存在，可以尝试换个源，例如教育网的源。
源的安装就不在这里叙述，需要看的点击这里。
- [替换源](brew.md)

### mac 安装php-opencv

你可以下载原作者的源
```
git clone https://github.com/hihozhou/php-opencv.git
```

也可以下载我这里的，区别我这里的是增加了获取video信息的方法，见上面的【新增功能说明】。

然后使用以下命令
```
cd php-opencv

phpize

./configure --with-php-config=/usr/local/Cellar/php@7.2/7.2.19/bin/php-config --enable-debug

make CXXFLAGS='-std=c++11'   //这个很重要，Mac很可能不支持c++11
```

解析下第三句命令
/usr/local/Cellar/php@7.2/7.2.19/bin/php-config
这个请自行替换你本机的php的php-config所在目录。实在不知道在哪里，可以使用以下命令查找下
```
find / -name php-config 
```
另外如果
make CXXFLAGS='-std=c++11'
命令出现问题，你可以直接使用以下命令
```
make && make install
```

如果编译成功，恭喜你，革命成功啦。
找到目录
php-opencv/modules
把编译好的 opencv.so 放入
/usr/local/Cellar/php72-opencv/opencv.so
以上目录也是没有的话，你可以自行创建，php72-opencv 这个目录名称也是我自己命名的，方便记忆而已。
找到目录
/usr/local/etc/php/
如果你的机器有多个版本的php，这里会有7.1,7.2等目录。再进去里面目录。找到conf.d目录。
在这个目录里面创建ext-opencv.ini空白文件
添加以下内容，
```
[opencv]
extension=/usr/local/Cellar/php72-opencv/opencv.so
```
/usr/local/Cellar/php72-opencv/opencv.so 记得是刚才你自己创建的目录和复制opencv.so的地址。
重启php，我用的nginx+php-fpm，所以直接使用
```
killall php-fpm 
```
搞定!!!


## centos7 的opencv安装

注意 centos7 需要epel来安装部分的插件，没有的安装的话请安装。

```
sudo yum -y install epel-release
```

开始拉取opencv源代码

```
git clone https://github.com/opencv/opencv_contrib.git
cd opencv_contrib
git checkout 3.4.5  //注意php7.0才需要checkout3.4.5
cd ..

git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 3.4.5  //注意php7.0才需要checkout3.4.5
```

### 安装重要插件

很重要，实在是 opencv 太多依赖了，我也不懂..一股脑安装就完事，你能看明白的认为不需要安装的请自行筛选。

```
sudo yum -y ffmpeg ffmpeg-devel --downloadonly --downloaddir=.
sudo yum -y install git gcc gcc-c++ cmake3 cmake-gui
sudo yum -y install qt5-qtbase-devel
sudo yum install -y python34 python34-devel python34-pip
sudo yum install -y python python-devel python-pip
sudo yum -y install python-devel numpy python34-numpy
sudo yum -y install gtk2-devel
sudo yum -y install vlc 
sudo yum install -y libpng-devel
sudo yum install -y jasper-devel
sudo yum install -y openexr-devel
sudo yum install -y libwebp-devel
sudo yum -y install libjpeg-turbo-devel
sudo yum install -y freeglut-devel mesa-libGL mesa-libGL-devel
sudo yum -y install libtiff-devel
sudo yum -y install libdc1394-devel
sudo yum -y install tbb-devel eigen3-devel
sudo yum -y install boost boost-thread boost-devel
sudo yum -y install libv4l-devel
sudo yum -y install gstreamer-plugins-base-devel
sudo yum -y install autoconf automake mercurial pkgconfig zlib-devel libtool freetype-devel make
sudo yum install -y hdf5-devel
sudo yum install -y liblas-devel atlas-devel 
sudo yum install -y gcc-gfortran
sudo yum install -y libevent-devel lua-devel openssl-devel flex mysql-devel
sudo yum install -y xz gettext-devel
sudo yum install -y tcl
sudo yum install -y openblas-devel
sudo yum install -y tesseract-devel tesseract-osd
sudo yum install -y java-1.7.0-openjdk-devel
sudo yum install -y pylint
sudo yum install -y python-flake8
sudo yum install -y vtk-devel vtk-python vtk-qt vtk
sudo yum install -y ccache
sudo yum install -y gflags gflags-devel
sudo yum install -y glog glog-devel
sudo yum install -y libpng libpng-devel
sudo yum install -y libXaw-devel freeimage freeimage-devel zziplib-devel cppunit-devel libXt-devel libX11-devel
sudo yum install -y re2c libgnomeui-devel
sudo yum install -y gcc gcc-c++ gtk2-devel gimp-devel gimp-devel-tools gimp-help-browser zlib-devel libtiff-devel libjpeg-devel libpng-devel gstreamer-devel libavc1394-devel libraw1394-devel libdc1394-devel jasper-devel jasper-utils swig python libtool nasm
sudo yum install -y gcc g++ cmake git  python-devel numpy  gtk2 libdc1394 libv4l gstreamer* nasm libtool swig jasper libdc1394-devel jasper-devel jasper-utils libraw1394-devel libgphoto2 tesseract libavc1394-devel gstreamer-devel libpng-devel libjpeg-devel libtiff-devel zlib-devel gimp gimp-devel gtk+-devel yasm libpciaccess libva-freeworld libva-intel-driver phonon-backend-gstreamer
```

以下命令你可以不运行，主要是 为了上面的安装 部分显示资源不存在的问题

```
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7 #提示缺少code时运行
sudo rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
sudo rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
```

### 不是重要插件的安装，但你的自行判断缺失什么，可以在这里查找补齐。

怎么判断缺失或者必须要安装呢，在编译opencv的过程中，例如显示
check tesseract ... no find 
或
check tesseract ... no
即表示这个依赖你本机器没有。

#### 安装 tesseract（可以不安装）
```
sudo yum -y install tesseract
//或者用编译安装
wget http://www.leptonica.org/source/leptonica-1.77.0.tar.gz
tar xzvf leptonica-1.77.0.tar.gz      
cd leptonica-1.77      
./configure
make
sudo make install
```

#### 安装OpenBLAS（可以不安装）
```
wget http://github.com/xianyi/OpenBLAS/archive/v0.2.20.tar.gz
make FC=gfortran （如果没有安装gfortran,执行sudo apt-get install gfortran）(centos是yum install gcc-gfortran)
make install (将OpenBLAS安装到/opt下)
```

#### VTK（opencv内含VTK,看提示是否需要安装）

```
wget https://www.vtk.org/files/release/8.2/VTK-8.2.0.rc2.tar.gz
tar -xvf VTK-8.2.0.rc2
cd VTK-8.2.0.rc2
mkdir build
cd build
cmake3 ../ -DBUILD_SHARED_LIBS=ON -DBUILD_TESTING=ON -DCMAKE_BUILD_TYPE=Release -DVTK_WRAP_PYTHON=ON
make -j5
make test
```

参考 https://unix.stackexchange.com/questions/306682/how-to-install-vtk-with-python-wrapper-on-red-hat-enterprise-linux-rhel

#### ogre
```

wget https://github.com/OGRECave/ogre/archive/v1.11.5.tar.gz
tar -xvf v1.11.5.tar.gz
mkdir build
cd build
cmake3 ..
make -j2
sudo make install
```
资源包
```
rpm -Uvh http://rpmfind.net/linux/fedora/linux/releases/29/Everything/x86_64/os/Packages/o/ogre-devel-1.9.0-24.fc29.i686.rpm
```

-资料查询http://wiki.ogre3d.org/Prerequisites?tikiversion=Linux
-安装资料http://wiki.ogre3d.org/Building+Ogre


#### ippicv （这个直接下载到opencv的对应的目录）

https://github.com/opencv/opencv_3rdparty/tree/ippicv/master_20170418/ippicv

## 编译opencv

这里分两种情况

### opencv3.4.5的编译
```
cd opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_QT=OFF -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_OPENGL=ON -D WITH_FFMPEG=OFF -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
make -j6
sudo make install
```

注意上面的代码
OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules
这里的目录地址是 你下载的opencv_contrib所在目录

### opencv4.0.0+的编译
```
mkdir build
cd build
cmake3 -D CMAKE_BUILD_TYPE=RELEASE ..
cmake3 -D CMAKE_INSTALL_PREFIX=/usr/local ..
cmake3 -D INSTALL_C_EXAMPLES=ON ..
cmake3 -D INSTALL_PYTHON_EXAMPLES=ON ..
cmake3 -D WITH_TBB=ON -D WITH_EIGEN=ON ..
cmake3 -D WITH_V4L=ON ..
cmake3 -D OPENCV_SKIP_PYTHON_LOADER=ON ..
cmake3 -D OPENCV_GENERATE_PKGCONFIG=ON ..
cmake3 -D WITH_QT=ON ..
cmake3 -D WITH_OPENGL=ON ..
cmake3 -D PYTHON_DEFAULT_EXECUTABLE=/usr/bin/python3 ..
cmake3 -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
```
不一定需要cxx11-所以不一定需要下面的参数
```
cmake3 -D ENABLE_CXX11=ON ..
```
最后
```
make && make install
```

### 修改环境变量文件，永久生效
```
vi /etc/profile
export  PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig    #添加在文件末尾并保存退出
source /etc/profile    #退出后执行
```
或者
sh -c 'echo "/usr/local/lib64" > /etc/ld.so.conf.d/opencv.conf'
### 测试是否编译成功
```
pkg-config --libs opencv
```
如果以上代码有输出代码，代表安装成功了。

### centos7 安装php-opencv
你可以下载原作者的源
```
git clone https://github.com/hihozhou/php-opencv.git
```

也可以下载我这里的，区别我这里的是增加了获取video信息的方法，见上面的【新增功能说明】。

然后使用以下命令
```
cd php-opencv

phpize

./configure --with-php-config=/usr/bin/php-config --enable-debug

make CXXFLAGS='-std=c++11'   //这个很重要，opencv4的话必须要这个
```
解析下第三句命令
/usr/bin/php-config
这个请自行替换你本机的php的php-config所在目录。实在不知道在哪里，可以使用以下命令查找下
```
find / -name php-config 
```
另外如果
make CXXFLAGS='-std=c++11'
命令出现问题，你可以直接使用以下命令
```
make && make install
```
进入php配置目录，如果你是yum安装的php应该可以找到
```
cd /etc/php.d
cp opencv.ini
vim opencv.ini
```
复制以下内容到opencv.ini
```
; Enable opencv extension module
extension=opencv.so
```
按esc,:qt保存。
执行
```
service php-fpm restart
php -m
```
查看opencv模块是否存在，存在的话就安装成功了。
