# Android_flask
这是一个基于falsk结构的后端，返回XGBoost算法运行前后的各种信息

### 系统结构
> 每个文件夹的意义
1. data_process，由于每个数据集的情况都不相同，可能属性名称不一致，也可能是因为有多个文件，需要合并。因此在本系统中统一规定数
据的结构，使用不同的处理方法对其进行处理，以Class为列名的就是每条样本的类别，其余为属性，保存为csv格式。原始数据位于 './static/' 
路径下处理完成的数据放在 './static/data/' 路径下。
2. dataInterface，项目的核心文件
    * cleanData.py，放置数据预处理的功能函数
    * finalXGBoost.py，被并入的模型文件，使用的是XGBoost模型，主要功能为训练得到模型
    * myInterface.py，核心逻辑文件，作为模型与后端路由接口之间的桥梁，对数据以及模型进行调度
    * myscoring.py，在不同评估指标下，对最佳参数进行网格搜索时使用的模型评估函数   
3. improve_model，对模型效果进行提升
    * process_data.py，主要通过数据类别比例平衡和特征增强进行模型分类效果进行提升。还未并入系统中。
4. mytools，为了降低代码复杂度，以及方便实现随着项目推进而不断反复横跳的功能需求，将某些临时添加的功能和会反复用到功能进行保存
    * location.py，前端某个视图为了评估指标分布而写的坐标计算功能，不过这个视图在最新版本中已经被删除了
    * make_data.py，对原始数据进行样本分割以及样本偏移人为制造一个差的数据集
5. static，里面放置了原始的数据以及处理完成的数据，也放了一些中间数据以便提高系统的总体运行速度。
    * data，放置系统使用的数据
    * make_data，放置人为制造的数据，可以不用管
    * Model，将模型结构存储为可以阅读的格式
    * Results，存放不同数据集在不同的评估指标下的最佳参数，因为最佳参数是通过网格搜索得到，非常耗费时间，因此将结果保存起来，
    提高系统运行速度
    * temp_data，存放临时数据
6. templates，一般放置前端页面的模板，由于前后端分离，此系统只需要实现后端的逻辑设计。
7. test，里面放置一些用来实验的代码。其中实现了很多功能，但是都未并入系统中，但是可能在后续阶段被反复横跳的需求又用上。
8. app.py，系统的入口，实现数据接口的逻辑设计。直接处理为前端某个视图需要的数据格式，将数据处理的逻辑全放在后端。

### 系统功能
> 抛开具有实验性质的代码，系统主要有三个核心文件：模型文件-数据管理文件-数据处理文件，分别对应finalXGBoost.py、myInterface.py、
>app.py

myInterface.py文件中构造了一个类，用于对数据和模型进行管理。数据包括原始数据和从模型中获得的数据，原始数据来自 'static/data/' 
文件夹下，为了方便动态的添加数据集，因此对数据集的名称映射为data-n；而从模型中获得的数据除了模型评估结果和学习状况等外还包括针
对SHAP解释的数据进行处理。  

app.py文件为前端每个视图都创建了一个路由，但随着视图的不断变动，某些路由也暂时闲置。

### 数据集
系统中用到了5个数据集，存放于 './static/' 路径下，都来自于<https://www.unb.ca/cic/datasets/index.html>。具体的数据介绍可以查看此网站。

### 注意点
1. 虽然后端使用的是flask框架，但是只使用了路由注册和前后端数据交换的功能
2. 如果不使用命令行运行app.py，可能会导致主函数无法运行，其中所做的一些配置也将无法实现，可能会导致一些未知的错误。不过问题不大。
