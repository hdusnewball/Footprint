# Jupyter Notebook in Ubuntu

在 Ubuntu 18.04 中安装并使用 Jupyter Notebook 以及相关配置说明。



## 前提

1. 有个 Ubuntu 18.04 
2. 有 root 权限
3. 当前 python version 是 3



## 第 1 步 - 设置 Python

需要更新本地`apt`包索引：

```
sudo apt update
```



接下来，安装pip和Python头文件：

```
sudo apt install python3-pip python3-dev
```



## 第 2 步 - 为Jupyter创建Python虚拟环境

这里用`python3-venv`安装 jupyter，保证安装好的 jupyter python 内核是 3 版本。先安装依赖再安装 `python3-venv`：

```
sudo apt install build-essential libssl-dev libffi-dev python3-dev
sudo apt install -y python3-venv
```

该`-y` 表示免确认直接安装。



安装好后，我们可以开始准备我们的项目环境-Jupyter 主目录。Jupyter 主目录其实就是之后 Jupyter 的“活动范围”，它会在我们指定目录下进行操作，就好像 Ubuntu 的 home 目录。

![1557392869908](/home/brandewijn/.config/Typora/typora-user-images/1557392869908.png)

在某处创建并`进入`到我们的 Jupyter 主目录中。我们会称之为`jupyter_project_dir`，可以自拟。

```
mkdir ~/my_project_dir
cd ~/my_project_dir
```



在Jupyter 主目录中，我们将创建一个Python虚拟环境。这将创建一个`my_project_env`在目录中。具体 3.x 使用 `python -V` 查看。

```
python3.6 -m venv my_env
```



在我们安装Jupyter之前，我们需要激活虚拟环境，且之后每次使用都要激活这个环境：

```
source my_project_env/bin/activate
```



命令行提示应更改为表明现在在Python虚拟环境中运行。它看起来像这样：`(my_project_env)user@host:~/my_project_dir$`



## 第 3 步 - 安装Jupyter

激活虚拟环境后，使用pip的本地实例安装Jupyter。

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