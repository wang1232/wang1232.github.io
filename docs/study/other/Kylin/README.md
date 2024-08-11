# Kylin

## 软件安装

### opencv

![image-20240621172717916](Kylin.assets/image-20240621172717916.png)

```shell
# opencv-4.9.0.zip 源文件解压
cd /opencv-4.9.0
mkdir bulid
sudo -s  #一定要在管理员模式下编译
cd bulid  # 上级目录一定要有CMakeList.txt
sudo cmake ..
sudo make -j 4/8/10
sudo make install
```

**添加opencv依赖库**

```shell
sudo apt-get install build-essential
```



### **opencv-contrib**

```shell

```

