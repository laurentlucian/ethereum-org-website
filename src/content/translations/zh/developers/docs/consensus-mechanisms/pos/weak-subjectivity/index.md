---
title: 弱主观性
description: 关于弱主观性及其在权益证明版以太坊中的作用。
lang: zh
sidebar: true
---

区块链的主观性是指对社会信息的依赖，以就当前状态达成一致。 根据从网络上其他对等点收集到的信息，会有多个有效分叉供选择。 与主观性相对的是客观性，后者指只有一个可能有效的链，必须应用编码规则才能使所有节点达成一致。 还有第三种状态，称为弱主观性， 是指检索完社会信息的一些初始来源后，可以取得客观进展的区块链。

## 前提条件 {#prerequisites}

理解此页必须首先理解[权益证明](/developers/docs/consensus-mechanisms/pos/)的基础知识。

## 弱主观性解决了哪些问题？ {#problems-ws-solves}

主观性是权益证明区块链所固有的，因为从多个分叉中选择正确的链是通过计算历史投票来完成的。 这使区块链暴露在多个攻击途径下，包括以下类型的长期攻击：很早就参与了区块链的节点维护一个替代分叉，并会在稍后因自己的利益释放此分叉。 另外，如果 33% 的验证者取出他们的质押物，但仍继续证明和生产区块，他们可能会生成一个与规范链相冲突的替代分叉。 新的节点或已经离线很长时间的节点可能不知道这些攻击验证者已经取出了他们的资金，所以攻击者可能会诱骗他们跟随不正确的链。 以太坊可以通过施加限制来解决这些攻击途径，因为这些限制可将机制的主观方面（也就是信任假设）降至最低。

## 弱主观性检查点 {#ws-checkpoints}

在权益证明版以太坊中，弱主观性的实现要用到“弱主观性检查点”， 即网络上所有节点都同意加入规范链的状态根。 它们的作用与创世区块的"普遍事实"相同，只是它们不在区块链的创世块位置上。 分叉选择算法相信该检查点中确定的区块链状态是正确的，并且独立和客观地验证了从该点开始的区块链。 检查点起到了"回滚限制"的作用，因为位于弱主观性检查点之前的区块不能改变。 如此，只需将远程分叉作为机制设计的一部分界定为无效，就能破坏远程攻击。 确保弱主观性检查点之间相隔的距离小于验证者提款期，这样便可保证分叉链的验证者在提取质押前至少被罚没一些阈值量，且新验证者不能被已经提取质押的验证者骗到错误的分叉上。

## 弱主观性检查点和最终确定区块的区别 {#difference-between-ws-and-finalized-blocks}

以太坊节点对最终确定区块和弱主观性检查点的处理方式不同。 如果一个节点意识到有两个相互竞争的最终确定区块，它就会在这两个区块之间纠结，因为它没有办法自动识别哪个是规范分叉。 这是共识失败的表现。 相反，节点只会拒绝任何与其弱主观性检查点有冲突的区块。 从节点的角度来看，弱主观性检查点代表了一个绝对的真理，即不能被其对等点的新信息所破坏。

## 多弱为弱？ {#how-weak-is-weak}

以太坊权益证明的主观方面要求从可信的同步来源获得最新状态（弱主观性检查点）。 得到坏的弱主观性检查点的风险非常低，因为它们可以通过对照多个独立的公开来源（如区块浏览器或多个节点）进行检查。 然而，运行任何软件应用程序总是需要一定程度的信任，例如，相信软件开发人员开发的是诚实的软件。

一个弱主观性检查点甚至可以成为客户端软件的一部分。 可以说，攻击者可以破坏软件中的检查点，就可以同样容易地破坏软件本身。 在这个问题上没有真正的加密经济路线，但在以太坊中，不可信开发者的影响得到了最小化，因为有多个独立的客户端团队，每个团队都在用不同的语言构建同等的软件，他们都是维护诚信链的既得利益者。 区块浏览器也可以提供弱主观性检查点，或者提供一种方法，将从其他地方获得的检查点与其他来源进行交叉对比。

最终，可以从其他节点请求检查点；也许另一个运行完整节点的以太坊用户可以提供一个检查点，然后验证者可以根据区块浏览器的数据进行验证。 总的来说，信任弱主观性检查点的提供者和信任客户端开发人员可视为同样的问题， 即所需的整体信任度很低。 需要注意的是，这些考虑只有在大多数验证者密谋生成另一个区块链分叉这种不太可能的情况下才变得重要。 在任何其他情况下，只有一条以太坊链可以选择。

## 延伸阅读 {#further-reading}

- [以太坊 2 中的弱主观性](https://notes.ethereum.org/@adiasg/weak-subjectvity-eth2)
- [Vitalik：我如何爱上弱主观性](https://blog.ethereum.org/2014/11/25/proof-stake-learned-love-weak-subjectivity/)
- [弱主观性（Teku 文档）](https://docs.teku.consensys.net/en/latest/Concepts/Weak-Subjectivity/)
- [阶段 0 的弱主观性指南](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/weak-subjectivity.md)
- [以太坊 2.0 的弱主观性分析](https://github.com/runtimeverification/beacon-chain-verification/blob/master/weak-subjectivity/weak-subjectivity-analysis.pdf)
