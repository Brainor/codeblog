---
title: 安装Blender module
tags: python blender
categories: 软件使用
---
[参考](https://playgrd.medium.com/data-analytics-in-blender-pandas-and-quandl-in-blender-d3c816237921)
1. 确定Blender使用的python版本
    ```python
    import sys
    print(sys.version)
    # 3.9.7 (default, Oct 11 2021, 19:31:28) [MSC v.1916 64 bit (AMD64)]
    ```
2. 在conda中安装新的环境
    ```batch
    conda create --name blender python=3.9.7 pandas numpy 
    ```
3. 在新环境中定位python路径
    ```python
    import sys; print(sys.path)
    # ['D:\\software\\miniconda\\envs\\blender\\python397.zip', 'D:\\software\\miniconda\\envs\\blender\\DLLs', 'D:\\software\\miniconda\\envs\\blender\\lib', 'D:\\software\\miniconda\\envs\\blender', '', 'D:\\software\\miniconda\\envs\\blender\\lib\\site-packages', 'D:\\software\\miniconda\\envs\\blender\\lib\\site-packages\\win32', 'D:\\software\\miniconda\\envs\\blender\\lib\\site-packages\\win32\\lib', 'D:\\software\\miniconda\\envs\\blender\\lib\\site-packages\\Pythonwin', 'C:\\Users\\brain\\AppData\\Roaming\\Python\\Python397\\site-packages']
    ```
4. 找到`site-packages`字样的路径添加到Blender的python中
    ```python
    import sys
    sys.path.append('D:\\software\\miniconda\\envs\\py310\\lib\\site-packages')
    ```

或者
1. 定位Blender使用的python路径
    ```python
    import sys
    print(sys.path)
    # ['D:\\software\\Blender\\3.0\\scripts\\startup', 'D:\\software\\Blender\\3.0\\scripts\\modules', 'D:\\software\\Blender\\python39.zip', 'D:\\software\\Blender\\3.0\\python\\DLLs', 'D:\\software\\Blender\\3.0\\python\\lib', 'D:\\software\\Blender\\3.0\\python\\bin', 'D:\\software\\Blender\\3.0\\python', 'D:\\software\\Blender\\3.0\\python\\lib\\site-packages', 'D:\\software\\Blender\\3.0\\scripts\\freestyle\\modules', 'D:\\software\\Blender\\3.0\\scripts\\addons\\modules', 'C:\\Users\\brain\\AppData\\Roaming\\Blender Foundation\\Blender\\3.0\\scripts\\addons\\modules', 'D:\\software\\Blender\\3.0\\scripts\\addons', 'D:\\software\\Blender\\3.0\\scripts\\addons_contrib']
    ```
2. 找到python位置后用pip安装新包
    ```batch
    D:\software\Blender\3.0\python\bin\python -Im pip install --upgrade pip
    D:\software\Blender\3.0\python\bin\python -Im pip install pandas
    ```