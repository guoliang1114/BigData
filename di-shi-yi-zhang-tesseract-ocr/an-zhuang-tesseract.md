## 10.2 安装配置Tesseract 4

**安装依赖包**

```
yum install g++ # or clang++ (presumably)
yum install autoconf automake libtool
yum install autoconf-archive
yum install pkg-config
yum install libpng12-dev
yum install libjpeg8-dev
yum install libtiff5-dev
yum install zlib1g-dev

#如果需要训练模型
yum install libicu-dev
yum install libpango1.0-dev
yum install libcairo2-dev

#安装leptonica,它是一个图示处理和分析的软件,可以在http://www.leptonica.org/download.html下载，请注意版本哦。
tar-zxvf leptonica-1.74.4.tar.gz
./configure
make
make install
ldconfig
```

**安装Tesseract**

```
./autogen.sh
./configure --prefix=/ocr/tesseract/ --with-extra-libraries=/usr/local/lib
make install
```

**准备Tesseract语言包**

我们准备常见的中文，繁体，英文包

chi\_sim    Chinese - Simplified    chi\_sim.traineddata

chi\_tra    Chinese - Traditional    chi\_tra.traineddata

eng            English    eng.traineddata

enm    English, Middle \(1100-1500\)    enm.traineddata

osd Orientation and script detection osd.traineddata

准备好包后，将这些文件存放到data目录下，并指定

```
export TESSDATA_PREFIX=/ocr/tesseract/tessdata
```



