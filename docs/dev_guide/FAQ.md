# FAQ for Developer

## 编辑文档
欢迎大家来协同编辑文档:  [codelab-adapter-docs](https://github.com/Scratch3Lab/codelab-adapter-docs)

## 讨论组
陆续有开发者建议我构建论坛(discourse)和微信群方便大家讨论技术问题。

微信群无法沉淀有价值的内容，搜索功能太烂了，对富文本/markdown的支持几近于无，微信不是好的办公工具。

与codelab-adapter相关技术问题，大家可以在[issue](https://github.com/Scratch3Lab/codelab_adapter_extensions/issues)里讨论。

也可以在[codelab-adapter讨论组](https://forums.codelab.club/c/codelab-adapter)里讨论。

## 插件启停
目前，插件启动为线程。Python线程需要[手动管理](https://python3-cookbook.readthedocs.io/zh_CN/latest/c12/p01_start_stop_thread.html)，这部分的代码目前还比较粗糙。为了允许用户在UI中通过勾选来启停插件。建议插件作者使用`while self._running:`,参考[extension_demo](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_demo.py)


在1.0版本发布之前，插件部分我们将迁往协程，如此一来我们就能轻易管理插件的启停。目前Python社区很多库还不支持协程，所以我们不打算立刻迁移。

## 引入第三方Python库
建议统一采用ZeroMQ通信风格。参考:

*  [extension_vector.py](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_vector.py)
*  [extension_cozmo.py](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_cozmo.py)
*  [extension_raspberrypi.py](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_raspberrypi.py)
*  [extension_opencv.py](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_opencv.py)

内置的第三方库参考:[wiki](https://github.com/Scratch3Lab/codelab_adapter_extensions/wiki)

<!--
Python社区有海量的第三方库，开发者可以将其引入插件中。

方法是使用`sys.path.append`,如果希望在插件中使用本机Python3已安装的库(推荐`pip3 install xxx --user`)，则将其添加到插件头部:`import sys;sys.path.append("/Users/wuwenjie/Library/Python/3.6/lib/python/site-packages")`,完整的示例参考[extension_third_party_library](https://github.com/Scratch3Lab/codelab_adapter_extensions/blob/master/extension_third_party_library.py)

`/Users/wuwenjie/Library/Python/3.6/lib/python/site-packages`可通过`python3 -m site --user-site`看到。你也可以使用virtualenv创建的虚拟目录。

有些库引入的时候可能会有问题，一些复杂库，建议使用subprocess跑为子进程。
-->


## Python与Scratch的双向通信
参考[Python与Scratch的双向通信](https://blog.just4fun.site/python-scratch-with-adapter.html)

大多数情况下，你只需要发送和接受字符串就够了，这种风格与Scratch内置的广播极为相近。是典型的事件驱动风格。

这篇教程主要针对那些希望去拓展Scratch的人。当你需要将一些复杂的程序接入Scratch（例如接入AI或者接入微信，如我们制作的例子），它会对你有帮助。

## 如何接入arduino
陆续有开发者问到，如何使用codelab-adapter将arduino接入到Scratch3.0中。

有许多种方法，但我比较偏好在arduino中烧入Firmata固件。之后以固件交互，我在[两种硬件编程风格的比较](https://blog.just4fun.site/Hardware-Programming-style.html)论述了这样做的好处。

之后使用Firmata python client与arduino交互。

细节可以参考[Arduino与Scratch3.0](https://blog.just4fun.site/Scratch3-adapter-Arduino-scratch.html)