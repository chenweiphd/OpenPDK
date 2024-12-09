1. Introduction
又是一篇猎奇内容。如何自动化生成标准单元库。
Another curious piece of content. How to automatically generate a standard cell library .

为什么讲这个？因为cell的定制化是芯片DTCO设计中很关键的一环。当然你可以让FIP团队给你手搓一个cell, 但这里面有两方面的问题。
Why talk about this? Because cell customization is a critical part of chip DTCO design. Of course, you can ask the FIP team to rub a cell for you, but there are two problems.

第一个方面，要让人家干活，就要证明这个新的cell有收益。但我都没有cell, 我怎么能准确的评估cell收益? 所以这是一个鸡生蛋，蛋生鸡的问题。
The first aspect is that if you want people to work, you must prove that this new cell is profitable. But I don’t have a cell, how can I accurately evaluate the cell revenue? So this is a chicken-and-egg problem.


第二个方面，由于摩尔定理的存在，工艺实际上是不断演进的。即使FIP团队同意给你手搓cell, 由于工艺经常迭代，这个cell要重新手搓，工作量实际上非常大。
Secondly, due to Moore’s theorem existence, technology is actually constantly evolving. Even if the FIP team agrees to hand-roll the cell for you, since the process is often iterated, the cell needs to be hand-rolled again, which is actually a very heavy workload.

所以这里面存在一个关键问题：如何能让cell的生成快速迭代起来！
So there is a key question here: how to make cell generation iterative quickly!

这是工业界遇到的实际问题，但学术界似乎并没有解决的很好。除了NVIDIA有用过自家的NVCELL, 其他公司还没听说谁用自动化工具来生成cell。可见实用还很远，只能是一篇猎奇文章。主要是一起瞅瞅有哪些文章，猎个奇。
This is a practical problem encountered in industry, but it does not seem to be solved well in academia. Except for NVIDIA, which has used its own NVCELL, no other company has heard of anyone using automated tools to generate cells. It can be seen that practicality is still far away, and it can only be a novel article. The main thing is to take a look at what articles there are and find out what they are like.

2. AutoCellLibX
先讲第一个。这个工作港科大张薇老师搞的。本科毕业设计的时候还发邮件交流过。这篇文章的核心思路通过频繁子图挖掘技术，识别出设计中高复用率的逻辑模式，并生成这些模式对应的定制标准单元，以减少设计面积。
Let’s talk about the first one first. This job is Zhang Wei from Hong Kong University of Science and Technology The teacher did it. We even exchanged emails during my undergraduate graduation project. The core idea of ​​this article uses frequent subgraph mining technology to identify highly reusable logic patterns in the design, and generate customized standard units corresponding to these patterns to reduce the design area.


如上图，找到公用的cell, 组成一个mega cell，来减少面积。
As shown in the picture above, find common cells and form a mega cell to reduce the area.


最终结论是平均面积缩小了4.49%。 The final conclusion is that the average area has shrunk by 4.49%.

3. ASTRAN 3.ASTRAN
这个工作算是比较有名。放在第二讲。这个工作主要是一个生成版本的工作。用netlist, design rule来生成可用的版图。这个工具应该容易获取，只是65nm以下应该效果就不好了。
This job is relatively famous. Put it in the second lecture. This work is mainly a build version work. Use netlist and design rules to generate usable layouts. This tool should be easy to obtain, but it should not be effective below 65nm.


主要有5个步骤。 There are mainly 5 steps.

第一步计算布局需要的最小面积。基于设计规则计算金属轨间距。据 PMOS 和 NMOS 区域的尺寸，预估单元模板的面积。
The first step is to calculate the minimum area required for the layout. Calculate metal rail spacing based on design rules. According to the size of PMOS and NMOS areas, estimate the area of ​​the unit template.

第二步晶体管folding。将较大的晶体管分解为多个并联的小晶体管，以减小扩散行的高度，同时增加单元宽度。
The second step is transistor folding. Breaking the larger transistor into multiple smaller transistors in parallel reduces the height of the diffusion row while increasing the cell width.

第三步晶体管palcement。这步主要是找到晶体管的位置。
The third step is the transistor palcement. This step is mainly to find the location of the transistor.


如上图，引入最大通道密度和局部通道密度作为成本函数，优化布局：
As shown in the figure above, the maximum channel density and local channel density are introduced as cost functions to optimize the layout:

