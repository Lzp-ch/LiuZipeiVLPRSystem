numpy是矩阵运算库，核心结构是ndarry类型

### 基本操作

#### genfromtxt

使得numpy可以在txt文件中获取数据集

语法

~~~python
numpy.genfromtxt('文件名', delimiter = '数据特征之间的分隔符', dtype = 数据类型)
~~~



~~~python
import numpy
friend = numpy.genfromtxt('friend.txt', delimiter = ',', dtype = str)
print(type(friend)) #numpy.ndarry 都是这种结构
print(friend) #python语法的数据矩阵

print(help(numpy.genfromtxt)) #不懂就help
~~~

friend.txt

~~~
Liuzipei,1998,male
Yangdaixin,1998,male
Wangzixu,1998,male
Luozehui,1998,male
~~~

#### numpy.arry

构造ndarry结构，传入python的list结构，就可以构造一个ndarry

~~~python
vector = numpy.array([5, 10, 15, 20])
matrix = numpy.array([[5, 10, 15], [10, 20, 30]])

print(matrix.shape) # 输出维度，这里就是(2,3)
~~~

而且，构造的时候，传入的list里边数据结构必须一致

如果不一致，他会向上取类型，比如有一个是float，那所有int都会变成float

#### :表示全选此维度

~~~python
print(friend(:,0)) # 打印矩阵中的所有行的第一列数据
~~~

#### 判断矩阵中有无指定值

~~~python
friend ==10 #将返回一个同纬度矩阵，并且是10的地方是True，不是的地方是False
~~~

而且返回值可以当作索引来使用

#### astype

可以改变ndarray数据的类型

~~~python
vector.astype(float)
print(vector.dtype) #都变成float了
~~~

#### 分维度求和

~~~python
matrix.sum(axis = 1)
~~~

`axis = 1`，把每一行数据都分别相加，最后就是一列和

`axis = 0`，把每一列都加起来，最后成为一行

#### reshape

重新构建矩阵

~~~python
np.arange(15).reshape(3, 5)
#把一个有15个元素的向量转换成了3行5列的矩阵
~~~

#### ndim

返回矩阵维度

#### np.zeros

元组格式，不指定默认是float类型

~~~python
np.ones((3, 4))
~~~

#### np.ones

~~~python
np.ones((2, 3, 4), dtype = np.int32)
~~~

#### np.arange

方便构造向量

~~~python
np.arange(0, 2, 0.2) #从0开始，每次增加0.2，到2结束（不包括2）
~~~

#### np.random

~~~python
np.random.random(2, 3) #random模块的random函数
~~~

返回2行3列介于-1~+1之间的随机数

#### np.linspace

自动计算间隔

~~~python
np.linspace(0, 3.1415, 100) # 从0~3.1315之间取100个相同间隔的数
~~~



### 矩阵计算

~~~python
c = a - b #同维度计算
c = c -1 # 每个元素都-1
c **2 #每个元素都平方
c <5 #返回bool类型的同维度矩阵
a*b #对应位置相乘
np.dot(a, b)#数学矩阵相乘
a.dot(b) #数学矩阵相乘
np.exp(b) #对于每个元素x都变成e的x次方，也就是e为底
np.sqrt(b) #对于b中每个元素开根号
~~~

### 矩阵操作

~~~python
a = np.floor(10*np.random.random((3, 4))) #向下取整构造3x4的-10~+10矩阵
a.ravel() #把矩阵弄成向量
a.shape = (6, 2) # 把a重构成6行2列矩阵
a.reshape(3, -1) # 可以一个维度赋-1，这样就表示根据另一维度自己计算
a.T #得到矩阵转置
~~~

~~~python
np.hstack((a, b)) #矩阵拼接，横向，增加特征
np.vstack((a, b)) #纵向，增加样本
~~~

~~~python
np.hsplit(a, 3) # 拆分特征 平均分成3个矩阵
np.vsplit(a, 3) # 拆分样本

np.hsplit(a, (3,4,5)) # 在第4，5，6个空白处分割，第一个空白是第一个元素左边的空白
~~~

~~~python
b = np.sort(axis = 1) #按照行排序，每一行由小到大
b = np.argsort(a) #返回的是每一行元素排序的索引
~~~



### 矩阵复制

~~~python
b = a #因为python只有名字概念，所以b跟a指向同一矩阵，所以会同时改变
a = c.view() #这样虽然不是同一id，但是共用了矩阵中的元素，形状不跟着改变但是元素同时变
~~~

~~~python
b = a.copy() #正确
~~~

~~~python
np.tile(a, (4, 3)) #行复制成4倍，列复制成三倍数据
~~~



