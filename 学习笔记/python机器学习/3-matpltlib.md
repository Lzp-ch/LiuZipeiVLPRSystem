可视化数据使用

跟matlab octave很像

#### 基本语法

~~~python
import pandas as pd
unrate = pd.read_csv('unrate.csv')
unrate['DATE'] = pd.to_datetime(unrate['DATE']) #pd中还提供了日期格式转换的函数
print(unrate.head(12))

import matplotlib.pyplot as plt
first_twelve = unrate[0:12]
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])#画图，两个参数分别对应xy
plt.show() #展示
~~~

~~~python
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=45) #把x坐标斜45°（如果太长影响美观）
plt.xlabel('Month') #加x轴 y轴
plt.ylabel('Unemployment Rate')
plt.title('Monthly Unemployment Trends, 1948') #标题
plt.show()
~~~

#### 绘制子图

~~~python
fig.add_subplot(2, 3, x) #将要绘制一个2行3列的子图，改变x值来指定在哪个位置画
~~~

~~~python
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = fig.add_subplot(3,2,1)
ax2 = fig.add_subplot(3,2,2)
ax2 = fig.add_subplot(3,2,6) # 在不同位置画图，空白位置会留白
#plt.show()

ax1.plot(np.random.randint(1,5,5), np.arange(5))
ax2.plot(np.arange(10)*3, np.arange(10)) #再画东西
plt.show()
~~~

~~~python
fig = plt.figure(figsize=(3, 3)) #指定画布的大小
~~~

#### 在一个图中画多条函数，并注明

~~~python
fig = plt.figure(figsize=(10,6))
colors = ['red', 'blue', 'green', 'orange', 'black']
for i in range(5):
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    label = str(1948 + i)
    plt.plot(subset['MONTH'], subset['VALUE'], c=colors[i], label=label)
plt.legend(loc='upper left') #给曲线的注释位置，best是自动定位
#help(plt.legend)
plt.xlabel('Month, Integer')
plt.ylabel('Unemployment Rate, Percent')
plt.title('Monthly Unemployment Trends, 1948-1952')

plt.show()
~~~

#### 柱形图

~~~python
import pandas as pd
reviews = pd.read_csv('fandango_scores.csv')
cols = ['FILM', 'RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
norm_reviews = reviews[cols]
print(norm_reviews[:1])
~~~

~~~python
import matplotlib.pyplot as plt
from numpy import arange
#The Axes.bar() method has 2 required parameters, left and height. 
#We use the left parameter to specify the x coordinates of the left sides of the bar. 
#We use the height parameter to specify the height of each bar
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']

bar_heights = norm_reviews.loc[0, num_cols].values #把列的值当作bar的高度
#print bar_heights
bar_positions = arange(5) + 0.75 #bar的间距
#print bar_positions
fig, ax = plt.subplots()

ax.bar(bar_positions, bar_heights, 0.5) #0.5是bar的宽度
#如果是 ax.barh 就是纵向柱形图

plt.show()
~~~

#### 散点图

~~~python
fig = plt.figure(figsize=(5,10))
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
ax1.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['RT_user_norm'])
ax1.set_xlabel('Fandango')
ax1.set_ylabel('Rotten Tomatoes')

ax2.scatter(norm_reviews['RT_user_norm'], norm_reviews['Fandango_Ratingvalue'])

ax2.set_xlabel('Rotten Tomatoes')
ax2.set_ylabel('Fandango')
plt.show()
~~~

#### 取区间，避免x轴乱

~~~python
ax1.hist(norm_reviews['Fandango_Ratingvalue'], bins=20, range=(0, 5)) #把x轴根据x最大值平均分成20个，并且只画x介于0~5的部分
~~~

#### 盒图 *四分图

- IQR即统计学概念四分位距，第一/四分位与第三/四分位之间的距离
- N = 1.5IQR 如果一个值>Q3+N或　<　Ｑ1-N,则为离群点

图中会标明对应y值的1/4、1/2、3/4处在图中什么地方

~~~python
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue']
fig, ax = plt.subplots()
ax.boxplot(norm_reviews[num_cols].values)
ax.set_xticklabels(num_cols, rotation=90)
ax.set_ylim(0,5)
plt.show()
~~~

#### 定义颜色

~~~python
c = (0/255, 197/255, 164/255) # 在rgb元组后都除255
~~~

#### 添加文字

~~~python
ax.text(x, y, 'text') #在指定坐标处添加text
~~~

