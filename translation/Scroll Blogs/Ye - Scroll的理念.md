## 介绍

zkEVM 在过去两年内是一个非常流行的话题。它可以说已经成为了扩容以太坊的标准技术方案 —— 不仅在Layer 2实现，而且还直接在Layer 1 —— “最终将以太坊本身SNARK化”。我们从一开始就与隐私和扩容研究（PSE, Privacy and Scaling Explorations） 团队一起推动这个雄心勃勃的梦想，并致力于在未来继续共同构建它。

在本文中，我想分享我们在构建 zkEVM 时吸取的一些经验教训，以及我们如何考虑不同的权衡取舍。我们采用了与生态中其他项目不同的方案，这使得我们处在一个独一无二的地位。

## 通过社区驱动的方式进行开发

Scroll 根本上是由开源方案支持的。我们正在使用Zcash 的[证明堆栈](https://github.com/zcash/halo2)，并且从第一天起就与[PSE 团队](https://twitter.com/PrivacyScaling)共同构建[zkEVM 电路。](https://github.com/privacy-scaling-explorations/zkevm-circuits)我们感谢社区的努力和所有正在构建的工具。本着开源精神，我们的一个重要理念是尽可能多地回馈社区，并以更开放协作的方式与社区继续构建。这使我们的价值观有别于其他项目。更具体地说，我们做了以下几件事，使 Scroll 的开发由社区驱动：

-   **面向广大受众的公共教育**。为了帮助人们了解我们的架构，我们在全球范围内进行了多次演讲和举办活动。你可以在[Devconnect](https://twitter.com/Scroll_ZKP/status/1521677531438628864)、[SBC](https://www.youtube.com/playlist?list=PLrzRr7okCcmauITgMFrrXdxE-QQ75-i4_)、[Devcon](https://www.youtube.com/playlist?list=PLrzRr7okCcmZKUhBNRXmK5vWmJMlQZzzD) 等找到我们。对于想要更深入学习的同学，我们在 0xPARC 举办了关于我们证明堆栈的[讲座](http://learn.0xparc.org/materials/halo2/learning-group-1/cost-model)，并在 [斯坦福](https://www.youtube.com/watch?v=1bVe77-yfBA&list=PLhA8D_GhTLc5MR5aecbSfL6wvqGS8IOqK)和[伯克利](https://www.youtube.com/watch?v=Ct6H5GcnA0A&t=2395s)也举办了关于我们研究成果的演讲。对于审计人员，我们主持了关于我们代码库的[审计人员会议](https://www.youtube.com/playlist?list=PLrzRr7okCcmZmDrVozX5hhBQlsrpZdsij)。我们还经常为通用零知识证明和以太坊社区创作教育资源 —— 我们举办了每周更新的[零知识证明应用研究系列](https://youtube.com/playlist?list=PLrzRr7okCcmbAlgYpuFjzUJv8tAyowDQY)，并分享了有关[零知识证明技术](https://scroll.io/blog/proofGeneration)和[以太坊](https://scroll.io/blog/kzg)的技术博客。
-   **与社区共同发展**。从第一天起，我们的 zkEVM 就是完全由社区驱动的方式开发的。除了我们的团队和 PSE 团队，还有多个社区成员为 zkEVM 的不同部分做出了贡献（例如，许多操作码电路已经由不同的社区成员并行实现，并且对 keccak 电路和 snark 验证者都进行了不俗的优化）。我们还领导了每两周一次的社区电话会议，以改进底层的[验证堆栈](https://github.com/halo2-ce)。我们已经取得了一些惊人的进展 —— 例如，Halo2 现在支持[Goldilocks](https://github.com/halo2-ce/halo2/pull/11)和[FRI](https://github.com/maxgillett/halo2-fri-gadget)通过社区努力打下的坚实基础可以实现共享安全和共享审计！

通过社区驱动的方式进行构建的好处是显而易见的。我们可以与一群人一起集思广益，获得更多创意。它也可以说更安全，因为每个PR都会得到其他社区成员的更多评论。一些公共部分甚至可以跨项目共享 —— 例如，[Axiom](https://twitter.com/axiom_xyz)实现了[配对电路](https://github.com/axiom-crypto/halo2-lib)，这是zkEVM 预编译最难的部分之一。

但是，在开放环境中进行构建当然也有所取舍。在一群人中进行协调变得更加困难（不仅在沟通方面，而且在优先级方面）。它会放缓开发速度，因为许多PR需要审查并且合并的标准因此也有所提高。

我们的 zkEVM 的独特之处在于我们还在维护一个[python](https://github.com/privacy-scaling-explorations/zkevm-specs)版本的规范，类似于以太坊一直在为其[consensus-spec](https://github.com/ethereum/consensus-specs)和[execution-spec](https://github.com/ethereum/execution-specs)所做的那样。维护这个规范可以让不熟悉 Rust 和 Halo2 的人理解电路逻辑。据我所知，没有其他 zkEVM 实现会花时间来做这件事，因此他们都为了可以更快地发布主网。

**在 Scroll，我们采用这种社区驱动的方式来开发整个 zkEVM。我们相信正确的方式就是从一开始就与社区一起建设。** 请注意，“社区驱动的开发”不仅仅意味着开源。这并不意味着私下构建然后某天突然开源所有代码。它应该通过有多少[外部贡献者](https://github.com/privacy-scaling-explorations/zkevm-circuits/graphs/contributors)，以及项目是如何随着时间的推移而发展来衡量。我们接受在早期阶段速度较慢的取舍，但随着我们的社区不断扩大，我们相信后期社区驱动开发的力量。

以太坊采用了类似的策略来实现其愿景和价值，称为“[做减法](https://www.youtube.com/watch?v=noXPewi5qOk)”。这个想法很简单——他们不是自己构建所有东西，而是尽可能地支持社区。它帮助他们寻求正确的平衡，并专注于对他们真正重要的事情。我们正在做完全相同的事情。我们会问自己，“我们可以为社区提供什么样的支持来帮助他们开发？“ 我们相信，我们以社区为导向的方式将使我们在该领域取得独一无二的地位。

**这种理念使我们有别于其他竞争对手，后者拥有大量人员构建多个内部解决方案，并且在各个方向疯狂营销。而我们只专注于交付最重要的部分并引领正确的方向。**

## 确保安全性并稳定发布

与其他alt L1相比，安全性是人们相信Layer 2的最大原因——你可以从以太坊继承安全性，而无需信任Layer 2的运营者。但所有现有的Layer 2项目仍远未达到该标准，具有不同程度的[辅助轮](https://ethereum-magicians.org/t/proposed-milestones-for-rollups-taking-off-training-wheels/11571)。例如，对于 optimistic rollups，即使许多现在已经在主网上线，他们仍然需要可升级的密钥并且不支持无许可的欺诈证明。

**对于 zkEVM，这也是一个大问题 —— 每个玩家都在进行一场漫长的比赛，无论他们选择如何实现主网，都需要多次迭代。** 一些关于 zkEVMs 的基本问题还没有解决。例如，证明者成本将不同于执行成本，这将影响Layer 2的gas定价或引入安全漏洞。

我们一直在考虑安全问题，并努力做出最佳决策。我在下面列出了其中一些决定：

-   **采用 EVM 等效的方案**。采用这种方案的一个重要原因是它带来了更好的开发者体验，但另一个更深层次的原因是它继承了已经经过实战检验的 EVM 模型的安全性。我们也在重用像[Geth](https://github.com/ethereum/go-ethereum)这样的基础设施，以尽量减少我们与以太坊的差异。这确保了我们 Layer 2 的排序器与 Layer 1 节点的行为完全相同，从而最大限度地提高了安全性。
-   **面向社区的方式**。正如我之前所说的，从外部的社区审阅者那里获得审阅可以为代码安全提供更有力的保证。工具和证明堆栈也在项目之间共享，因此我们对当前代码库的关注度更高。一个好的衡量标准应该是“有多少人熟悉你的代码库？”，“有多少人真的在使用它？”
-   **审计，审计，再审计。测试，测试，再测试**。我们举办了审计人员会议，向审核人员介绍我们的开发堆栈，并为我们的 zkEVM 电路聘请了业内最好的审计人员，但外部审计是不够的。在内部，我们集成了标准的 EVM 测试向量，并进行了大量模拟来证明主网区块。除此之外，我们还提供资助来支持探索我们 zkEVM 的形式验证和模糊测试等。
-   **建立内部安全团队**。我们与众不同的一点在于我们拥有一支非常强大的内部安全团队来密切关注我们的代码库。最佳方案不仅仅是通过审计人员进行审计，而是让一个全职团队关注我们的代码安全。除了我们自己的安全，我们也在帮助其他项目。例如，我们发现了Aztec 中的[电路错误](https://twitter.com/Scroll_ZKP/status/1454403921531703296)和针对 Consensys 的 zkEVM 证明者的[安全攻击](https://eprint.iacr.org/2022/1754)。
-   **关于辅助轮的研究**。在内部，我们一直在围绕移除辅助轮的多种解决方案以及如何确保我们的 zkEVM 的[2FA](https://ethresear.ch/t/2fa-zk-rollups-using-sgx/14462)或 3FA 进行研究。我们相信这是最重要的事情，尽管它比拥有更多花哨新功能的营销声量要小很多。随着我们取得更多进展，我们将一如既往分享我们的成果并与社区进行讨论。

**为了保持高标准的安全性，我们选择让每个版本都更加稳定，并迭代现有版本以不断提高稳健性和性能。** 我们将使[Twitter每周更新](https://twitter.com/Scroll_ZKP/status/1621573571259793408)中的所有内容更加透明。

我们安全第一的理念是我们的路线图的第一决定因素。它可以帮助我们决定应该走哪条方向，同时还可以回答以下问题：

-   “我们现在应该目标 EVM 等价还是以太坊等价？”
-   “我们应该先去中心化证明者还是排序器？”
-   “我们应该继续添加新功能还是专注于移除辅助轮？”

我将在后续帖子中逐一回答这些问题。

## 每一层的去中心化都很重要

回想以太坊的历史，以及为什么人们认为它是[可信的中立的](https://nakamoto.com/credible-neutrality/)。这不仅仅是因为先进的技术，还因为取得今天的成就它所走过的道路。以太坊在每一层都是去中心化的（决策透明度、社会共识等）。同样，我们定义了多个不同层的去中心化目标，并为自己设定了极高的标准：

- **去中心化证明者**。我们是第一个提出[去中心化证明网络](https://scroll.io/blog/architecture)想法的。这是我们将实现的第一个技术路线，以实现完全去中心化并确保高可靠性。一个优化目标是[降低证明成本](https://twitter.com/Scroll_ZKP/status/1621573597566468096)，这将使更多的人可以运行证明者，进一步去中心化。我们正在有意识地努力避免“最快的证明者永远获胜”的困境，这样人们就不需要依赖昂贵的定制硬件来参与我们的网络。
- **去中心化排序器**。去中心化排序器是另一个有助于抗审查的重要步骤，我们致力于此。我们有多个关于如何实现这一目标的内部提案，并将很快公开这些想法以进行更广泛的讨论。我们想要首先去中心化证明者的原因有很多（例如，对没有bug的 zkEVM 的担忧，以及证明者和排序起之间更复杂的交互和激励）。从长远来看，我们正在考虑如何在协议级别上，与以太坊在排序器方面保持一致。
- **发展和治理**。zkEVM 的开发是通过开源贡献者社区，以去中心化的方式进行的。我们通过 zkEVM 和证明者社区电话会与他们协调。随着我们进一步的开发，我们将使开发和治理越来越透明（包括开发决策过程，类似于以太坊的[核心开发者会议](https://github.com/ethereum/pm)）。
- **生态系统和社区**。遵循以太坊“[Infinite Garden](https://ethereum.foundation/infinitegarden)”的愿景，我们希望支持我们的生态系统和社区的持续增长。因此，我们将尽量减少与具体个别项目的“伙伴关系”，而是站在更中立的立场上支持所有基础建设。我们不是在营销方面考量，而是在消息传递和沟通方面进行思考。我们问自己，“我们怎样才能对我们的社区更加透明？” 我们相信，这种方法是创建更加去中心化的生态系统和鼓励创造力的最佳方式。
- **社会和文化多样性**。除了技术和生态系统，我们的目标是在社会和文化层面实现另一个层面的去中心化。我们的团队分布在多个大洲（亚洲、欧洲、北美、南美、澳大利亚）。你几乎可以在世界任何地方找到 Scroll 团队成员，这使我们能够建立当地分布的社区。我们正在与文化多样性一起成长，以获得更深层次的社会共识。

## 不仅为 Scroll 打造，也为以太坊打造

在构建扩容解决方案时，我们与以太坊保持高度一致。以太坊有一个雄心勃勃的最终目标，即“zk-SNARK everything” —— 构建一个以太坊等效的 zkEVM，可用于证明主网区块。想象有一天，验证者不需要重新执行 Layer 1 区块，而是只需要验证一个简洁的零知识证明。想象有一天，你可以通过一个证明来验证以太坊的整个历史。是不是超级刺激？

这正是 PSE 团队的目标！作为在同一代码库上开发了大约 2 年的共同构建者，我们正在直接推动这一雄心勃勃的目标。

现在已经提出了一些标准来对[不同类型的 zkEVM 进行分类](https://vitalik.ca/general/2022/08/04/zkevm.html)。然而，它更像是一个更高层面的规范，描述了最终结果应该是怎样。**作为推动以太坊等效 zkEVM 的主要贡献者之一，我们想提出一些不同的东西来区分目标和实现目标的实际路径。** 如下是我们想实现 SANRK 以太坊的道路：

- 使用可靠的零知识证明实现字节码级别兼容的 zkEVM
- 推出相关举措，协调 Layer 1 和 zkEVM 的发展
- 达成社区标准并提出 EIP 来改进以太坊以实现最终目标

目前，我们正处于推出产品完备的字节码兼容 zkEVM 的第一阶段，我们致力于与整个社区一起建设以太坊的未来。构建一个高性能和足够安全的以太坊等效的 zkEVM 可能需要数年时间，涉及证明系统升级、新电路设计以及软件和硬件加速方面的创新。但更重要的是，要在Layer 1层面采用它，以太坊本身必须做出一些改变。在实现最终目标之前，以太坊的所有重要升级都需要考虑 zkEVM。

目前主流的想法是 Layer 2 单向适配 Layer 1 的变化。然而，随着 rollups 的成熟，我们认为情况不应再如此。Rollups 应该在推动 Layer 1的变化方面发挥作用，并且 Rollup 团队应该在Layer 1的基础设施方面发挥更重要的作用（[4844](https://www.eip4844.com/)、[Danksharing](https://notes.ethereum.org/@dankrad/new_sharding)、[Verkle 树](https://vitalik.ca/general/2021/06/18/verkle.html)、[PBS](https://notes.ethereum.org/@domothy/pbs_links)、[1559](https://ethresear.ch/t/multidimensional-eip-1559/11651)等）。我们需要注意向后兼容性的影响，但历史包袱不应该限制未来发展。整个生态系统应该协调一致，打造一个更好的以太坊。

## 结论

我们从一开始就采用社区驱动的方式来开发 zkEVM，并致力于以更协作的方式继续构建并回馈社区。我们非常重视安全性和去中心化，我们正在专注于实现这些目标的具体方案。出于我们的安全性理念，我们为自己设定了一个高标准，让每个版本都更加安全和稳定。出于我们的去中心化理念，我们正在追求所有不同层面的去中心化，包括我们的技术堆栈、开发过程、生态系统、社区和社会多样性。

我们希望尽可能地推动区块链的开放性和抗审查性。**我们选择了一条从一开始就独一无二的道路。我们已经将自己定位在不仅要构建Layer 2，还要推动实现 SNARK 整个以太坊的雄心勃勃的目标。我们的理念是，像以太坊一样运作，致力于 infinite garden 的同一个未来！**