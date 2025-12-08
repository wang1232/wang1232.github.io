# MATLAB安装mingw64

MATLAB手动安装MinGW64：

![image-20251208143009573](README.assets/image-20251208143009573.png)

## 1 MinGW下载

MinGW

- 32位下载链接：[Browse /Toolchains targetting Win32/Personal Builds/mingw-builds at SourceForge.net](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/)

- 64位下载链接： [Browse /Toolchains targetting Win64/Personal Builds/mingw-builds at SourceForge.net](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/)

如果是window系统，则选择 [版本号] -> threads-win32 ->seh(任意) -> 点击下载[7z压缩包](https://zhida.zhihu.com/search?content_id=235105882&content_type=Article&match_order=1&q=7z压缩包&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjUzNDcyOTIsInEiOiI3euWOi-e8qeWMhSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjIzNTEwNTg4MiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.vU5NVYp1eKYh-lIAVcDJfs8Ro_be8J6VpWwfAX0LIGk&zhida_source=entity)

我这里使用的是[Matlab 2020b]，因此这里下载的是6.3.0版本

![image-20251208144727654](README.assets/image-20251208144727654.png)

![image-20251208143414759](README.assets/image-20251208143414759.png)



将7z压缩包放到任意任意**不带空格、不带中文**的路径均可。

压缩包解压，图示如下。在这里定义解压后的文件夹路径为[MinGW]，方便后文说明。

![image-20251208145900076](README.assets/image-20251208145900076.png)



## 2 Matlab中配置GCC编译器路径

在Matlab命令行中输入：

```c
setenv('MW_MINGW64_LOC','D:\mingw64')
```

回车，然后再输入：

```c
mex -setup C++
```

如下图示即已完成安装GCC编译器，可以使用mex命令编译C代码了。

![image-20251208145813055](README.assets/image-20251208145813055.png)