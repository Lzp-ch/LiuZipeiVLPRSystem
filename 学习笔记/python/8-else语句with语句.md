## else

在python中else有更多的功能

### 与if配合

~~~python
if :
    #语句
else:
    #语句
~~~



### ..就怎样，否则就别怎样

实例，判断一个数是不是素数

~~~python
def showMaxFactor(num) :
    count = num // 2
    while count > 1 :
        if num % count == 0:
            print('%d的最大约数是%d' % (num, count))
            break
		count -= 1
	else:
            print('%d是素数' % num)
num = int(input('输入一个数'))
showMaxFactor(num)
~~~

只要是在while中break了，那么else就不运行，如果while循环完整的运行完了，则会执行else代码



### 没有问题的话，那就干...

~~~python
try:
    int('abc')
except ValueError as reason:
    print('出错')
else:
    print('没错')
    
#执行出错，else就不运行,注意else在try外边
~~~

~~~python
try:
    int('123')
except ValueError as reason:
    print('出错')
else:
    print('没错')
#try中没有问题，顺利走下来了，那么会运行else，可以发现，else是相对try来说的
~~~



## with

打开关闭文件又要处理异常会很烦人，python提供了with语句

可以简化代码，自动关闭文件

这是一般的没有with的代码，中规中矩

~~~python
try:
    f = open(r'C:\Users\lzp\Desktop\python练习\data.txt', 'w')
    for each_line in f:
        print(eack_line)
except OSError:
    print('出错')
finally:
    f.close()
~~~

用with优化的代码

~~~python
try:
    with open('data.txt', 'w') as f:
        for each_line in f:
            print(each_line)
except OSError :
    print('出错')
    
~~~

