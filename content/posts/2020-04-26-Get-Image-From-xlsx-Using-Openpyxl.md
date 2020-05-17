---
title: "Get Images from .xlsx using python openpyxl | 使用python openpyxl提取所有的图片"  
date: "2020-04-26 16:56:57 -0700"
template: "post"
draft: false
slug: "python-openpyxl"
categories: "python"
description: "Get Images from .xlsx using python openpyxl | 使用python openpyxl提取所有的图片"
tags:
  - "python"
---

希望提取出xlsx中的图片文件以及他们所在的行和列

## openpyxl （2.6.2）

这里使用的是[openpyxl](https://openpyxl.readthedocs.io/en/stable/)，但是官方文档中似乎没有对相应需求的解决办法。

所以本文用了许多不是官方文档API的hack，可能只适用于某些openpyxl的版本。

## 实现步骤

下面主要介绍一下本文中需要用到的相关API

0. 安装openpyxl

1. [load_workbook](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.reader.excel.html?highlight=read_workbook#openpyxl.reader.excel.load_workbook) 加载本地的xlsx文件；该函数会返回一个[Workbook](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.workbook.workbook.html#openpyxl.workbook.workbook.Workbook)类型，这是整个xlsx的抽象。

```python
from openpyxl import load_workbook
wb = load_workbook(filename='raw.xlsx')
```

2. 接下来我们从Workbook中获取到某个Worksheet，也就是xlsx中的一个表，使用[wb[sheetname]](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.workbook.workbook.html#openpyxl.workbook.workbook.Workbook.get_sheet_by_name)

``` python
courses = wb['Courses']
```

3. **[可以直接看6]**下一步很自然我们希望逐行遍历找到包含image的cell, 于是通过iter_rows这个方法来遍历每一行，不幸的是，这个信息不在这里。[Cell](https://openpyxl.readthedocs.io/en/stable/_modules/openpyxl/cell/cell.html#Cell)

``` python
TYPE_STRING = 's'
TYPE_FORMULA = 'f'
TYPE_NUMERIC = 'n'
TYPE_BOOL = 'b'
TYPE_NULL = 'n'
TYPE_INLINE = 'inlineStr'
TYPE_ERROR = 'e'
TYPE_FORMULA_CACHE_STRING = 'str'

VALID_TYPES = (TYPE_STRING, TYPE_FORMULA, TYPE_NUMERIC, TYPE_BOOL,
               TYPE_NULL, TYPE_INLINE, TYPE_ERROR, TYPE_FORMULA_CACHE_STRING)
```

4. **[可以直接看6]**那接下来应该怎么找到这些图片？经过一番搜索虽然没找到如何获取image的相关文档，但是文档详细介绍了如何[添加图片](https://bitbucket.org/openpyxl/openpyxl/src/8953233f5af287d9cdf3dae34e437e76bea5bd59/openpyxl/worksheet/worksheet.py?at=default#lines-549)，我们可以发现这是通过Worksheet的一个_images的成员变量实现的。

``` python
def add_image(self, img, anchor=None):
    """
    Add an image to the sheet.
    Optionally provide a cell for the top-left anchor
    """
    if anchor is not None:
        img.anchor = anchor
        self._images.append(img)
```

因此我们可以通过遍历wb._images来找到所有的图片。

5. **[可以直接看6]**那么有了这些Image我们应该怎么获取具体的图像数据，和所在位置呢？文档基本上已经没有任何相关信息，但是源代码可以给我们答案！

通过[image._data()](https://openpyxl.readthedocs.io/en/stable/_modules/openpyxl/drawing/image.html#Image)我们可以获取图像的二进制数据

``` python
def _data(self):
    """
    Return image data, convert to supported types if necessary
    """
    img = _import_image(self.ref)
    # don't convert these file formats
    if self.format in ['gif', 'jpeg', 'png']:
        img.fp.seek(0)
        fp = img.fp
        else:
            fp = BytesIO()
            img.save(fp, format="png")
            fp.seek(0)

            return fp.read()
```

我们也可以通过获取anchor来获取相关的位置信息，在我需要解析的文件中，anchor的类型是[OneCellAnchor](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.drawing.spreadsheet_drawing.html?highlight=oneCellAnchor#openpyxl.drawing.spreadsheet_drawing.OneCellAnchor)。其中的位置编码信息存在于_from中，他的类型是[AnchorMarker](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.drawing.spreadsheet_drawing.html?highlight=AnchorMarker#openpyxl.drawing.spreadsheet_drawing.AnchorMarker)。通过他的col和row的成员变量可以得到所在的行和列。

6. 所以最后的结果是

``` python
for img in courses._images:  
    row = img.anchor._from.row
    col = img.anchor._from.col
    data = img._data()
    do_sth(row, col, data)
```

7. data是二进制数据，如果想要查看，可以通过matplotlib来实现

``` python
import io
from matplotlib import pyplot as plt
import matplotlib.image as mpimg

def show_binary_image(data):
    i = io.BytesIO(data)
    i = mpimg.imread(i, format='PNG')
    plt.imshow(i, interpolation='nearest')
    plt.show()
```
