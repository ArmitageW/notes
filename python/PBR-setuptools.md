# PBR 



## 简介

setuptools 是针对 python 项目的一个打包工具，使用它可以方便地对 python 项目进行
打包安装。要使用 setuptools 只需要在安装脚本中调用其提供的 setup 方法，项目的信
息都通过高度可定制的参数传递给该方法。当调用安装脚本的时候，可以通过传递诸如
build, install 等参数直接进行构建与安装的操作。setuptools 是对 python 标准库中的
[distutils](https://docs.python.org/2.7/library/distutils.html) 的加强。

pbr 是一个对 setuptools 加强的插件，通过它可以将 setup 函数所需的参数放在一个统
一的配置文件中集中管理，此外它也带有自动化管理项目版本的功能。pbr 来自于
[openstack](http://www.openstack.org/software/) 社区。

## 使用

安装：

```shell
pip install setuptools pbr
```

setup.py

```python
import setuptools

# In python < 2.7.4, a lazy loading of package `pbr` will break
# setuptools if some other modules registered functions in `atexit`.
# solution from: http://bugs.python.org/issue15881#msg170215
try:
    import multiprocessing  # noqa
except ImportError:
    pass

setuptools.setup(
    setup_requires=['pbr>=2.0.0'],
    pbr=True)
```

setup.cfg

```
[metadata]
name = Arwen
version = 1.0
summary = my love arwen who is doudou.
author = armitage
home-page = http://mylove_arwen.com
author-email = 729071452@qq.com
python-requires = >=3.6

[files]
packages =
    arwen
data_files =
    etc/arwen =
        etc/api-paste.ini
scripts =
    tools/bm_test.sh
[entry_points]
console_scripts =
    bm-debug = bm.debug.shell:main
```

执行 python setup.py install

（理解怎么使用几个重点功能，相应的文件copy到对应的目录）

```shell
[root@control-plane Arwen]# python setup.py install
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
/home/wyj/Arwen/.eggs/pbr-5.5.1-py2.7.egg/pbr/core.py:131: UserWarning: Unknown distribution option: 'requires_python'
  warnings.warn(msg)
running install
[pbr] Writing ChangeLog
[pbr] Generating ChangeLog
[pbr] ChangeLog complete (0.0s)
[pbr] Generating AUTHORS
[pbr] AUTHORS complete (0.0s)
running build
running build_py
creating build
creating build/lib
creating build/lib/arwen
copying arwen/__init__.py -> build/lib/arwen
copying arwen/arwen_agent.py -> build/lib/arwen
creating build/lib/arwen/common
copying arwen/common/__init__.py -> build/lib/arwen/common
creating build/lib/arwen/common/bm_i18n
copying arwen/common/bm_i18n/__init__.py -> build/lib/arwen/common/bm_i18n
copying arwen/common/bm_i18n/_factory.py -> build/lib/arwen/common/bm_i18n
copying arwen/common/bm_i18n/_lazy.py -> build/lib/arwen/common/bm_i18n
copying arwen/common/bm_i18n/_locale.py -> build/lib/arwen/common/bm_i18n
running egg_info
creating Arwen.egg-info
writing pbr to Arwen.egg-info/pbr.json
writing Arwen.egg-info/PKG-INFO
writing top-level names to Arwen.egg-info/top_level.txt
writing dependency_links to Arwen.egg-info/dependency_links.txt
writing entry points to Arwen.egg-info/entry_points.txt
[pbr] Processing SOURCES.txt
writing manifest file 'Arwen.egg-info/SOURCES.txt'
[pbr] In git context, generating filelist from git
warning: no previously-included files found matching '.gitignore'
warning: no previously-included files found matching '.gitreview'
warning: no previously-included files matching '*.pyc' found anywhere in distribution
writing manifest file 'Arwen.egg-info/SOURCES.txt'
running build_scripts
creating build/scripts-2.7
copying tools/bm_test.sh -> build/scripts-2.7
changing mode of build/scripts-2.7/bm_test.sh from 644 to 755
running install_lib
creating /usr/lib/python2.7/site-packages/arwen
copying build/lib/arwen/__init__.py -> /usr/lib/python2.7/site-packages/arwen
copying build/lib/arwen/arwen_agent.py -> /usr/lib/python2.7/site-packages/arwen
creating /usr/lib/python2.7/site-packages/arwen/common
copying build/lib/arwen/common/__init__.py -> /usr/lib/python2.7/site-packages/arwen/common
creating /usr/lib/python2.7/site-packages/arwen/common/bm_i18n
copying build/lib/arwen/common/bm_i18n/__init__.py -> /usr/lib/python2.7/site-packages/arwen/common/bm_i18n
copying build/lib/arwen/common/bm_i18n/_factory.py -> /usr/lib/python2.7/site-packages/arwen/common/bm_i18n
copying build/lib/arwen/common/bm_i18n/_lazy.py -> /usr/lib/python2.7/site-packages/arwen/common/bm_i18n
copying build/lib/arwen/common/bm_i18n/_locale.py -> /usr/lib/python2.7/site-packages/arwen/common/bm_i18n
byte-compiling /usr/lib/python2.7/site-packages/arwen/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/arwen_agent.py to arwen_agent.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/common/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/common/bm_i18n/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/common/bm_i18n/_factory.py to _factory.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/common/bm_i18n/_lazy.py to _lazy.pyc
byte-compiling /usr/lib/python2.7/site-packages/arwen/common/bm_i18n/_locale.py to _locale.pyc
running install_data
creating /usr/etc/arwen
copying etc/api-paste.ini -> /usr/etc/arwen
running install_egg_info
Copying Arwen.egg-info to /usr/lib/python2.7/site-packages/Arwen-1.0.0.dev4-py2.7.egg-info
running install_scripts
copying build/scripts-2.7/bm_test.sh -> /usr/bin
changing mode of /usr/bin/bm_test.sh to 755
Installing bm-debug script to /usr/bin
[root@control-plane Arwen]#
[root@control-plane Arwen]# bm-debug
this bm main!
[root@control-plane Arwen]# bm_test.sh
Holle test bm_test.sh

[root@control-plane Arwen]# ll /usr/lib/python2.7/site-packages/arwen/
总用量 12
-rw-r--r-- 1 root root 282 1月   4 15:03 arwen_agent.py
-rw-r--r-- 1 root root 738 1月   4 15:03 arwen_agent.pyc
drwxr-xr-x 3 root root  60 1月   4 15:03 common
-rw-r--r-- 1 root root   0 1月   4 15:03 __init__.py
-rw-r--r-- 1 root root 137 1月   4 15:03 __init__.pyc
```





参考：https://blog.apporc.org/python--e6-8c-81-e7-bb-ad-e9-9b-86-e6-88-90-ef-bc-9a-setuptools-pbr/