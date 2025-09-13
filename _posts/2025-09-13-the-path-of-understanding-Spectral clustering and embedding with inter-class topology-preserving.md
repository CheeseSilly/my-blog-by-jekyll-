---
title: Note of Spectral clustering and embedding with inter-class topology-preserving
author: Sillycheese
date: 2025/9/13 09:42:22 +0800
categories:
  - Paper
math: true
---
## Abstract

Spectral clustering and embedding rely heavily on **pre-computed graph structure**  
**quality**.

Some methods(for high-quality graph structures with separable categories): simultaneously learn **graph structures** and **low-dimensional embeddings**

- inter-class separability
- inter-class topological structure.(overlooked)

causing issues: causing the learned graph structures to **represent categories partitioning inaccurately**.

solutions: inter-class topology and utilize **category anchors** to indicate class locations in low-dimensional space.

makes the **neighbor classes rationally separable** and helps to learn better **low-dimensional embedding representations**.

result: preserve inter-class topology while making categories separable.

ensure intra-class sample compactness in low-dimensional space by  
**assigning sample embeddings near the anchors**

## Introduction

For unsupervised data, existing methods obtain separable class information  
by mining the data graph structure.

way: Generally, these methods  construct a **data similarity matrix** and then embed the original space into the low-dimensional space **based on the constructed graph
structure**

Spectral graph theory

 In the latent data manifold,  samples from similar categories are expected to exhibit similarity in  the embedding space. Therefore, preserving inter-class topology within
 the embeddings is crucial for accurately depicting the data and capturing its underlying structure.

the embedding representation learned by existing methods:
- contradict this assumption
- presenting challenges in retaining intricate inter-class relationships within the data manifold
- potentially resulting in suboptimal clustering outcomes

**propose an inter-class topology-preserving approach to capture the structural relationships between classes.**

By incorporating inter-class semantic information into clustering.we can  
generate embeddings that align with the latent manifold distribution  
of the data, enhancing the rational separation of neighboring classes  
through **meaningful interactions** between categories.

Our approach,  
which focuses on **inter-class topology preservation**, offers a comprehensive understanding of **relationships among diverse classes across**  
**various domains.**

Existing method:
- extract category separable information from graph structure, which is subsequently used to drive the low-dimensional embedding
- After learning low-dimensional representations and graph structures iteratively, the neighbor classes can in some sense well separated from each other
- While the learned **low-dimensional representations may not properly reflect the structure of real manifolds without preserving inter-class topology structure**, resulting in mining inexact graph structures.

Recap: only focus on separability yet topology relationship -> not properly reflect the structure of real manifolds -> inexact graph structures.

Intuitively, this approach may result in **neighbor categories overlapping**  
**in low-dimensional space**, making the categories more inseparable.

This paper introduces **inter-class topology-preserving approaches for clustering and unsupervised low-dimensional embedding learning**.

- On the one hand, we ensure that the **topology of categories in  low-dimensional space is consistent with the real topology** through inter-class topology preservation
- thus solving the problem that **existing methods cannot mine the reliable manifold structure**

On the other hand, we make intra-class samples more compact, further ensuring  topological inter-class separability. Here, we briefly summarize the  contributions of this paper as follows:

1. We demonstrate the significance of preserving inter-class topology and intra-class compactness in learning the graph structure  and low-dimensional embedding representations. We propose  **the Inter-class Topology-Preserving Clustering method (ITPC)**  and its variants, which improve inter-class separability and faith-fully capture inter-class topology, restoring the data manifold.
2. **An iterative optimization algorithm** is designed to solve our  proposed methods, and a theoretical analysis of convergence and complexity is given.
3. Extensive experiments show that our methods outperform state-  of-the-art models, demonstrating their effectiveness in learn-  ing graph structure and low-dimensional embedding represen-tations.

## Related works

### Shallow spectral clustering and embedding

these methods(like a history of methods for embedding?) compute  
the graph structure in the original space and utilize fixed graph struc-  
ture to learn embeddings. However, **noise and irrelevant features** may  
affect these computed graphs, subsequently impacting clustering and  
embedding performance.

Recap: **immutable graph structure**

To address the above issues, another line of research incorporates  
the **graph structure as a learnable variable**.

due to learning **low-dimensional representations and graph structures simultaneously**.

Our proposed meth-  ods are constructed based on the principles of graph structure learning,  thereby inheriting the advantages of improved inter-class separabil-ity.

in several significant aspects, our approach surpasses  
and differs from the previously mentioned graph learning methods.

- existing spectral clustering methods overlook the topological relationships between different categories.Consequently, the embed-  ding representations learned by these methods may not accurately  depict the relative positions between categories, potentially affecting  **category classification**
- guides graph structure learning by carefully preserving inter-class relationships, thereby facilitating  the discrimination of samples from neighboring classes and capturing  more stable manifold structures.
- incorporates  the learning of intra-class compactness to ensure compact intra-class  embeddings, distinguishing it from existing methods that might lead to  scattered embeddings and category overlaps

- incorporates  the learning of intra-class compactness to ensure compact intra-class  embeddings, distinguishing it from existing methods that might lead to  scattered embeddings and category overlaps.

### Deep spectral clustering and embedding

Recently, a category of clustering algorithms augmented with **deep  learning techniques** has emerged, showcasing notable efficacy in performance

Consequently, these  methods can reveal complex information that might be overlooked  by traditional shallow-based spectral clustering algorithms . However, this advantage comes at the **cost of substantial data and computational resource requirements**, which can be a limiting factor for many applications.

Conversely, **shallow methods** retain value in terms  of interpretability and performance enhancement when dealing with  **constrained datasets where deep learning’s resource demands can be  impractical**

**strikes a balance by leveraging the advantages of both approaches.**

## Proposed method

### Notations and definitions


$$
\min_{S, W} \;
\sum_{i,j} S_{ij} \, \| z_i - z_j \|_2^2 \; + \; \alpha \, \| S \|_F^2
$$


$$
S \mathbf{1}_n = \mathbf{1}_n, \quad 
S \geq 0, \quad 
\mathrm{rank}(LS) = n-r, \quad 
\Omega(W)
$$

### Inter-class topology preserving

