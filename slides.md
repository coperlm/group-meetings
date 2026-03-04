---
theme: frankfurt
title: Shorter, Tighter, FAESTer VOLEitH
author: Tianyi Wang
date: 2026-02-22
class: text-center
transition: slide-left
mdc: true
fonts:
  sans: '"Times New Roman", "Microsoft YaHei", sans-serif'
  serif: '"Times New Roman", "Microsoft YaHei", serif'
  mono: '"Fira Code", "Microsoft YaHei", monospace'
---

# Shorter, Tighter, FAESTer VOLEitH

## Shorter, Tighter, FAESTer: Optimizations and Improved (QROM) <br> Analysis for VOLE-in-the-Head Signatures

### authors: Carsten Baum , Ward Beullens , Lennart Braun etc.

### presenter: Tianyi Wang

### 2026-02-22

---
section: highlight
---

# 主要内容

CRYTPO2023提出VOLEitH实现基于VOLE的ZKP公开可验证，并构造了FAEST（对AES进行VOLEitH的ZKP，实现后量子签名，安全性依赖于AES的安全性）

亚密2024提出BAVC优化了数据结构，把每个门的GGM树合成为一个大树，优化性能

本文(CRYTPO2025) 实现对AES的进一步优化，并且目前是NIST后量子签名候选方案


> reference: [1] [2] [3]

---
# 后量子签名方案对比

签名速度，验证速度，签名大小，安全性

基于格 ，基于编码，本文方案

---
section: Preliminaries
---

# VOLE

P持有 $u,m$ ， V持有 $\Delta,v$，满足：

$$
m = u\cdot \Delta + v
$$

对于每个witness $w$，P发送 $d=w-u$，V更新 $m'=m+d\cdot \Delta$（加法门是免费的）

对于乘法门，P计算多项式 ，V检查在$\Delta$处多项式是否符合条件

例如quicksilver：

例如JesseQ：

---

# VOLEitH

Prover commit first

then calculate $\Delta$ by fait shamir

VOLE是非指定验证者的，想公开验证，先证明后用证明结果的Fait-Shamir生成$\Delta$


---

# GGM Tree

construct N-out-of-N-1 OT

把之前的图拿过来

用于打开N个数值里面的N-1个数值（OT协议）

---

# BAVC

**Batch All-but-One Vector Commitment**

alter many small trees by a large tree

这部分需要图解

---
section: FAESTer
---

# From Hash to PRG

construct a GGM Tree needs quiet a lot **hash function**(such as SHAKE, SHA-3)

solution: alter hash function by **AES-CTR (PRG)**

---
section: Shorter
---

# construct $c\cdot a^2\cdot a^{16}=a$

odd and even

通信量 8 8 -> 4 8

---
section: Tighter
---

# QROM Proofs

Lossy Keys

证明思路从“提取私钥”转变为“区分公钥”。在归约中，将真实的公钥替换为一个没有对应私钥的“无效公钥”。攻击者如果还能伪造签名，就打破了底层证明系统的可靠性（Soundness），而不是知识可靠性（Knowledge Soundness）。这避免了在 QROM 下构造复杂的知识提取器，从而获得了更紧致的安全界

---
section: my idea
---

# 国密

- AES -> SM4 <br> SHA-3 -> SM3
- 代码直接在faest的官方代码的基础上进行修改即可

---

# AES -> SM4 

- ZKP部分（已完成）
- 把AES-CTR (PRG)替换为SM4（已完成）
- 待完成：degree-3优化（SM4结构不一样，应使用1:3结构，但是这样优化的收益不多，具体而言AES是通讯减半，SM4是 8 8 8 8 变为 4 8 8 8 ，故写入未来工作里，代码不写了）
- 纯软件实现，SM4比AES快（应该是AES的S-box实现依赖于openssl，不开openssl的时候PRG用的和zkp是一套函数导致变慢，实际上SM4的确比不过AES）

---

# SHA-3 -> SM3

- 可拓展性 -> SHA3为SHAKE结构，输出长度可变，但SM3固定 -> 解决方案：使用HMAC（待完成）