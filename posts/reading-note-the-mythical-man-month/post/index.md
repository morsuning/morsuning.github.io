# 《人月神话》 - 读书笔记

很显然，作为一名技术人员，阅读一本不能提供任何技术的“管理学”书籍，首先需要的是耐心，需要的是那种想明白这样一本书到底是存在什么价值才会被奉为经典的好奇心。同时我也希望我能在阅读这样一部已经想读了好多年的经典书籍后能给自己留下一些有用的东西，因此我力图在这篇读书笔记中保留我学习到的知识、对本书的思考与评价和我自己的一些观点。

本书是作者开发 System/360 项目时的经验结晶，本书讨论了软件工程项目过程中较为完整的要素，并给出了一些赫赫有名的观点和结论。最令我印象深刻的有这么几个地方。

首先，它指出程序和编程系统和编程产品和系统级的编程产品这些概念之间是有本质上的不同的。通过这一个观点，首先说明的是文档，系统测试等并不实际起到软件产品的作用，却又是软件产品的重要组成部分的这些软件产物的重要性。失去了这些，那么一个产品将不能被一个团队共同开发，不能保证良好运行，不能继续进行维护和迭代。这完全符合本书所强调的软件任务的根本性问题即软件复杂度问题以及如何降低或者控制复杂度的问题（使用软件工程的除程序外的产物）。

其次，本书开创性地通过量化的形式，讨论了软件工程中工作量和参与人员的关系，同样符合软件复杂性的论述：人越多，添加的复杂性越高，工作进度并不能保证随着人数的增加而增加——即人月神话。在人月神话中，最令人印象深刻的莫过于作者指出添加了人意味着增加了沟通的难度，也就是增加了软件工程任务的复杂度，所以没有人月神话，并通过 Brooks 法则揭示了真实的情况。这是本书的核心观点之一。接着，既然说明了增加人员并不能有效地推进软件工程任务的推进，那么该如何做呢，接下来的 13 章作者给出了自己的观点和建议，以剖析创造性工作的本质为暗线，讲述自己在项目实践中的经验为明线，给出了一套解决方案，按照话题划分，我在这里大致总结如下，只挑关键的部分重点讲述：

- 介绍了一种外科手术团队般的开发团队组成和分工。
  这里也有一些重要结论，如软件开发应该为“精英模式”，也就是永远让最有才华的人解决最难解决的问题，而不是让很多人共同解决问题，那样只会得到更差的结果，除非是进度的压迫迫使我们需要分解问题让更多的人更快地解决。此外，还对小团队高效但是进度慢，大团队低效的问题给出了解决办法。
- 讲述了如何保证设计系统时的概念完整性。
  保证了这种完整性的产品有一个明显的例子，苹果公司(Apple Inc.)的产品，作为一个科技巨头，苹果公司拥有一个 IT/互联网公司拥有的一切重要的东西，处理器，编程语言，操作系统（桌面+移动），独到的工业设计，软件生态等等。但是苹果公司的产品相较于其强大的实力——少的惊人，却又有着极为出众的体验。这就是保证了设计时的概念完整性的结果，产品极为简洁、易用却又能解决所有该解决的问题。当然本书中是在陈述如何做到这件事。
- 给出了一个在二次设计系统时添加不必要的东西的警示。
  描述了在第二次设计一个具相似功能的系统时（也可以说在迭代开发同一个系统时），应该具有什么样的约束和心态。
- 讲述了如何严格执行设计目标。
  这部分是讲述如何进行手册的书写，安排会议，登记日志，安排测试小组等方式达到目标。
- 说明了如何促进团队交流。
- 如何估计编码工作量。
- 如何控制软件规模。
  讲述了功能换空间的理论和如何做到时间空间折中还有改变数据表现形式的观点。我想说一点就是现在一些产品设计上的问题，即会有用空间换时间的操作。现代性能和存储空间已不是瓶颈，存储空间甚至被认为是廉价的这也是一些问题现在不会被考虑的原因。但是，从整体上想，如果想在的系统设计都不考虑这些问题，那未来的某一天，对于一个没怎么考虑过这些问题的超大型复杂系统，显然它是可以正常运行的，但是会变得超级臃肿不堪，数据过度冗余到了无法接受的地步，这样总归是要吃以空间换时间的恶果的。