最大通道密度： 减少最拥挤列中的水平连接数，优化布线能力。
Maximum channel density: Reduce the number of horizontal connections in the most crowded columns, optimizing routing capabilities.
局部通道密度： 降低每列中的交叉连接数，均衡布局密度。
Local channel density: Reduce the number of cross-connects in each column to equalize layout density.
第四步，单元内布线。如下图。生成布线图，节点表示连接点，边表示金属轨道或过孔。
The fourth step is wiring within the unit. As shown below. Generate a wiring diagram with nodes representing connection points and edges representing metal tracks or vias.


用算法找到比较好的布线方式。 Use algorithms to find better wiring methods.

最后一步对布局进行压缩。Layout Compaction。总之就是讲图从数学表达换成物理布局，再优化一轮。
The final step is to compress the layout. Layout Compaction. In short, it is about changing the diagram from mathematical expression to physical layout, and then optimizing it again.

PROBE 3.0
下面一个工作介绍PROBE 3.0。这是一个DTCO FRAMEWORK。之所以这篇文章靠前是因为它写作水平确实不赖，至少做到了样子上的专业。
The next work introduces PROBE 3.0. This is a DTCO FRAMEWORK. The reason why this article ranks high is because its writing quality is indeed not bad, at least it looks professional.


主要思路是上面一张图。自动化DTCO流程。图上一共三个阶段。Technology阶段，Design Enablement阶段，以及Design stage阶段。
The main idea is the picture above. Automate the DTCO process. There are three stages in the picture. Technology stage, Design Enablement stage, and Design stage.

技术阶段（Technology） Technology stage

定义工艺规则和器件模型（如栅极间距、金属间距等）。
Define process rules and device models (such as gate spacing, metal spacing, etc.).
输出设计规则和基础技术参数。 Output design rules and basic technical parameters.
设计启用阶段（Design Enablement）
Design Enablement

自动生成过程设计套件（PDK）和标准单元库，包括DRC、LVS规则和RC模型。
Automatically generate process design kits (PDK) and standard cell libraries, including DRC, LVS rules and RC models.
提供设计工具支持文件（如布线技术文件）。
Provide design tool support files (such as wiring technology files ).
设计阶段（Design） Design stage

使用PDK和标准单元库完成逻辑综合和布局布线。
Complete logic synthesis using PDK and standard cell library and place and route.
评估PPAC（功耗、性能、面积、成本），优化电路性能。
Evaluate PPAC (power consumption, performance, area, cost) and optimize circuit performance.
反馈循环（Feedback） Feedback Loop (Feedback)

设计阶段的结果反馈至技术和设计启用阶段，进一步优化技术和设计规则。
The results of the design phase are fed back to the technology and design enablement phases to further optimize the technology and design rules.
最终目标是把这一套东西都自动化下来。提出一种全面、快速的框架来支持PPAC评估，并为新技术节点提供准确的路径优化。恩，现在叫PPAC，不叫PPA了，多出来的C指的是Cost。
The ultimate goal is to automate this set of things. Propose a comprehensive and rapid framework to support PPAC assessment and provide accurate path optimization for new technology nodes . Well, it is now called PPAC, not PPA. The extra C refers to Cost.

首先是生成标准单元库和PDK。主要讲就干了下面表里这么几件事。
The first is to generate the standard cell library and PDK. Mainly speaking, I did the following things inside and outside.


通过输出一些规则文件，最后把PDK给我们生产出来。目前看起来生产的还是一些基本的cell.
By outputting some rule files, the PDK is finally produced for us. At present, it seems that we are still producing some basic cells.


接下来文章重点讲了这套流程支持的四种电源网络探索。看图。
The following article focuses on the four types of power network exploration supported by this process. Look at the picture.


四种方式的优劣如下表，随着电源网络越来越复杂，手动显然是搞不定了，所以需要这篇文章里提到的自动化。
The pros and cons of the four methods are as follows. As the power network becomes more and more complex, it is obviously impossible to handle it manually, so the automation mentioned in this article is needed.


实验表明，采用 BSPDN 和 BPR 组合的配置（PBB）在3nm技术节点中显著优化了功耗和面积：
Experiments show that the configuration using a combination of BSPDN and BPR (PBB) significantly optimizes power consumption and area in the 3nm technology node:

