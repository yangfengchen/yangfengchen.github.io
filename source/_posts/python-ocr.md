---
title: python使用ocr入门
categories:
  - python
date: 2020-06-06 21:00:13
tags:
---

风尘：小二，这种纯中文字的图片处理成文本格式，你有哪些推荐

小二：客官推荐你使用Tesseract-OCR试试



1、目标

​	python使用ocr能识别简单的图片文字信息转换成文本

2、磨刀

​	2.1、安装Tesseract-OCR

​	2.2、pip install -U pillow

​	2.3、pip install -U pytesseract

#ocr的tessdata可以放训练模型

https://github.com/tesseract-ocr/tessdata.git

3、结果

```
import pytesseract
from PIL import Image

#设置ocr路径
pytesseract.pytesseract.tesseract_cmd = 'D:/Program Files/Tesseract-OCR/tesseract.exe'
tessdata_dir_config = '--tessdata-dir "D:/Program Files/Tesseract-OCR/tessdata"'

#若图片模糊可以通过程序降噪来处理
image = Image.open("D:/1.png")
# image = image.convert('RGB')
code = pytesseract.image_to_string(image)
print(code)
```