相当于在matpltlib上又做了一些封装

使得画图更容易

~~~python
import seaborn as sns
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
~~~

matpltlib默认的，但是可能会不美观，自己一点点调各种参数又很麻烦

~~~python
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
sinplot()
~~~

sns内置了很多模板，可以通过函数调用出来，然后再绘图就是sns模板的样子了

~~~python
sns.set() #sns默认的模板 也跟plt的没大差别
sinplot()
~~~

### 基础语法

#### sns的五种风格模板

* darkgrid
* whitegrid
* dark
* white
* ticks

用`sns.set_style`调用

~~~python
sns.set_style("whitegrid")
data = np.random.normal(size=(20, 6)) + np.arange(6) / 2
sns.boxplot(data=data)

sns.set_style("dark")
sinplot()

sns.set_style("white")
sinplot()

sns.set_style("ticks")
sinplot()

#几种风格
~~~

#### 去掉没用的轴

~~~python
sinplot()
sns.despine()

sns.set_style("whitegrid")
sns.boxplot(data=data, palette="deep")
sns.despine(left=True) #隐藏左边轴
~~~

#### 改变图与轴线间的距离

~~~python
#f, ax = plt.subplots()
sns.violinplot(data)
sns.despine(offset=10)
~~~

#### ‘风格域’

with中的执行darkgrid风格，外部可以执行别的风格，实现不同子图的模板不同

~~~python
with sns.axes_style("darkgrid"):
    plt.subplot(211)
    sinplot()
plt.subplot(212)
sinplot(-1)
~~~

#### 画布风格

~~~python
sns.set_context("paper")
sns.set_context("talk")
sns.set_context("poster")	
sns.set_context("notebook", font_scale=1.5, rc={"lines.linewidth": 2.5}) #字体1.5， 线条粗细2.5
~~~

#### 颜色

##### 调色板 

* 颜色很重要，影响图美观
* color_palette()能传入任何Matplotlib所支持的颜色
* color_palette()不写参数则默认颜色
* set_palette()设置所有图的颜色

##### 分类色板

~~~python
current_palette = sns.color_palette()
sns.palplot(current_palette)
~~~

6个默认的颜色循环主题： deep, muted, pastel, bright, dark, colorblind，没有指定就会循环着用这几个颜色

##### 圆形画板

当你有六个以上的分类要区分时，最简单的方法就是在一个圆形的颜色空间中画出均匀间隔的颜色(这样的色调会保持亮度和饱和度不变)。这是大多数的当他们需要使用比当前默认颜色循环中设置的颜色更多时的默认方案。

最常用的方法是使用hls的颜色空间，这是RGB值的一个简单转换。

~~~python
sns.palplot(sns.color_palette("hls", 8)) #给8个颜色
~~~

##### hls_palette()函数来控制颜色的亮度和饱和

- l-亮度 lightness
- s-饱和 saturation

~~~python
sns.palplot(sns.hls_palette(8, l=.7, s=.9))
~~~

##### 有时候需要几个近似色组

~~~python
sns.palplot(sns.color_palette("Paired",8))
~~~

这样一共4组，一组有两个近似色

##### 应用到数据中

~~~python
sns.boxplot(data=data,palette=sns.color_palette("hls", 8))
~~~

#####  使用xkcd颜色来命名颜色 #####

xkcd包含了一套众包努力的针对随机RGB色的命名。产生了954个可以随时通过xdcd_rgb字典中调用的命名颜色。

就一些名字就行了，调用的时候可以把颜色名调用

~~~python
plt.plot([0, 1], [0, 1], sns.xkcd_rgb["pale red"], lw=3)
plt.plot([0, 1], [0, 2], sns.xkcd_rgb["medium green"], lw=3)
plt.plot([0, 1], [0, 3], sns.xkcd_rgb["denim blue"], lw=3)
~~~

#####  连续色板 #####

色彩随数据变换，比如数据越来越重要则颜色越来越深

~~~python
sns.palplot(sns.color_palette("Blues")) # 一套渐变蓝
#如果想要翻转渐变，可以在面板名称中添加一个_r后缀
sns.palplot(sns.color_palette("BuGn_r"))
~~~

##### cubehelix_palette()调色板

色调线性变换

~~~python
sns.palplot(sns.color_palette("cubehelix", 8))
sns.palplot(sns.cubehelix_palette(8, start=.5, rot=-.75))
sns.palplot(sns.cubehelix_palette(8, start=.75, rot=-.150))
~~~