功耗： 使用 BSPDN 和 BPR 使得功耗降低了 7%-8%。
Power consumption: Using BSPDN and BPR reduces power consumption by 7%-8% .
面积：PBB 配置相比传统前端PDN（PFS）在性能相同的情况下面积减少了 24%。
Area: Compared with traditional front-end PDN (PFS), the area of ​​PBB configuration is reduced by 24% with the same performance.
IR降压： 在不同PDN配置下，背面PDN配置（PBS和PBB）能够有效减小IR降压，而使用BPR的PFB配置则对IR降压的影响较小。
IR voltage reduction: Under different PDN configurations, the backside PDN configuration (PBS and PBB) can effectively reduce the IR voltage reduction, while the PFB configuration using BPR has less impact on the IR voltage reduction.
接下来是文章的玄学部分，AI探索。主要是搞了一个神经网络用来生成网表。通过调节这些参数，ANG能够生成具有不同规模、互连复杂性和时序特性的人工电路网表。我觉得这个东西似乎比较玄学，不细说也罢。
Next is the metaphysical part of the article, AI exploration. The main thing is to build a neural network to generate a netlist. By adjusting these parameters, ANG is able to generate artificial circuit netlists with varying sizes, interconnect complexity, and timing characteristics. I think this thing seems to be rather metaphysical, so I won’t go into details.

接下来是文章如何优化整个框架的性能。 Next is the article how to optimize the performance of the entire framework.

单元优化： 选择合适的标准单元库，在面积和功耗之间找到最佳平衡。PROBE3.0引入了定制单元库的概念，通过自动生成标准单元库，并对其进行面积、功耗和性能的优化。通过自动化的标准单元库扩展，在不同的工艺节点下生成适配的单元库，减少了手工设计单元的时间和成本。
Cell optimization: Choose the appropriate standard cell library to find the best balance between area and power consumption. PROBE3.0 introduces the concept of customized cell libraries by automatically generating standard cell libraries and optimizing their area, power consumption and performance. Through automated standard cell library expansion, adapted cell libraries are generated under different process nodes, reducing the time and cost of manual unit design.
布局与布线（P&R）优化： 通过有效的布局与布线技术，减少线长、交叉点和互连延迟，从而优化功耗和性能。PROBE3.0采用了包括 聚类 和 单元宽度正则化 等方法来提升P&R的效率和效果。
Place and route (P&R) optimization: Optimize power consumption and performance through effective place and route techniques that reduce wire lengths, crossover points, and interconnect delays. PROBE3.0 uses methods including clustering and unit width regularization to improve the efficiency and effect of P&R.
电源优化：PROBE3.0框架支持多种电源网络配置的优化，能够在不同的技术节点下实现更高效的电源分配。
Power optimization: PROBE3.0 framework supports multiple power network configurations The optimization can achieve more efficient power distribution under different technology nodes.
从结果来看，面积减少20%，性能提升20%，功耗降低7%。感觉有点水分。。。一般这种工作提升个5%算是比较理想的结果。
Judging from the results, the area is reduced by 20%, the performance is improved by 20%, and the power consumption is reduced by 7%. Feeling a little hydrated. . . Generally, a 5% increase in this kind of work is considered a relatively ideal result.


如上图，直接生成4套lib, lib1-4实现JPEG编码, 然后分别尝试不同电源方案，画出power（a）, area(b), 随性能的变化。d画一个energy-delay product和area的关系。
As shown above, 4 sets of lib are directly generated , lib1-4 implements JPEG encoding, and then tries different power solutions respectively to draw the changes in power (a), area (b), and performance. d Draw a relationship between energy-delay product and area.

可以看到，实际上不同的设计库有不同的trade off，我们可以通过这套东西快速迭代，来获取我们想要的收益。
As you can see, in fact, different design libraries have different trade off , we can quickly iterate through this set of things to get the benefits we want.

BonnCell
博恩大学搞出来的一个工具。主要业是用来生成stdcell版图的。motivation还是目前缺乏高质量的自动单元布局生成器，因此这些定制单元通常由经验丰富的设计师手动设计。7nm以下cell设计太复杂了，非常耗时。
A tool developed by the University of Bonn. The main business is used to generate stdcell layout. motivation still lacks a high-quality automatic cell layout generator , so these custom units are often designed manually by experienced designers. Cell design below 7nm is too complex and very time-consuming.

这种东西实际上可以总结成一系列比较标准的优化问题。主要流程如下图。
This kind of thing can actually be summarized into a series of relatively standard optimization problems. The main process is as shown below.


