1. 读取文件--**注**：读取文本文件时，Python将所有**文本读取为字符串**，若读取的是**数字，用int()或float()转换**
   - (1) 读取文件全部内容
   ```python
    # 在当前文件所在目录查找"文件名"
    # open()返回一个表示文件"文件名"的对象
    # with在不需要访问文件后将其关闭
    with open("文件名") as fobj:
        # read()读取"文件名"的全部内容，返回一个字符串
        contents = fobj.read()
   ```  
   路径：**相对文件路径**，相对于当前运行的程序所在目录；**绝对文件路径**，windows系统反斜杠。
   - (2) 逐行读取
   
   ```python
   with open("文件名") as fobj:
       for line in fobj:
           print(line)
   ```
   - (3) 创建一个包含文件各行内容的列表，在**with代码外访问文件内容**，文件各行存在一列表中
    ```python
    with open("文件名") as fobj:
        lines = fobj.readlines()

    for line in lines:
        print(line.rstrip())
    ```
   
2.写入文件
3.异常
4.存储文件
