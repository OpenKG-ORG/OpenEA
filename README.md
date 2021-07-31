# 基于知识图谱嵌入的开源实体融合工具

## 背景

知识图谱可以由任何机构和个人自由构建，其背后的数据来源广泛、质量参差不齐，导致它们之间存在多样性和异构性。例如，对于相交领域 (甚至是相同领域)，通常会存在多个不同的实体指称真实世界中的相同事物。知识融合的目标就是将不同知识图谱融合为一个统一、一致、简洁的形式，为使用不同知识图谱的应用程序间的交互建立互操作性 。知识融合的常用技术方法包括本体匹配 (也称为本体映射)、实例对齐 (也称为实体匹配、对象共指消解) 以及真值验证 (也称为冲突检测) 等。

知识融合是知识图谱研究中的一个核心问题，对于人工智能和大数据至关重要。知识融合研究有助于提升基于知识图谱的信息服务水平和智能化程度，推动语义网以及人工智能、数据库、自然语言处理等相关领域的研究发展，具有重要的理论价值和广泛的应用前景，可以创造巨大的社会和经济效益。

## OpenEA-Tutorial

为帮助了解和熟悉知识融合的常用技术，我们推出了 OpenEA-Tutorial(https://github.com/OpenKG-ORG/OpenEA/tree/master/tutorial)，其中包括本体匹配、实体对齐和真值验证三个任务的代码框架。我们为每个任务给定了评测数据集，并实现了一个基线方法以供参考，使用者可修改指定代码段来实现自己的算法完成相应任务。

1. 本体匹配。本体匹配侧重发现 (模式层) 等价或相似的类、属性或关系，是消除本体间异构性的一种有效途径，可以为应用程序之间的交互建立互操作性，是知识融合的重要任务。在这一任务中，我们的基线方法使用了最基础的文本相似性度量方法——基于字符的 Levenshtein 编辑距离。我们鼓励使用者自行实现其他文本相似性度量方法或是基于图结构的匹配方法等，以在测试数据集上取得更好的效果。
2. 实体对齐。相较于本体匹配，实体对齐侧重发现指称真实世界相同对象的不同实例。我们在此任务中提供了 MTransE 的实现作为基线方法，这是一种基于表示学习的实体对齐方法，其实现基于后续将进行介绍的开源软件库 OpenEA。使用者可以通过改进 embedding learning (EL) 模块和 alignment learning (AL) 模块提升模型性能，也可以进一步尝试其他实体对齐方法。
3. 真值验证。在匹配的基础上，知识融合需要消解知识集成过程中的冲突，再对知识进行关联与合并，最终形成一个一致的结果，真值验证就是冲突消解中的一种技术。为了消解多源数据的冲突，基线方法简单地在离散无序的属性上投票、在数值属性上取均值。使用者显然可以优化这一算法，或者实现其他真值验证算法。

## OpenEA开源库

作为知识融合的重要一环，实体对齐旨在从不同知识图谱中识别指向真实世界同一对象的实体。随着表示学习技术在诸如图像、视频、语音、自然语言处理等领域的成功，基于嵌入的实体对齐方法开始涌现，并取得重大突破。这类方法基于知识图谱嵌入技术，其将知识图谱中的符号表示嵌入到低维向量中，使得实体之间的语义关联能够通过嵌入空间中的几何结构捕捉到。基于嵌入的实体对齐方法典型框架以两个不同知识图谱作为输入，并根据源信息收集种子实体对，然后在嵌入和对齐模块中输入这两个知识图谱和种子实体对，捕捉实体嵌入的对应关系。模块交互有两种典型的组合范式：(1) 嵌入模块将两个知识图谱嵌入进不同空间中，同时对齐模块通过种子实体对学习两个空间中的映射关系；(2) 对齐模块指导嵌入模块，通过强制种子实体对中的对齐实体具有非常相似的嵌入，使得两个知识图谱被表示到一个统一空间中。最后，通过学习到的嵌入表示来测量实体相似性。

![image](https://user-images.githubusercontent.com/54384385/127739763-7c0b55bf-4202-4b39-8bc1-c67b2e843a34.png)

OpenEA (https://github.com/OpenKG-ORG/OpenEA) 是一个面向基于嵌入的知识图谱实体对齐的开源软件库，由南京大学万维网软件研究组 (Websoft) 贡献。OpenEA 通过 Python 和 Tensorflow 开发得到，集成了 12 种具有代表性的基于嵌入的实体对齐方法，同时它使用了一种灵活的架构，可以较容易地集成大量现有的嵌入模型。

![image-20210723164239053](openEA.jpg)

- 嵌入模块 (embedding module)。嵌入模块试图将知识图谱嵌入到低维空间中。根据三元组的类型，我们可以将嵌入模型分为两类：关系嵌入与属性嵌入。前者采用关系学习技术捕捉知识图谱结构，后者利用实体的属性三元组信息。关系嵌入主要有三种实现方式：基于三元组的嵌入能够捕捉关系三元组的局部语义 (例如 TransE)、基于路径的嵌入利用跨越路径的关系之间的长程依赖信息 (例如 IPTransE、RSN4EA)、基于邻居的嵌入主要利用实体之间的关系构成的子图结构 (例如 GCN)。一些方法使用属性嵌入增强实体之间的相似性度量，属性嵌入有两种方式：属性相关性嵌入主要考虑属性间的相关性 (例如 JAPE)、字面量嵌入将字面量值引入到属性嵌入中 (例如 AttrE)。
- 对齐模块 (alignment module)。对齐模块使用种子实体对作为训练数据来捕捉实体嵌入表示的相关性，其中两个关键是选择何种距离度量方式以及设计何种对齐推断策略。度量方式有三种被广泛使用：余弦距离、欧几里得距离和曼哈顿距离。针对对齐推断策略，目前所有方法都采用贪心搜索方式，即为每一个实体依据度量方式选择距离最短的实体作为推断的对齐实体。
- 交互模块 (Interaction between modules)。有四种典型的组合模式用于调整知识图谱嵌入以便实体对齐：嵌入空间的转换，通过种子实体对 (e_1,e_2) 学习两个嵌入空间中的转换矩阵 M 使得 Me_1 \approx e_2。另一种组合模式称为嵌入空间校准，其将两个知识图谱嵌入到统一空间中，通过最小化 ||e_1-e_2|| 来校准实体对中的嵌入表示。作为两个特例，参数共享模式直接设置 e_1=e_2，而参数交换模式通过在三元组中交换种子实体来产生额外三元组作为监督数据。这两种方式都没有引入新的损失函数，但后者会产生更多三元组。基于如何处理标记和未标记数据，学习策略可以被分为监督学习和半监督学习。监督学习采用种子实体对作为标记的训练数据。对于嵌入空间的转换，种子实体对用于学习转换矩阵；对于空间校准，其被用于让对齐的实体具有相似的嵌入表示。半监督学习会在训练阶段使用未标记数据，例如自我学习和协同学习。前者迭代地选出新的实体对补充进种子实体对中，后者通过组合两个学习模型，交替增强彼此的对齐能力。

## 结束语

如果您在使用 OpenEA 及其 Tutorial 过程中遇到任何问题，欢迎在项目 Issues 中提出！感谢孙泽群、张清恒、王成名等人研发 OpenEA，孙泽群、成威和朱向荣对 Tutorial 的实现，以及李光耀对相关工作的总结。
