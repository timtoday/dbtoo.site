---
title : Windows下PIL安装
layout :  post
tags : 
- Python
- PIL
---
PIL在windows可以到官网下载   
http://www.pythonware.com/products/pil/

但是64位的不能下载，找到一个第三方的库：   

http://www.lfd.uci.edu/~gohlke/pythonlibs/   

这里可以找到很多whl结尾的文件，下载后改成zip格式拷贝到python目录就可以使用了。

PIL的替代版本是Pillow，使用方式跟PIL稍微有点不同就是引入：

```python
from PIL import Image
```

原本的PIL版本是：

```python
import Image
```