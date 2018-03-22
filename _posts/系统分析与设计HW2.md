# 系统分析与设计HW2

标签（空格分隔）： HW

---
**目录**
---
[TOC]

#1简答题
##简述瀑布模型、增量模型、螺旋模型（含原型方法）的优缺点。

 - **瀑布模型**
 **优点**
>* 降低软件开发的复杂程度，提高软件开发过程的透明性，提高软件开发过程的可管理性
>* 推迟软件实现，强调在软件实现前必须进行分析和设计工作
>* 以项目的阶段评审和文档控制为手段有效地对整个开发过程进
行指导，保证了阶段之间的正确衔接，能够及时发现并纠正开发过程中存在的缺陷，使产品达到预期的质量要求

 **缺点**
>* 强调过程活动的线性顺序
>* 缺乏灵活性，特别是无法解决软件需求不明确或不准确的问题
>* 风险控制能力较弱
>* 瀑布模型中的软件活动是文档驱动的，当阶段之间规定过多的
文档时，会极大地增加系统的工作量
>* 管理人员如果仅仅以文档的完成情况来评估项目完成进度，往
往会产生错误的结论

 - **增量模型**
  **优点**
>* 增强客户对系统的信心
>* 降低系统失败风险
>* 提高系统可靠性
>* 提高系统的稳定性和可维护性

 **缺点**
>* 增量粒度难以选择
>* 确定所有的基本业务服务比较困难

 - **螺旋模型**
 - **优点**
>* 引入风险管理
>* 设计上的灵活性,可以在项目的各个阶段进行变更
>* 成本计算变得简单容易

 **缺点**
>* 用螺旋模型需要具有相当丰富的风险评估经验和专门知识，在风险较大的项目开发中，如果未能够及时标识风险，势必造成重大损失
>* 过多的迭代次数会增加开发成本，延迟提交时间

## 简述 UP 的三大特点，其中哪些内容体现了用户驱动的开发，哪些内容体现风险驱动的开发
 >UP(统一模型)将软件开发过程要素和软件工件要素整合在统一的软件工程框架中，是一个面向对象的基于 web 的程序开发方法论。
 
 - **UP的三大特点**
1.用例驱动Use-case-driven
* Use-case-driven means the development team employs the use cases from requirements gathering through code and test

 2.以架构为中心Architecture-centric
* software architecture provides the central point around which all other development evolves
 
 3.迭代式增量Iterative and Evolutionary
* An iterative and evolutionary approach allows start of development with incomplete, imperfect knowledge

 - **体现用户驱动的开发**
 用例驱动和迭代式增量
 - **体现风险驱动的开发**
 以架构为中心

## UP 四个阶段的划分准则是什么？关键的里程碑是什么？
 - **UP 四个阶段的划分准则及关键里程碑**
<table>
    <thead>
        <tr>
            <th style="width: 50px;">阶段</th>
            <th style="text-align: center;width: 340px;">划分准则</th>
            <th style="text-align: center;">里程碑</th>
        </tr>
    </thead>
    <tbody>
       <tr>
            <th>初始</th>
            <th>为系统建立业务案例 (Business Case)<br>并确定项目的边界</th>
            <th>生命周期目标(Lifecycle Objective)里程碑</th>
        </tr>
        <tr>
            <th>精化</th>
            <th>分析问题领域，建立健全的体系结构基础，编制项目计划，完成项目中高风险需求部分的开发</th>
            <th>生命周期体系结构(Lifecycle Architecture)里程碑</th>
        </tr>
        <tr>
            <th>构建</th>
            <th>完成所有剩余的技术构件和稳定业务需求功能的开发，并集成为产品，详细测试所有功能</th>
            <th>初始运行能力(Initial Operational Capability) 里程碑</th>
        </tr>
        <tr>
            <th>产品化</th>
            <th>确保软件对最终用户是可用的</th>
            <th>产品发布(Product Release)里程碑</th>
        </tr>
    </tbody>
</table>
 
## IT 项目管理中，“工期、质量、范围/内容”三个元素中，在合同固定条件下，为什么说“范围/内容”是项目团队是易于控制的
- 因为在合同固定的条件下，工期已由合同明确期限，质量由合作方验收或基于用户使用反馈，而范围/内容则是由项目团队自行控制的。


## 为什么说，UP 为企业按固定节奏生产、固定周期发布软件产品提供了依据？
- 因为UP以用例为中心，使用UML的9种图形作为交流语言，在项目之初就建立详细的用例说明，制定详细计划。UP中的软件生命周期在时间上被分解为四个顺序的阶段：初始阶段(Inception)、精化阶段(Elaboration)、构建阶段(Construction) 和产品交付阶段(Transition)，每个阶段结束于一个主要的里程碑(Major Milestone)，并在阶段结尾执行一次评估以确定这个阶段的目标是否已经满足。如果评估结果令人满意的话，可以允许项目进入下一个阶段，所以说UP为企业按固定节奏生产、固定周期发布软件产品提供了依据。

# 2.项目管理使用
使用截图工具（png格式输出），展现你团队的任务Kanban，请注意以下要求

* 每个人的任务是明确的。即一周后可以看到具体成果
* 每个人的任务是1-2项。
* 至少包含一个团队活动任务
![Kanban][1]


  [1]: http://wx4.sinaimg.cn/mw690/a111daecly1fplyiaz0xmj20o4072dg3.jpg