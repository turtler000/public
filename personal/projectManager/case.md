## 计算题

#### 第一期

1、决策树和期望货币值

成功X概率  -  失败X概率

2、加权系统-采购管理

评定人，类似招标方评委

3、沟通渠道---沟通管理

沟通渠道：n(n-1)/2 数量级：n^2

4、盈亏平衡点

成本，服务费，单价

5、投资回收期、回收率

贴现率，把以后的钱换算成现值，每年累计计算，和利息一样

静态投资回收期，不考虑贬值

动态投资回收期，考虑贬值，净现值（NPV)：当年扣掉利息赚的钱,越大越好, 净现值=当前静态钱/（1+贴现率）

回本时间算线性

投资回收率=1/回本时间

6、进度、网络计算题

PERT 计划评审技术

期望时间-----通过计算得到，我们期望的一个工期 				T1

悲观时间-----题目给出，最糟糕的情况                                     T2 

乐观时间-----题目给出，最好的情况										 T3 

最可能时间----题目给出，一般的情况                                      T4

在不同的地方，可能给的符号不一样，但是意思是一样，OK,对于这4个名词，有

一个计算公式：**T1=（T2+T3+4*T4）/6**

另外，还有一个名词，叫做标准差（δ），在很多教程中，写错了，我在这里进行一个纠正，标准差（δ）=（悲观时间-乐观时间）/6，这个必须记住。另外，方差=δ²（标准差的平方）    这个可以不用刻意掌握。

正态分布，68%，95%，99%        每个标准差对应的偏差

#### 第二期

1、双代号网络图算工期，关键路径为历时最长路径，也是整个工作完成的最短时间

双代号网络图：线表示工作，圈表示状态/节点

虚线：可以理解为工期0天的虚工作

关键路径上的自由时差为零

单代号网络图：不带时间，框表示工作，线表示前后关系

总时差：关键路径总时差一定为零

自由时差：紧后工作最早开始-此工作最早结束

#### 第三期

挣值

PV  Planned Value  计划价值（计划成本） ，截止到某时间点计划要完成工作量的价值，也就是计划要做多少事；

EV  Earned Value    挣值 ，截止到某时间点实际已经完成工作量的价值，也就是实际做了多少事；

AC   Actual Cost     实际成本，截止到某时间点实际已经发生的成本，也就是实际花了多少钱；

BAC  Budget At Completion   完工预算，对完成该项目的计划预算，也就是完成整个项目计划多少预算；

**成本偏差（Cost Variance, CV）**，截止到某时点发生的实际成本与计划成本的偏差，**CV=EV-AC**

**进度偏差（Schedule Variance, SV）**，截止到某时点的实际进度与计划进度的偏差，**SV=EV-PV**

**成本绩效指数（Cost Performance Index, CPI）**，截止到某时点衡量成本绩效的一种指标，也就是实际每花一元钱，完成做了多少钱的事（花钱的效率），**CPI=EV/AC**

**进度绩效指数（Schedule Performance Index, SPI）**，截止到某时点衡量进度绩效的一种指标，也就是实际完成的工作量与计划完成工作量之比，**SPI=EV/PV**



预测

ETC=(BAC-EV)/CPI  当前偏差被看做是代表未来的典型偏差,**完工还需要多少钱**

EAC=AC+ETC----------------------------- 衍化为下面两个公式

EAC=AC+BAC-EV 当前偏差被看做是非典型的

EAC=AC+(BAC-EV)/CPI 当前偏差被看做是代表未来的典型偏差

### 例题

#### 1、任务拖后的原因

1、仅依靠一个道路监控项目来估算项目历时，**根据不充分**

2、制定进度计划时，**不仅考虑到活动的历时还要考虑到节假日**

3、没有对项目的技术方案、管理计划进行详细的评审

4、监控粒度过粗（或监控周期过长）

5、对项目进度风险控制考虑不周

#### 2、进度计划包括的种类和用途

1、**里程碑**计划**（1分）**，由项目的各个里程碑组成。里程碑是项目生命周期中的  一个时刻，在这一时刻，通常有重大交付物完成。此计划用于甲乙丙等相关各方**高层对项目的监控****（1分）**。

**2、阶段计划**，或叫**概括性进度表（1分）**，该计划标明了各阶段的起止日期和交

付物，用于**相关部门的协调（或协同）（1分）**。

3、详细**甘特图**计划，或详细**横道图计划，或称时标进度网络图****（1分），**该计划

#### 3、请简要叙述“滚动波浪式计划”方法的特点和确定滚动周期的依据。

1、“滚动波浪式计划”方法的特点是**近期的工作计划得较细，远期的工作计划比较粗**

2、根据**项目的规模**、**复杂度**以及**项目生命周期的长短**来确定滚动周期