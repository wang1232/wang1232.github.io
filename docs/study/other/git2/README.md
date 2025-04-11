# 一、github克隆别人的项目给自己

1.新建一个仓库：

![image-20240427151316541](GitHub.assets/image-20240427151316541.png)

2.填写项目名称：

![image-20240427151338823](GitHub.assets/image-20240427151338823.png)

3、不需要勾选readme，直接创建：

![image-20240427151403932](GitHub.assets/image-20240427151403932.png)

4.创建完成，后面即仓库地址

![image-20240427151428981](GitHub.assets/image-20240427151428981.png)

指令开始：

这里注意如果不想在c盘克隆，在其他盘创建一个文件夹cd过去即可。

1.clone你需要的项目

```cpp
git clone xxx
```

2.进入项目目录

```cpp
cd xxx
```

3.删除原有的git信息，有问题一直回车

```cpp
rm -r .git
```

4.初始化git

```cpp
git init
```

5.讲本地代码添加到仓库

```cpp
git add .
git commit -m “xxx” //xxx是下图中的备注，随便打也可以
```

![image-20240427151510416](GitHub.assets/image-20240427151510416.png)

6.在git官网上新建一个项目，注意不要生成README文件，参考上面即可。
7.关联远程库

```cpp
git remote add origin 远程库地址
//即上面的本地仓库的地址
```
8.提交代码，等待上传完毕

```cpp
git push --set-upstream origin master
```

出现以下错误：
![在这里插入图片描述](GitHub.assets/2db54752b7054852a17508bf5c126cb4.png)

解决方法：git config --global http.sslVerify "false"



# 二、Typora与GitHub链接

1、密钥生成

```shell
ssh-keygen -o
```

![image-20240427153647465](GitHub.assets/image-20240427153647465.png)

打开这个目录，里面的pub文件存放的密钥：

```shell
cat  /c/Users/雁门/.ssh/id_ed25519.pub
```

![image-20240427153824249](GitHub.assets/image-20240427153824249.png)

复制密钥到生成即可：





![image-20240427153939283](GitHub.assets/image-20240427153939283.png)

2、切换新目录-->笔记存放的地方

![image-20240427154048046](GitHub.assets/image-20240427154048046.png)

3、链接新建的私人仓库

```shell
git clone https://github.com/wang1232/NoteBook.git
```

![image-20240427154112592](GitHub.assets/image-20240427154112592.png)

4、开始上传

![image-20240427154226543](GitHub.assets/image-20240427154226543.png)

```shell
git add .
git commit -m "add new file"
git push
```



![image-20240427154249919](GitHub.assets/image-20240427154249919.png)

![image-20240427154323874](GitHub.assets/image-20240427154323874.png)







# 三、vscode与github

```
git status
git add .
git commit -m "1111"
git branch
git push origin master
```

## 3.1 下载Git

vscode中使用github首先第一步得下载Git，可以自己从官网下载也可以直接打开vscode：

![image-20250411154014176](GitHub.assets/image-20250411154014176.png)

下载完成后进行傻瓜式安装即可。参考：[Git 详细安装教程（详解 Git 安装过程的每一个步骤）_git安装-CSDN博客](https://blog.csdn.net/mukes/article/details/115693833)

![image-20250411155406029](GitHub.assets/image-20250411155406029.png)

## 3.2 开始克隆

点击克隆git仓库，当选择从远程克隆仓库时，**输入远程仓库地址（一定要输入仓库的地址按下回车）**，按下回车即可：

![image-20250411154715947](GitHub.assets/image-20250411154715947.png)

点击之后会在这输入，点击之后会让你选择自己的本地下载位置，随意选择即可：

![image-20250411154837902](GitHub.assets/image-20250411154837902.png)









# 四、typora图像格式问题

直接处理文件夹下的所有.md文件

```python
import os
import traceback
from bs4 import BeautifulSoup
from bs4.element import Tag


class TransformMD(object):
    def __init__(self):
        if not os.path.exists("transformed"):
            os.mkdir("transformed")

    def trans_img(self, line):
        """
        将 <img> 标签转换为 Markdown 图片格式。
        """
        soup = BeautifulSoup(line, "html.parser")
        newLine = ''
        for content in soup.contents:
            if isinstance(content, Tag) and content.name == "img":
                url = content.get("src", "")
                alt = content.get("alt", "")
                newLine += f"![{alt}]({url})"
            else:
                newLine += str(content)
        return newLine

    def transform(self, filename):
        try:
            name = os.path.basename(filename)
            savepath = os.path.join("transformed", name[:-3] + "-transformed" + name[-3:])
            with open(filename, "r", encoding="utf-8") as r, open(savepath, "w", encoding="utf-8") as w:
                for line in r:
                    if "<img" in line:
                        w.write(self.trans_img(line))
                    else:
                        w.write(line)
        except Exception as e:
            print(f"处理文件时出错: {filename}")
            traceback.print_exc()

    def transformDir(self, dirname):
        """
        转换指定目录中的所有 Markdown 文件。
        """
        for path, _, file_list in os.walk(dirname):
            for file_name in file_list:
                if file_name.lower().endswith(".md"):
                    full_path = os.path.join(path, file_name)
                    print(f"正在处理文件: {full_path}")  # 打印调试信息
                    self.transform(full_path)


if __name__ == '__main__':
    TMD = TransformMD()
    TMD.transformDir("D:/Study_NoteBook/NoteBook/杂/")
```

