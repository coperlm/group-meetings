zkSNARK（Zero-Knowledge Succinct Non-Interactive Argument of Knowledge）的“原生抗量子”特性要求其底层数学困难问题不能被Shor算法（用于分解大整数和求解离散对数）或Grover算法（用于搜索）有效破解。

传统的zkSNARK（如Groth16, Plonk, Marlin）大多基于椭圆曲线离散对数问题（ECDLP）或大整数分解，因此不抗量子。

近年来在**三大密（CRYPTO, EUROCRYPT, ASIACRYPT）和Big 4（S&P, CCS, USENIX Security, NDSS）**上发表的抗量子zkSNARK/Argument方案主要分为三大类：基于格（Lattice-based）、基于编码（Code-based）以及基于哈希/对称原语（Hash-based / Symmetric-based，包含STARKs和MPC-in-the-head）。

以下为您检索并整理的近几年（2021-2025）发表在顶会上的代表性抗量子方案：

1. 基于格（Lattice-Based）
这类方案通常基于LWE（Learning With Errors）或SIS（Short Integer Solution）问题，支持同态特性，但证明大小通常较大（KB到MB级别）。

Succinct Vector, Polynomial, and Functional Commitments from Lattices

会议: EUROCRYPT 2023

核心贡献: 提出了一种基于格的简洁承诺方案（Basis-Augmented SIS），可用于构建格基zkSNARK。该方案在证明向量和多项式关系时提供了比以往方案更优的渐进复杂度。

Lattice-Based Zero-Knowledge Proofs and Applications: Shorter, Simpler, and More General

会议: CRYPTO 2022

核心贡献: 改进了格基零知识证明的底层技术（如“One-Shot”证明），显著减小了证明大小，并使其更通用。这类技术是构建全功能格基zkSNARK的基础模块。

Shorter and Faster Post-Quantum Designated-Verifier zkSNARKs from Lattices

会议: CCS 2021

核心贡献: 提出了一种**指定验证者（Designated-Verifier）**模型的格基zkSNARK。虽然是非公开验证（verifier需要私钥），但该方案将证明大小压缩到了16KB左右（相比之前的MB级别有巨大提升），且验证速度很快。

MatRiCT+: More Efficient Post-Quantum Confidential Transactions (相关工作)

会议: CCS 2021 / ASIACRYPT 2023 (MatRiCT系列)

核心贡献: 虽然主要针对机密交易（RingCT），但其核心是一个高效的格基零知识证明系统，解决了由于格的“拒绝采样”导致的证明膨胀问题。

2. 基于编码（Code-Based）与 域无关（Field-Agnostic）
这类方案通常利用纠错码（如Reed-Solomon码或Tensor Codes）的性质。它们的优势是证明生成速度极快（线性时间），且通常是“域无关”的（Field-Agnostic），即不依赖特定椭圆曲线或素数域。

BaseFold: Efficient Field-Agnostic Polynomial Commitments from Foldable Codes

会议: CRYPTO 2024

核心贡献: 结合了FRI（STARK的核心）和折叠（Folding）技术，提出了一种新的多项式承诺方案。它基于纠错码，实现了抗量子安全性，且在证明大小和验证成本上达到了很好的平衡，是目前最先进的通用抗量子SNARK构建模块之一。

Brakedown: Linear-time and field-agnostic SNARKs for R1CS

会议: CRYPTO 2021

核心贡献: 提出了第一个证明生成时间为线性复杂度的SNARK方案。它基于线性纠错码（Linear Codes）和张量积，完全不使用昂贵的模幂运算或椭圆曲线乘法，天然抗量子。

Orion: Zero Knowledge Proof with Linear Prover Time

会议: CRYPTO 2022

核心贡献: 在Brakedown的基础上进一步优化，显著减小了证明大小（虽然仍比传统SNARK大，但已经进入实用范围），同时保持了极快的证明生成速度。

Field-Agnostic SNARKs from Expand-Accumulate Codes

会议: ASIACRYPT 2023

核心贡献: 引入了一类新的编码“Expand-Accumulate Codes”，用于构建更高效的域无关SNARK，进一步优化了证明大小和验证开销。

3. 基于哈希与MPC-in-the-Head (Hash-Based / STARKs)
这类方案安全性仅依赖于哈希函数的抗碰撞性（以及随机预言机模型），是目前最成熟的抗量子方向。STARKs属于此类。

HyperNova: Recursive Arguments for Customizable Constraint Systems (注：Folding本身通常不抗量子，但HyperNova结合了Sumcheck)

注意：许多Folding Scheme（如Nova, SuperNova）主要基于离散对数，不抗量子。但在CCS 2023/2024及后续工作中，研究者开始探索基于哈希（如FRI-based）的Folding方案（如LatticeFold或上述的BaseFold），请务必区分。

STIR: Reed-Solomon Proximity Testing with Fewer Queries

会议: CRYPTO 2024

核心贡献: 对STARKs的核心组件FRI进行了重大改进。STIR通过减少查询次数来减小证明大小，直接提升了所有基于STARK系统的抗量子SNARK的效率。

Ligero++: A New Optimized Sublinear IOP

会议: CCS 2020 (稍早，但在2021-2023仍有大量引用和改进)

核心贡献: 基于MPC-in-the-Head范式的轻量级zkArgument。后续的改进版本（如Limbo - CCS 2021）进一步优化了通信开销。

FAEST: From AES to Short Signatures (虽然是签名，但基于zk)

会议: CRYPTO 2023 / 2024

核心贡献: 使用了VOLE-in-the-head技术。这本质上是一种极高效的零知识证明技术，虽然主要用于NIST PQC签名竞赛，但其底层的ZK论证系统是通用的且抗量子的。

总结与建议
如果您寻找最短的证明大小，目前的抗量子方案（如STARKs或Lattice-based）通常在KB级别（10KB-100KB），远大于Groth16的200字节。

关注性能（Prover Time）: 推荐阅读 Brakedown (CRYPTO 21) 和 Orion (CRYPTO 22)。

关注前沿承诺方案: 推荐阅读 BaseFold (CRYPTO 24)，代表了编码与折叠技术的最新结合。

关注Lattice原生: 推荐阅读 CCS 2021 的指定验证者方案或 EUROCRYPT 2023 的格基承诺。

参考文献链接示例 (来自 SearchResults)
BaseFold: Efficient Field-Agnostic Polynomial Commitments from Foldable Codes
(此链接指向Crypto 2025/2024相关Code-based session，BaseFold是该领域近期重要工作)。

BaseFold 结合了纠错码和折叠方案，是当前通过编码理论实现高效抗量子多项式承诺的前沿代表。