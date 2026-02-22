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
section: Preliminaries
---

# VOLE

$$
m = u\cdot \Delta + v
$$


---

# VOLEitH

Prover commit first

then calculate $\Delta$ by fait shamir



---

# GGM Tree

construct N-out-of-N-1 OT

---

# BAVC

**Batch All-but-One Vector Commitment**

alter many small trees by a large tree

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

---
section: Tighter
---

# QROM Proofs

Lossy Keys

证明思路从“提取私钥”转变为“区分公钥”。在归约中，将真实的公钥替换为一个没有对应私钥的“无效公钥”。攻击者如果还能伪造签名，就打破了底层证明系统的可靠性（Soundness），而不是知识可靠性（Knowledge Soundness）。这避免了在 QROM 下构造复杂的知识提取器，从而获得了更紧致的安全界

---
section: summary
---

# my idea

- AES -> SM4 <br> SHA-3 -> SM3
