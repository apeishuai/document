# 前置知识
本文档目的是形成对python全局的理解，拿到一个代码片段知道从什么地方切入分析，核心内容大体如下：

## 核心概念
重用机制、代码组织方式
内存管理
变量生命周期
构建抽象方式

## 基础语法
注释、缩进、变量和数据类型、控制语句、函数。(详见pep8)

## 内存管理机制、变量初始化、范围
变量：显示初始化、隐示初始化、未初始化(NameError异常)

内存管理机制：引用技术、垃圾回收机制(不用操心内存问题)

c中可以进行宏替换，python使用模块机制：
python解释器会搜索一系列预定的路径，包括当前目录、python标准库路径和环境变量中指定的路径，按照一定的顺序逐个搜索这些路径，并编译成字节码(.pyc)文件，缓存，以便下次导入时直接使用

python中代码的组织单位主要是模块，模块是一种将相关功能和代码组织在一个独立文件夹中的方式，基本每个.py文件都可以看作一个模块
## 并发

## 编程范式
### 面向过程
以函数为基本单位构建过程抽象，(回到过程抽象手法，但是算法很不直观)，python提供了一些基本的数据结构，如列表、元组、字典、集合

关注算法即可！

### 面向对象
class为基础，核心是设计模式，即通过类的继承、组合解耦合业务逻辑，使其面对变更更容易扩展

### 元编程
装饰器
类装饰器、etc
元编程就是将过程作为参数或者返回值，这么做能大大提升函数的抽象能力，参考c++泛型(模板机制)


# 项目构建
文件操作(excel、word、etc)、各种库的使用、网络编程、服务器后端、写脚本、etc
(具体的业务逻辑及相关模型，等遇到实际项目再细化)



# 代码片段
生成临时文件
```python
import subprocess
import tempfile

grep_command = ['grep', '//$', 'tmp_urls.txt']
awk_command = ['awk', '-i','inplace','{sub(/\/\/$/, \"/\")}1', 'tmp_urls.txt']

grep_rlt = subprocess.run(grep_command, capture_output=True, text=True)
file_list = grep_rlt.stdout.splitlines()

for file in file_list:
    subprocess.run(awk_command,text=True)


'''
with tempfile.NamedTemporaryFile(mode='w', delete=False) as tmp_grep:
    subprocess.run(grep_command, stdout=tmp_grep, text=True)

    with tempfile.NamedTemporaryFile(mode='w', delete=False) as tmp_awk:
        subprocess.run(awk_command, stdout=tmp_awk, text=True)
        
        tmp_file = tmp_awk.name

        with open(tmp_file, 'r') as tmp_awk_read:
            file_content = tmp_awk_read.read()
            print("awk 临时文件内容:")
            print(file_content)



    tmp_grep_file = tmp_grep.name
    with open(tmp_grep_file, 'r') as tmp_file_read:
        file_content = tmp_file_read.read()
        print("grep 临时文件内容:")
        print(file_content)
        '''

```	