- 介绍了重要的文档。
- 如何为变化（需求变更，人员变更等）做准备。
- 通用工具的使用。
- 如何保证系统的整体性。
- 如何进行进度控制。、
- 需要对系统进行什么样的说明。

在这些话题中，作者同样给出了很多结论和建议，它们共同组成了一个软件工程活动如何进行的指导方案，或者至少是一个 40 多年前的较为完美的方案。

本书的另外一个核心观点是没有银弹，这是一个预言，一个谦虚的预言，到现在也能成立。作者指出没有任何技术或者管理上的进展，能够独立地许诺在生产率、可靠性或简洁性上取得数量级地提高。这是建立在对软件工程活动本质上的认识上的，虽然作者没有直接地指出这一点，但我们很容易能得出这样的结论：软件工程的本质是一种纯粹的创造活动，我们没有办法大幅度提高人的创造力，也就没有办法大幅度提高生产力。如果对于本质的论断正确，那毫无疑问这个预言是能够成立的。当然，四十多年后的今天，软件的生产率确实提升了，这也是建立在无数抽象和好用的工具及理念的基础之上的。时至今日，仍然没有一个实质意义上的银弹出现。本书也讨论了一些根本解决问题的方法，最让人印象深刻的是“构建软件最可能的彻底解决方案是不开发任何软件”，这个观点完全可以套用到现代的云计算技术上，结合本书的上下文，可以解释云计算所拥有的巨大优势——几乎实现了量化购买“人月”，成本当然远远低于开发自己的全套系统。同时也能解释互联网公司为什么要追求“唯快不破”，其中一方面是因为一旦竞争对手的产品占据了市场，那么自己开发的系统实际上是徒增成本而且还得不到多少收益了。

结合软件工程管理的角度说，我们可以做一个简单的总结。我们要管理的终极目标，正如 SCIP 第一课中所讲述的观点——计算机科学家的任务就是学会如何管理复杂度所说，就是通过人，去设计一个复杂度很高的东西并很好地管理它的复杂度，只不过计算机科学家还要更加地关注硬件罢了。所以说，管理的主要要素，即为人员（团队），文档，成本，进度，工作空间。我们在软件工程管理这个话题下所讨论的一切都要围绕着如何组织这些，这些东西里面包含哪些内容能最大化地帮助我们完成目标进行，而重中之重，是为组织沟通。

本书最大的价值在于对企业项目管理人员的启示，也对创业者大有裨益。它开创性地，系统性地思考了计算机产品开发过程中会出现的问题。同时，还让我们对软件工程管理中的核心问题有一个认识和思考的过程，所以此书中的理论历经 40 多年经久不衰。本书中的很多经验已经被进一步发展形成一套新的管理方式与体系，同时还有一些以前存在的问题被完全解决。如第七章中有明显已不在当前时代的出现的印制工作手册，目前文档基本完全以电子形式。第十章提到的按版本印制的团队文档，现在已被共享文档和项目知识库取代。第十二章提到的程序库，现在已有成熟的分布式版本控制工具如 Git 等，也有在线代码库 GitHub 等，还有集成经理的角色，现在则被成熟的 Code Review 机制取代。第十三章中提到的非常具有前瞻性的结构化编程现在则发展成了开发框架，计算框架和脚手架项目甚至设计模式等形式。第十五章提到的代码格式化规范等等现在已经可以通过工具来实现。

可以看到，很多提到的技术都变成规范化的东西，本书所讲的内容没有被抛弃的，这让我知道了这些东西是因为什么需要而出现的。

由于缺乏具体的实践经验我只能讨论我从书中学习到的东西和我的看法而不是在书中所表达的观点引发的我对实际问题的思考和改进，这也是这一篇笔记的遗憾，希望在未来的某一天，我能带着这本书中的观点，重新审视我做过的事，完成从单纯的技术人员到管理人员的转变。
