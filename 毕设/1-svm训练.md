作用：训练分类车牌数字跟省份的svm

### 如何遍历获取文件?



函数介绍

要取得该文件夹下的所有文件，可以使用for (root, dirs, files) in walk(roots)函数。

- roots 代表需要遍历的根文件夹
- root 表示正在遍历的文件夹的名字（根/子）
- dirs 记录正在遍历的文件夹下的子文件夹集合
- files 记录正在遍历的文件夹中的文件集合

~~~python
for (root, dirs, files) in os.walk("开始遍历的目录文件夹名(基于项目，写相对路径)"):
    if len(os.path.basename(root)) > 1:
            continue
~~~

这样就会遍历这个文件夹下的所有文件，在for语句内可以执行操作

并且`os.path.basename(root)`会返回当前目录的这个文件最后的文件名(如果是文件夹则返回文件夹名，没有任何文件返回空) 比如

~~~python
path='D:\lzp\abc\one.jpg'
os.path.basename(path)
#输出 one.jpg
~~~

这样就确保了没有正确的文件则及时停止程序

### svm的训练？

opencv文档中，介绍了一个手写数据识别的例子，其中在训练svm时用定义的函数很有参考价值，所以可以借鉴一下

[OCR of Hand-written Data using SVM](https://docs.opencv.org/trunk/dd/d3b/tutorial_py_svm_opencv.html)

