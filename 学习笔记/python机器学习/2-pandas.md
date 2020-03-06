pandas是数据处理库，有利于对于数据的标准化等操作，便于下一步处理

核心结构是pandas.core.frame.DataFrame

#### 数据读取

~~~python
import pandas
food_info = pandas.read_csv("food_info.csv")
print(type(food_info))
print (food_info.dtypes) #返回特征的数据结构，pandas中把字符叫做object
~~~

#### 显示数据

~~~python
food_info.head() # 输出格式化后的数据集，表格形式，跟csv中的一样
food_info.head(3) # 返回前三条
food_info.tali(3) # 返回后三条
food_info.columns # 返回所有特征
food_info.shape # 返回维度
food_info.loc[6] #返回指定样本
food_info["NDB_No"] # 根据列名返回特定列
print(food_info[["Zinc_(mg)", "Copper_(mg)"]]) # 返回多列，这样也是可以的


~~~

#### 数据处理

~~~python
food_info["Energ_Kcal"].max() #某一列的最大值
food_info.sort_values("Sodium_(mg)", inplace=True) # 从小到大排序，True将改变原有数据
food_info.sort_values("Sodium_(mg)", inplace=True, ascending=False) # 降序
~~~



#### 泰坦尼克实际数据处理

~~~PYTHON
import pandas as pd
import numpy as np
titanic_survival = pd.read_csv("titanic_train.csv")
titanic_survival.head()

age = titanic_survival["Age"] #查看age
age_is_null = pd.isnull(age) #判断是否缺失，pd中缺失值是NaN，缺失返回True
age_null_true = age[age_is_null] #把True都存入age_null_true数组
age_null_count = len(age_null_true) #查看长度即缺失age的人数
print(age_null_count)

#所以要对这些缺失age的数据进行处理，否则pd运算时，有一个NaN，结果就会都变成NaN
good_ages = titanic_survival["Age"][age_is_null == False] #只取出没有缺失的进行计算
correct_mean_age = sum(good_ages) / len(good_ages) #correct_mean_age = titanic_survival["Age"].mean()
print(correct_mean_age) #成功

passenger_survival = titanic_survival.pivot_table(index="Pclass", values="Survived", aggfunc=np.mean)
# 每个仓平均获救比例，pivot_table，index是基准类似sql的group，values是计算依据，aggfunc传入方法

passenger_age = titanic_survival.pivot_table(index="Pclass", values="Age")
#同理，计算三个仓的平均年龄，默认是均值

port_stats = titanic_survival.pivot_table(index="Embarked", values=["Fare","Survived"], aggfunc=np.sum)
#同理，获得三个量之间的关系


~~~

~~~python
#specifying axis=1 or axis='columns' will drop any columns that have null values
drop_na_columns = titanic_survival.dropna(axis=1)
new_titanic_survival = titanic_survival.dropna(axis=0,subset=["Age", "Sex"])
#丢弃NaN数据的语法
~~~

~~~python
new_titanic_survival = titanic_survival.sort_values("Age",ascending=False)
#这时候会保留原始index
itanic_reindexed = new_titanic_survival.reset_index(drop=True)
#这样就可以重新给排序后的添加规整的index了，drop说明原来的index直接丢弃
print(titanic_reindexed.iloc[0:10])
~~~

~~~python
def hundredth_row(column):
    # Extract the hundredth item
    hundredth_item = column.loc[99]
    return hundredth_item

# Return the hundredth item from each column
hundredth_row = titanic_survival.apply(hundredth_row)
print (hundredth_row)
#在pd中定义函数并且调用可以这样，用frame的apply方法
~~~

~~~python
def which_class(row):
    pclass = row['Pclass']
    if pd.isnull(pclass):
        return "Unknown"
    elif pclass == 1:
        return "First Class"
    elif pclass == 2:
        return "Second Class"
    elif pclass == 3:
        return "Third Class"

classes = titanic_survival.apply(which_class, axis=1)
print (classes)
#相当于sql中的起别名
~~~

#### Series结构

其实frame结构就是由很多Series结构构成的

~~~python
import pandas as pd
fandango = pd.read_csv('fandango_score_comparison.csv')
series_film = fandango['FILM'] #其中的一列
type(series_film) #返回的就是pandas.core.series.Series
~~~

而且Series是封装的numpy的ndarry类型

~~~python
film_names = series_film.values #<class 'numpy.ndarray'>
~~~

那么如何构造一个Series?

~~~python
from pandas import Series	
film_names = series_film.values
rt_scores = series_rt.values
series_custom = Series(rt_scores , index=film_names) #取rt_scores这一列，把电影名当作索引；相当于人为构造了一种对应
series_custom[['Minions (2015)', 'Leviathan (2014)']] 

'''返回如下内容
Minions (2015)      54
Leviathan (2014)    99
dtype: int64
'''
~~~

不仅数值可以当作索引进行切片操作，电影名也可以当作索引进行切片，同理进行。 要先根据电影名重新排列索引

~~~python
fandango_films = fandango.set_index('FILM', drop=False) #而且保留了原index，也可以用这个查找
fandango_films["Avengers: Age of Ultron (2015)":"Hot Tub Time Machine 2 (2015)"]
~~~

