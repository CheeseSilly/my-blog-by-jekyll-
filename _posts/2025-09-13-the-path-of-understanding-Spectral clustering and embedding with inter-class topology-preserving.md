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
\sum_{i,j} S_{ij} \, \| z_i - z_j \|_2^2 \; + \; \alpha \, \| S \|_F^2 \tag{1}
$$


$$
S \mathbf{1}_n = \mathbf{1}_n, \quad 
S \geq 0, \quad 
\mathrm{rank}(LS) = n-r, \quad 
\Omega(W)
$$

### Inter-class topology preserving

To measure the relationship between categories, we propose **category anchors** to represent category locations.

𝐇 =$[𝐡1, ... , 𝐡𝑟]$ is the latent representation of classes in low-dimensional space.

Inspired by the **reversed graph embedding assumption**, we give the following inter-class topology assumption.

if $C_i$ and $C_j$ are closely related, then the positions of the corresponding  category anchors $𝐡_𝑖$ and $𝐡_𝑗$ in the low-dimensional space should be  close.

$$
\min_{H} \; \sum_{i,j}^r R_{ij} \, \mathrm{dis}(h_i, h_j) \tag{4}
$$
we use Euclidean distance $𝑑𝑖𝑠(𝐡_𝑖, 𝐡_𝑗 ) = ‖𝐡_𝑖 − 𝐡_𝑗 ‖_2 ^2$

$$
\min_{\mathbf{H}} \sum_{ij} R_{ij} \| \mathbf{h}_i - \mathbf{h}_j \|_2^2    \tag{5}
$$

For class $C_i$, we obtain its inter-class topology by solving the following problem

$$
\begin{aligned}
\min_{\mathbf{R}_i} \quad & \sum_{j=1}^{r} R_{ij} \| \mathbf{h}_i - \mathbf{h}_j \|_2^2 + \gamma R_{ij}^2 \\
\text{s.t.} \quad & \sum_{j=1}^{r} R_{ij} = 1, \\
& \mathbf{R}_i \ge \mathbf{0}, \\
& R_{ii} = 0
\end{aligned}

\tag{6}
$$

$$
\begin{aligned}
\min_{\mathbf{R}} \quad & \sum_{ij}^r R_{ij} \| \mathbf{h}_i - \mathbf{h}_j \|_2^2 + \gamma R_{ij}^2 \\
\text{s.t.} \quad & \mathbf{R} \mathbf{1} = \mathbf{1}, \quad \mathbf{R} \ge \mathbf{0}, \quad \mathrm{diag}(\mathbf{R}) = \mathbf{0}, \quad \mathbf{R} = \mathbf{R}^T
\end{aligned}
\tag{7}
$$

Recap: alternative optimization
### Intra-class compactness learning

After learning the category anchors, we **assign samples near anchors**  o obtain a **compact intra-class distribution**.

$$
\begin{aligned}
\min_{\mathbf{F},\mathbf{Z}} \quad & \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{z}_i - \mathbf{h}_l \|_2^2 \\
\text{s.t.} \quad & \mathbf{F} \mathbf{1}_r = \mathbf{1}_n, \quad \mathbf{F} \ge \mathbf{0}
\end{aligned}
\quad (8)

\tag{8}
$$

Then, the connections between **intra-class samples and their corresponding**  
**class anchors** are established through the matrix $𝐅$.

Combining the problem (8) and graph structure learning (1),we have the following optimization problem:

$$
\begin{aligned}
\min_{\mathbf{S},\mathbf{F},\mathbf{W}} \quad & \sum_{ij=1}^{n} (S_{ij} \| \mathbf{z}_i - \mathbf{z}_j \|_2^2 + \alpha S_{ij}^2) + \delta \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{z}_i - \mathbf{h}_l \|_2^2 \\
\text{s.t.} \quad & \mathbf{S} \mathbf{1}_n = \mathbf{1}_n, \quad \mathbf{S} \ge \mathbf{0}, \\
& \mathrm{rank}(\mathbf{L}_{\mathbf{S}}) = n - r, \\
& \mathbf{W}^T \mathbf{W} = \mathbf{I}_k, \\
& \mathbf{F} \mathbf{1}_r = \mathbf{1}_n, \quad \mathbf{F} \ge \mathbf{0}
\end{aligned}
\tag{9}
$$

Since the rank constraint is challenging to solve, we use **Ky Fan’s Theorem**  to  
convert the rank constraint into the following easy-to-solve way.