流程起点 Process starting point

输入：晶体管集合和网络连接的描述（例如，FET 和 netlist），以及技术约束（设计规则、制造规则）。
Input : Description of the transistor collection and network connections (e.g., FETs and netlist), and technical constraints (design rules, manufacturing rules).
目标：生成一个满足设计规则的晶体管布局，并实现最优的布线。
Goal : Generate a transistor layout that satisfies design rules and achieves optimal routing.
布局算法（Placement Algorithm）
layout algorithm (Placement Algorithm)

起始宽度设置：从最小可能的单元宽度开始，逐步增加。
Starting width setting : Start with the smallest possible cell width and gradually increase.
分支定界布局：
branch and bound layout :
按照从左到右的顺序放置晶体管。 Place the transistors in order from left to right.
对每个晶体管，尝试所有可能的配置（包括位置、指的数量、翻转状态等）。
For each transistor, try all possible configurations (including position, number of fingers, flip states, etc.).
布线检查集成： Cabling check integration :

布局算法与布线检查深度集成。在放置每个晶体管时，通过调用布线引擎（Routing Engine）验证布局的合法性和可布线性。
Placement algorithms are deeply integrated with routing inspections. When placing each transistor, the legality and routability of the layout are verified by calling the Routing Engine.
布线检查确保布局在设计规则约束下是可行的，避免传统方法中布局与布线分离导致的低效。
Routing check ensures that the layout is feasible within the constraints of the design rules, avoiding the inefficiency caused by the separation of layout and routing in traditional methods.
布线步骤（Routing Steps） Routing Steps

布线任务由混合整数规划（MIP）方法求解，确保最短布线长度、最少跨层通孔（vias）和符合制造规则。
The routing task is planned by mixed integer (MIP) method is solved to ensure the shortest wiring length, the minimum cross-layer vias (vias) and compliance with manufacturing rules.
布线引擎多次被调用： The routing engine is called multiple times:
全局布线检查：验证所有网络的可布线性。
Global routing check : Verifies the routability of all nets.
部分布线检查：在布局中间阶段，通过快速的局部布线检测，排除不可行的布局。
Partial wiring inspection : In the intermediate stage of layout, through fast partial wiring inspection, infeasible layouts are eliminated.
总之，直接看结果。 In short, just look at the results.


a图是原始的cell, b图是bonn搞出来的cell。小了不少。标准单元库一共120个cell, 10个cell面积有优化。平均每个cell布局布线时间不足2min。对简单单元（如 AND、OR 门）的布局优化显著。能处理复杂的多行单元（如锁存器）。手动设计需要数天的复杂单元，BonnCell 可在数小时内完成。
Picture a is the original cell, picture b is the cell created by bonn. Much smaller. The standard cell library has a total of 120 cells, and the area of ​​10 cells has been optimized. The average placement and routing time for each cell is less than 2 minutes. The layout optimization of simple cells (such as AND, OR gates) is significant. Ability to handle complex multi-row units such as latches ). Complex cells that would take days to manually design can be completed in hours with BonnCell.

效果至少从文章中看是已经达到得了。只要是可用的，对于DTCO研究来讲就已经解决了部分问题。
At least from the article, the effect has been achieved. As long as it is available, some problems have been solved for DTCO research.

NCTUcell
这些standard cell generator。。名字起的倒是还挺统一。这文章主要解决的一个问题是FinFET 的设计可能需要额外的虚拟栅极（dummy poly）插入，从而增加单元间距并浪费芯片面积。算法都基本上ILP，不细说了，我们只讲讲概念。这文章画的图要看哭了。。。一点都不深入人心，我从其他图找来一个画了一下。
These standard cell generators. . The names are pretty consistent. One of the main issues addressed in this article is that FinFET designs may require additional dummy poly insertion, thereby increasing cell spacing and wasting chip area. The algorithms are basically ILP, so I won’t go into details. We’ll just talk about the concepts. The pictures in this article make me cry. . . It's not very popular at all, so I found one from other pictures and drew it.


FINFET长上面这样。如果直接把dummy gate删掉会导致fin的机械结构不好。不能轻易干掉。如果不轻易干掉，遇到这种情况dummy gate会造成额外电容。
FINFET looks like this. If you directly put the dummy gate Deleting it will result in poor mechanical structure of the fin. It cannot be killed easily. If it is not removed easily, the dummy gate will cause additional capacitance in this situation.


