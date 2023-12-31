# AI 集群

构建可用的 AI 集群相对来说比较容易，但是构建面向大模型应用的高性能 AI 集群，则需要应对非常多的技术挑战。这对于 AI 架构从业者而言，不仅是挑战，还是机遇。

在未来相当长的一段时间里，提升大模型各项指标的核心推动力都会落在数据上。数据越多，质量越好，训练出来的大模型质量、泛化性也就越好。对比与传统模型，大模型对数据的需求可以说是爆炸性增长，几个 T 的数据集都可以说是小打小闹。数据量的爆炸性增长不是简单的“增加更多硬盘就好了”！一旦数据多起来，除了要在物理上扩容存储，还要考虑怎么高效地组织这些数据，如怎么应对读多写少的场景、怎么按照不同维度快速索引到想要的数据、怎么区分不同版本的数据...此外，数据越多意味着处理时间也就越多，怎么利用更多的机器协同处理数据以降低平均等待时间，对算力、网络、分布式训练系统都是极大的挑战。

最后，对于大模型这种高计算型、高 I/O 型的应用场景而言，需要保证整个链路上的各个环节都是高性能的，任何一个部分的短板都将成为整个系统的短板 —— 木桶理论。

## 最终目标

以构建高性能、高可用、高可扩展性的 AI 集群为最终目标，形成一套快捷、简单、高效的指导方法论，帮助 AI 架构从业者基于实际需求快速搭建一套可交付生产的高性能 GPU 集群。

## 目录结构

先分别从算力、存储和网络三个维度深入分析其软硬件现状，在大模型场景下存在什么问题、有什么解决方案，最后站在整体的视角来探究如何构建一个高性能存算一体的 AI 集群。