$$
\begin{aligned}
\min_{\mathbf{S},\mathbf{F},\mathbf{W}} \quad & \sum_{ij=1}^{n} (S_{ij} \| \mathbf{z}_i - \mathbf{z}_j \|_2^2 + \alpha S_{ij}^2) + \delta \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{z}_i - \mathbf{h}_l \|_2^2 + 2\beta \mathrm{Tr}(\mathbf{F}^T \mathbf{L}_{\mathbf{S}} \mathbf{F}) \\
\text{s.t.} \quad & \mathbf{S} \mathbf{1}_n = \mathbf{1}_n, \quad \mathbf{S} \ge \mathbf{0}, \\
& \mathbf{W}^T \mathbf{W} = \mathbf{I}_k, \\
& \mathbf{F}^T \mathbf{F} = \mathbf{I}_r, \quad \mathbf{F} \ge \mathbf{0}
\end{aligned}
\tag{10}
$$

the above thing converts $rank(L_S) = n - r$ to $+2βTr(F^T L_S F)$

every row of F is one-hot 

$$
\| \mathbf{W}^T \mu_s - \mathbf{W}^T \mu_t \|_2^2 = \| \mathbf{W}^T (\mu_s - \mu_t) \|_2^2 \tag{11}
$$


$$
\begin{aligned}
\| \hat{\mathbf{W}}^T (\mu_s - \mu_t) \|_2^2 &= \| \mathbf{W}^T (\mu_s - \mu_t) \|_2^2 + \| \mathbf{W}_{\perp}^T (\mu_s - \mu_t) \|_2^2 \\
& \ge \| \mathbf{W}^T (\mu_s - \mu_t) \|_2^2
\end{aligned}
\tag{12}
$$

$$
\| \mu_s - \mu_t \|_2^2 \ge \| \mathbf{W}^T (\mu_s - \mu_t) \|_2^2 \tag{13}
$$



which shows that inter-class distances after projection are less than or  equal to in the original space, concluding the proof.

orthogonal transformation from high-  dimensional to low-dimensional space makes the inter-class distance  smaller while the intra-class variance remains unchanged under some  scenarios, which may lead to the **overlapping of neighbor classes in low-dimensional space**.

In contrast, the problem (8) aggregates  **samples of the same class near the class anchors** and learns the accurate clustering structure to eliminate the issues caused orthogonal  
constraint.

We can simultaneously learn category-separable embeddings and  graph structure by optimizing the problem (10).

three advantages

- a sample can only be assigned to one category  under the non-negative orthogonal constraint of the label assignment matrix.

- Second, we get a more compact intra-class representation by **minimizing the distances between samples and category anchors**, which eliminates the problem of inseparability caused by the overlapping of neighbor classes

- due to the fact that category anchors contain inter-class  topology, our approach also preserves the inter-class topology. The results further verify that our proposed intra-class compactness learning can obtain more inter-class separable results in Section 5.

### Preserving projected clustering

we integrate **inter-class topology preserving** **(5)** with **intra-class compactness learning (10)** and propose the following Inter-class Topology-Preserving Clustering (ITPC) objective function.

$$
\begin{aligned}
\min_{\mathbf{S},\mathbf{F},\mathbf{H}} \quad & \sum_{ij=1}^{n} (S_{ij} \| \mathbf{x}_i - \mathbf{x}_j \|_2^2 + \alpha S_{ij}^2) + 2\beta \mathrm{Tr}(\mathbf{F}^T \mathbf{L}_{\mathbf{S}} \mathbf{F})+ \lambda_1 \sum_{st=1}^{r}  R_{st} \| \mathbf{h}_s - \mathbf{h}_t \|_2^2 + \lambda_2 \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{x}_i - \mathbf{h}_l \|_2^2 \\
\text{s.t.} \quad & \mathbf{S} \mathbf{1}_n = \mathbf{1}_n, \quad \mathbf{S} \ge \mathbf{0}, \quad \mathbf{F}^T \mathbf{F} = \mathbf{I}_r, \quad \mathbf{F} \ge \mathbf{0}.
\end{aligned}
\tag{14}
$$

The first  item is used for **graph structure learning**. 

The second term aims to  **learn the label assignment matrix 𝐅 that aligns with the learned graph structure**. 

The third term focuses on **learning anchor representations 𝐇, preserving the inter-class topology structure**. 

The fourth term is used to  **assign samples near the class anchors, facilitating accurate clustering structures and ensuring intra-class compactness.**


Well,in my view. it looks so raw.