为了避免这种情况，我们需要把中间dummy gate接地。
In order to avoid this situation, we need to connect the middle dummy gate to ground.


这种SDSD间隔的晶体管显然没问题，但是有时候会有两个怼在一起的情况。dummy gate无法接地了。
This kind of SDSD spaced transistor is obviously no problem, but sometimes there will be two cases of being stuck together. The dummy gate cannot be grounded.


这个时候要加上S好让poly gate接地。自然就会导致cell面积上涨。NCTUcell这类工作实际上是在优化，尽量少插这些DDA引起的多余面积。结论是面积降低了5%左右。
At this time, S must be added to ground the poly gate. This will naturally lead to an increase in cell area. Work like NCTUcell is actually optimizing to minimize the redundant area caused by inserting these DDAs. The conclusion is that the area has been reduced by about 5%.

SP&R SP&R
这篇文章似乎是candence的文章。看起来这些传统的EDA厂商也在搞类似的研究。
This article seems to be a candence article. It seems that these traditional EDA manufacturers are also engaged in similar research.

这篇文章的思路和前几篇又有一些不同。传统方法通常通过分解问题（例如晶体管放置、单元内布线、引脚可达性优化）来优化布局，但这限制了整体的解决空间和结果的最优性。这篇文章尝试以pin位置为牵引，一把搞定晶体管的摆放和cel内部的routing。
The idea of ​​​​this article is somewhat different from the previous ones. Traditional methods usually optimize layout by decomposing the problem (such as transistor placement, intra-cell routing, pin accessibility optimization), but this limits the overall solution space and the optimality of the results. This article attempts to use the pin position as a guide to figure out the placement of transistors and the routing inside the cel.


具体这怎么做的我们暂且不关心，此处只贴个结果。SP&R 相较于传统顺序方法，金属长度平均减少 10.5%，最佳情况减少 18.2%。高层布线（M2 层）轨道使用从平均 1.3 减少到 0.8，显著优化了布线资源的利用率。
We don't care about how to do this for now, we will just post the results here. Compared with the traditional sequential method, SP&R reduces the metal length by 10.5% on average and 18.2% in the best case. High-level cabling (M2 layer) track usage has been reduced from an average of 1.3 to 0.8 , significantly optimizing the utilization of cabling resources.

NVCell
这个东西上篇文章应该介绍过。NVIDIA搞出来的cell generator。NVIDIA的思路是先让generator生成一般自动的cell, 然后人工手动调优。
This thing should have been introduced in the previous article. The cell generator developed by NVIDIA. NVIDIA's idea is to first let the generator generate generally automatic cells, and then manually tune them.

主要目标有两个： There are two main goals:

利用强化学习（Reinforcement Learning, RL）来处理布局和布线中的设计规则约束，生成高效的cell布局。
Reinforcement Learning (RL) is used to handle design rule constraints in layout and wiring to generate efficient cell layout.
实现具有竞争力的自动化标准单元设计，降低人工干预的需求。
Achieve competitive automated standard cell designs and reduce the need for manual intervention.
实现算法基本上是传统的EDA算法，模拟退火，基因算法。修DRC的时候用到了强化学习。
The implementation algorithm is basically the traditional EDA algorithm, simulated annealing , genetic algorithm. Reinforcement learning was used when repairing DRC.


结果： result:

15%的单元宽度减少，少于2%的单元宽度增加。 15% of the cells are reduced in width, and less than 2% of the cells are increased in width.
对于某些关键单元（如算术逻辑单元和触发器），实现了显著的面积优化。
For certain key units (e.g. arithmetic logic unit and flip-flops), achieving significant area optimization.
总结 Summarize
这又是一篇猎奇内容。standcell实际上属于很专的一门技术活。迭代了这么久，库里那些cell为什么那么设计都是一代代人踩了若干坑得到的。要想超越，甚至用cell generator自己产生何其困难。AI也许能带来机遇，但从机遇到能拿到收益，还有非常长的路要走。等着吧。
This is another curious piece of content. Standcell is actually a very specialized technical activity. After iterating for so long, the reason why the cells in Curry are so designed is the result of generations of people stepping on many pitfalls. If you want to go beyond it, even using a cell generator to generate it yourself is extremely difficult. AI may bring opportunities, but there is still a long way to go from opportunity to profit. Just wait.
