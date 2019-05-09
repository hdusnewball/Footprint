# Jupyter Notebook in Ubuntu

在 Ubuntu 18.04 中安装并使用 Jupyter Notebook，原文是数字海洋的 Lisa Tagliaferri，我这里做个翻译备用。

[原文点此处](<https://www.digitalocean.com/community/tutorials/how-to-set-up-jupyter-notebook-with-python-3-on-ubuntu-18-04>)



## 前提

1. 有个 Ubuntu 18.04 
2. 有 root 权限
3. 当前 python version 是 3

> 如果是配置远程服务器，请直接查看原文



## 第 1 步 - 设置 Python

我们首先需要更新本地`apt`包索引，然后下载并安装包：

```
sudo apt update
```



接下来，安装pip和Python头文件，这些文件由Jupyter的一些依赖项使用：

```
sudo apt install python3-pip python3-dev
```



## 第 2 步 - 为Jupyter创建Python虚拟环境

我们可以创建一个 Python 虚拟环境来管理我们的项目， 并将 Jupyter 安装到这个虚拟环境中。为了创建这个虚拟环境，我们首先准备好 `virtualenv` ，可以使用 pip 安装的命令，通过输入以下命令升级pip并安装包：

```
sudo -H pip3 install --upgrade pip
sudo -H pip3 install virtualenv
```

该`-H`标志确保安全策略将`home`环境变量设置为目标用户的主目录。



`virtualenv`安装好后，我们可以开始准备我们的项目环境-Jupyter 主目录。Jupyter 主目录其实就是之后 Jupyter 的“活动范围”，它会在我们指定目录下进行操作，就好像 Ubuntu 的 home 目录。

![1557392869908](/home/brandewijn/.config/Typora/typora-user-images/1557392869908.png)

在某处创建并`进入`到我们的 Jupyter 主目录中。我们会称之为`jupyter_project_dir`，但您应该使用对您有意义的名称以及您正在处理的名称。

```
mkdir ~/my_project_dir
cd ~/my_project_dir
```



在Jupyter 主目录中，我们将创建一个Python虚拟环境。这将创建一个`my_project_env`在您的目录中调用的`my_project_dir`目录。在里面，它将安装本地版本的Python和本地版本的pip。我们可以使用它来为Jupyter安装和配置一个独立的Python环境。

```
virtualenv my_project_env
```



在我们安装Jupyter之前，我们需要激活虚拟环境，且之后每次使用都要激活这个环境。您可以输入以下命令：

```
source my_project_env/bin/activate
```



您的提示应更改为表明您现在在Python虚拟环境中运行。它看起来像这样：`(my_project_env)user@host:~/my_project_dir$`



## 第 3 步 - 安装Jupyter

激活虚拟环境后，使用pip的本地实例安装Jupyter。

> **注意：**当虚拟环境被激活时（当前目录位置在`(my_project_env)`前面时），即使使用的是Python，也请使用`pip`而不是`pip3`。

```
pip install jupyter
```



## 第 4 步 - 运行Jupyter Notebook

要运行它，请执行以下命令：

```
jupyter-notebook
```

> Jupyter 在使用的时候需要命令行一直开着，有强迫症可以使用 `screen` 创建可离线窗口。

Jupyter笔记本的活动记录将打印到终端。当您运行Jupyter Notebook时，它运行在特定的端口号上。您运行的第一个笔记本通常会使用端口`8888`。要检查Jupyter Notebook正在运行的特定端口号，请参阅用于启动它的命令的输出。