To uncover the data’s latent manifold structure, we perform graph  structure learning in the low-dimensional embedding space. We present the objective function for Inter-class Topology-Preserving Projected Clustering (ITPPC) as follows:

$$
\begin{aligned}
\min_{\mathbf{S},\mathbf{F},\mathbf{H},\mathbf{W}} \quad & \sum_{ij=1}^{n} (S_{ij} \| \mathbf{W}^T \mathbf{x}_i - \mathbf{W}^T \mathbf{x}_j \|_2^2 + \alpha S_{ij}^2) + 2\beta \mathrm{Tr}(\mathbf{F}^T \mathbf{L}_{\mathbf{S}} \mathbf{F}) \\
& + \lambda_1 \sum_{st=1}^{r} R_{st} \| \mathbf{h}_s - \mathbf{h}_t \|_2^2 + \lambda_2 \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{W}^T \mathbf{x}_i - \mathbf{h}_l \|_2^2 \\
\text{s.t.} \quad & \mathbf{S} \mathbf{1}_n = \mathbf{1}_n, \quad \mathbf{S} \ge \mathbf{0}, \\
& \mathbf{W}^T \mathbf{W} = \mathbf{I}_k, \\
& \mathbf{F}^T \mathbf{F} = \mathbf{I}_r, \quad \mathbf{F} \ge \mathbf{0}
\end{aligned}
\tag{15}
$$


**rely on the existence of the inter-class relationship matrix 𝐑**. However,  acquiring this matrix can still be **exceedingly expensive** in certain scenarios, despite domain experts’ being capable of constructing it through empirical methods.

To address this issue, we propose a two-step solution called ITPPC-2.

The first step involves obtaining pseudo-labels by solving problem (15) without the inter-class preserving item, which is achieved by **setting 𝜆1 = 0.**

We then use these pseudo-labels to solve problem (7) and derive the inter-class relationship matrix.

In the second step, we utilize the learned inter-class relationship matrix **as input for  Algorithm 2 to generate the low-dimensional embedding representation**.

This two-step approach enables us to effectively overcome the challenge of accessing the inter-class topology in a **more cost-efficient** manner. Here, we give a detailed description of ITPPC-2.

- In the **R-stage**, we use **intra-class compactness learning** to make  classes more separable in low-dimensional embeddings and mine  better graph structure representation 𝐒. Specifically, we learn cluster results via solving the problem (15) with **𝜆1 = 0**, denoted  as **ITPPC-R**. Then, we calculate the category anchors with the  learned **cluster structure information**. We get an approximate  **inter-class relationship matrix($R$)** by solving the problem (7). To  approximate the real class topology as closely as possible, each  class has relationships with other 𝑟 − 2 classes (i.e., **except itself  and the least relevant class**).  
- In the S-stage, we solve the problem (15) using the inter-class  topology obtained from the **R-step calculation**. Through Algorithm 2, we obtain the final graph structure and low-dimensional embedding representation.

**Kernelization Analysis**: The proposed ITPPC can be extended to a  **kernel graph embedding version**, enabling **the handling of non-linear  data distributions**. Specifically, we can substitute the data matrix 𝐗  with the gram matrix 𝐊, where feature space mapping  maps data points to the reproducing kernel Hilbert space. We formulate the Kernel ITPPC as

$$
\begin{aligned}
\min_{\mathbf{S},\mathbf{F},\mathbf{H},\mathbf{W}} \quad & 2 \mathrm{Tr}(\mathbf{W}^T \mathbf{K} \mathbf{L}_{\mathbf{S}} \mathbf{K}^T \mathbf{W}) + \alpha \| \mathbf{S} \|_F^2 + 2\beta \mathrm{Tr}(\mathbf{F}^T \mathbf{L}_{\mathbf{S}} \mathbf{F}) \\
& + \lambda_1 \sum_{st=1}^{r} R_{st} \| \mathbf{h}_s - \mathbf{h}_t \|_2^2 + \lambda_2 \sum_{i=1}^{n} \sum_{l=1}^{r} F_{il} \| \mathbf{W}^T \mathbf{K}_i - \mathbf{h}_l \|_2^2 \\
\text{s.t.} \quad & \mathbf{S} \mathbf{1}_n = \mathbf{1}_n, \quad \mathbf{S} \ge \mathbf{0}, \\
& \mathbf{W}^T \mathbf{W} = \mathbf{I}_k, \\
& \mathbf{F}^T \mathbf{F} = \mathbf{I}_r, \quad \mathbf{F} \ge \mathbf{0}
\end{aligned}
\tag {16}
$$



