---
title : PIL压缩图片的方法
layout :  post
tags : 
- Python
- PIL
---
对于jpg文件，都有一个 quality 值，表示图片质量，降低会大大减小文件体积，so,jpg文件可以这样压缩：

```python
from PIL import Image

filename = "1.png"
ouputfile = "1_s.png"
image=Image.open(filename)
image.save(ouputfile,quality=30)

```

但是png没有这个选项，或者说quality对png文件无效果，甚至压缩后还多出一点，所以png最好的办法只有png32转换成png8

```python
from PIL import Image

im = Image.open("1.png")

# PIL complains if you don't load explicitly
im.load()

# Get the alpha band
alpha = im.split()[-1]

im = im.convert('RGB').convert('P', palette=Image.ADAPTIVE, colors=255)

# Set all pixel values below 128 to 255,
# and the rest to 0
#mask = Image.eval(alpha, lambda a: 255 if a <=128 else 0)

# Paste the color of index 255 and use alpha as a mask
#im.paste(255, mask)

# The transparency index is 255
#im.save("1_s.png", transparency=255)

im.save("1_s.png",colors=255)


```