##### light_palette() 和dark_palette()调用定制连续调色板

返回相应色调的一组深浅，并且light就是整体较浅

~~~python
sns.palplot(sns.light_palette("green"))
~~~

~~~python
sns.palplot(sns.dark_palette("purple"))
~~~

~~~python
sns.palplot(sns.light_palette("navy", reverse=True)) #反转深浅顺序
~~~

##### 应用

~~~python
x, y = np.random.multivariate_normal([0, 0], [[1, -.5], [-.5, 1]], size=300).T
pal = sns.dark_palette("green", as_cmap=True)
sns.kdeplot(x, y, cmap=pal);
~~~

#### 直方图

~~~python
import numpy as np
import pandas as pd
from scipy import stats, integrate
import matplotlib.pyplot as plt

import seaborn as sns
sns.set(color_codes=True)
np.random.seed(sum(map(ord, "distributions")))
~~~

~~~python
x = np.random.normal(size=100)
sns.distplot(x,kde=False) #直接把数据传进来就行了，其他啥也不用管

sns.distplot(x, bins=20, kde=False) #自己指定bins
~~~

查看数据分布情况

~~~Python
x = np.random.gamma(6, size=200)
sns.distplot(x, kde=False, fit=stats.gamma)
~~~

#### 根据均值和协方差生成数据

~~~python
mean, cov = [0, 1], [(1, .5), (.5, 1)]
data = np.random.multivariate_normal(mean, cov, 200)
df = pd.DataFrame(data, columns=["x", "y"]) #pd的DataFrame结构
df
~~~

#### 观测两个变量之间的分布关系最好用散点图

~~~python
sns.jointplot(x="x", y="y", data=df); #可以把直方图，散点图都画出来
~~~

#### hex图

数据量比较大的时候，用这种图看分布情况更直观

~~~Python
x, y = np.random.multivariate_normal(mean, cov, 1000).T
with sns.axes_style("white"):
    sns.jointplot(x=x, y=y, kind="hex", color="k") #指定kind就可以了
~~~

#### 想要特征两两比较

sns提供了一个很方便的函数，可以自动画出来

~~~python
iris = sns.load_dataset("iris") #内置的测试数据集 有四个特征
sns.pairplot(iris) #两两都画出来
~~~

### 回归分析绘图

数据集

~~~python
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt

import seaborn as sns
sns.set(color_codes=True)

np.random.seed(sum(map(ord, "regression")))

tips = sns.load_dataset("tips")

tips.head()
~~~

regplot()和lmplot()都可以绘制回归关系,推荐regplot()

~~~python
sns.regplot(x="total_bill", y="tip", data=tips) #指定y还有特征x到底是谁
#画图完毕会出现一条拟合的直线

sns.regplot(x="size", y="tip", data=tips, x_jitter=.05) #效果差不多，只不过这个方法里支持一些更高级的内容
~~~

实在想用离散值的特征做拟合的话，可以用参数使得数据左右浮动一点（没啥用）

~~~python
sns.regplot(x="size", y="tip", data=tips, x_jitter=.05)
~~~



### 多变量分析绘图

如果特征值重叠较多影响观察，同样可以使他浮动

~~~python
sns.stripplot(x="day", y="total_bill", data=tips, jitter=True)
~~~



### 盒图

* IQR即统计学概念四分位距，第一/四分位与第三/四分位之间的距离
* N = 1.5IQR 如果一个值>Q3+N或　<　Ｑ1-N,则为离群点

一般离群点是很特殊的一组样本，可能是出现了特殊情况，所以可以考虑忽略

~~~python
sns.boxplot(x="day", y="total_bill", hue="time", data=tips);
~~~

### 小提琴图

越胖的地方值越多

~~~python
sns.violinplot(x="total_bill", y="day", hue="time", data=tips);
~~~

### FacetGrid

当想展示子集的时候，需要用到这个

~~~python
import numpy as np
import pandas as pd
import seaborn as sns
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt

sns.set(style="ticks")
np.random.seed(sum(map(ord, "axis_grids")))
tips = sns.load_dataset("tips")
tips.head()

g = sns.FacetGrid(tips, col="time") #告诉他将由time这个列的类型进行子集划分
g.map(plt.hist, "tip"); #根据数据tip中不同时间（早，晚）画出来了两个子图
~~~

