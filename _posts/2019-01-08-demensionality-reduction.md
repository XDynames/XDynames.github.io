---
layout: post
title:  "[Report] Dimensionalit Reduction"
date:   2019-01-08
excerpt: "Statistical methods for dimensionality reduction of feature spaces."
tag:
comments: false
---

# Introduction

Dimensionality reduction is concerned with finding meaningful
representations of high-dimensional data in lower-dimensionality spaces.
Often called embeddings, these reduced spaces are desirable for many
reasons. Their increased interoperability can offer better understanding
of phenomenon and decision boundaries by researchers who are restricted
to visualisation of a few dimensions at a time. Samples in the reduced
space also can be operated on more efficiently due to their smaller
size. In some cases the transform used to lower the samples
dimensionality also endeavours to separate classes for subsequent ease
of classification. Another large motivator behind this field of study is
that often the way human brains process real-data is analogous to
dimensionality reduction. Thereby, if the fundamental underpinnings of
informed reduction to a samples intrinsic dimensionality are understood
they may enable further understandings in parallel fields of artificial
intelligence and neuroscience. However, constructing meaningful
embeddings often starts with some assumptions about what the data looks
like or what aspects of that data are valued over others. This leads to
the current diverse pool of methods lacking a universal approach that
achieves informed and optimal embeddings for all databases. Different
approaches to the problem of embedding are explored from their
foundations, providing an overview of the intuition and assumptions
behind their inception. This will drive the discussion of the
similarities and differences in addition to their informed application
to problems.

# Full Spectral Methods

## Principal Component Analysis

Principal Component Analysis (PCA) is guided by the assumption that
features containing smaller amounts of variance are less relevant to the
classification problem. By retaining the \\(k\\) dimensions with the
highest variance, the reduced space is hoped to contain enough of the
information that was present in the original space but without the added
noise contained in the excluded low variance features. Ultimately
enabling better classification performance and reduced computation time
in the smaller space. Before application the technique the data-space
needs to be centred about zero: \\[\begin{aligned}
            \sum_{i=1}^{n} \textbf{x}\_i = 0
        \end{aligned}\\] Where \\(n\\) is the total number of features in
the space. 

### Linear

Two initial types of PCA can be formed by considering the covariance or
correlation matrix respectively. The difference between these methods is
that correlation PCA requires features to be re-scaled to the unit norm
before the formulation of \\(\textbf{C}\\), otherwise covariance PCA is
being applied . The covariance/correlation matrix can be constructed
using: \\[\begin{aligned}
            \textbf{C} = \frac{1}{n}\sum_{i=1}^{n} \textbf{x}\_{i}\textbf{x}\_{i}^{T}
        \end{aligned}\\] This matrix will contain the respective
correlation or covariance between each feature in the space, with
diagonal containing either ones or the variance of each feature. As
\\(\textbf{C}\\) is found via the multiplication of a matrix with its
transpose, the singular value decomposition of the matrix becomes
equivalent to eigenvalue decomposition: \\[\begin{aligned}
            \textbf{C} = \textbf{U}\textbf{S}^{2}\textbf{U}^T = \textbf{E}\Lambda\textbf{E}^{-1}
        \end{aligned}\\] Where \\(\textbf{E}\\) is a matrix formed with
columns that are the eigenvectors of \\(\textbf{C}\\) and \\(\Lambda\\) is a
diagonal matrix containing the corresponding eigenvalues for each
eigenvector in \\(\textbf{E}\\).  Each eigenvalue represents the variance
in its respective eigenvectors direction, which collectively form a
basis for the original data-space. Principal components are required to
be orthogonal to one another, in this case the problem has been
formulated in terms of eigenvectors which are mutually orthogonal and
thus any pair of columns selected from the matrix \\(\textbf{E}\\) will be
orthogonal. This simplifies the extraction of components to picking the
\\(k\\) largest eigenvalues from the matrix \\(\Lambda\\) which have
corresponding principle components contained in the columns of
\\(\textbf{E}\\). The reduced matrices containing only the \\(k\\) principle
components and their respective eigenvalues are denoted as
\\(\textbf{E}\_{k}\\) and \\(\Lambda_{k}\\) respectively. The original data
can then be projected into the space defined by the primary components
by using the eigenvectors to inform the reconstruction of the sample as
a linear combination of its original features: \\[\begin{aligned}
            \textbf{y}\_{i} = \textbf{x}\_{i}\textbf{E}\_{k}
        \end{aligned}\\] where \\(\textbf{y}\_{i}\\) is the \\(1 \times k\\)
sample projection, \\(\textbf{x}\_{i}\\) is the \\(1 \times d\\) sample in
the original space and \\(\textbf{E}\_{k}\\) is the reduced \\(d \times k\\)
matrix of eigenvectors. This transformation can be shown to de-correlate
the projected data, minimise the \\(L_{2}\\)-norm between
\\(\textbf{y}\_{i}\\) and \\(\textbf{x}\_{i}\\) and allow the construction of
any eigenvector via a linear combination of the original samples. 

### Kernalised

The application of PCA can be extend using a kernel function. To achieve
this the covariance matrix \\(\textbf{C}\\) is re-formulated in terms of
the \\(\phi(\textbf{x}\_{i})\\), which is the transformation function
\\(\phi\\) applied to the sample \\(\textbf{x}\_{i}\\). Next the
corresponding eigenvectors of \\(\textbf{C}\\) are written as a linear
combination of the now transformed samples: \\[\begin{aligned}
            \textbf{C} &= \frac{1}{n}\sum_{i=1}^{n} \phi(\textbf{x}\_{i}) \phi(\textbf{x}\_{i})^{T}
        \end{aligned}\\] \\[\begin{aligned}
            \textbf{v}\_{k} &= \sum_{i=1}^{n} \alpha_{ki} \phi(\textbf{x}\_{i})
        \label{eq:kernelVec}
        \end{aligned}\\] Where \\(\textbf{v}\_{k}\\) is the \\(k^{th}\\)
eigenvector of \\(\textbf{C}\\). The two above results can be used to
expand the eigenvalue equation: \\[\begin{aligned}
            \textbf{v}\_{k}\textbf{C} = \lambda_{k} \textbf{v}\_{k} \\
            \sum_{j=1}^{n} \alpha_{kj} \phi(\textbf{x}\_{j}) \frac{1}{n}\sum_{i=1}^{n} \phi(\textbf{x}\_{i}) \phi(\textbf{x}\_{i})^{T} &= \lambda_{k} \sum_{i=1}^{n} \alpha_{ki} \phi(\textbf{x}\_{i})
        \end{aligned}\\] Multiplying through by
\\(\phi(\textbf{x}\_{l})^{T}\\) \\[\begin{aligned}
             \frac{1}{n}\sum_{i=1}^{n} \phi(\textbf{x}\_{l})^{T} \phi(\textbf{x}\_{i}) \sum_{j=1}^{n} \alpha_{kj} \phi(\textbf{x}\_{i})^{T} \phi(\textbf{x}\_{j}) &= \lambda_{k} \sum_{i=1}^{n} \alpha_{ki} \phi(\textbf{x}\_{l})^{T} \phi(\textbf{x}\_{i})
        \end{aligned}\\] Now the kernel function is defined such that
\\(K(\textbf{x}\_{i}, \textbf{x}\_j) = \phi(\textbf{x}\_{i})^{T} \phi(\textbf{x}\_{j})\\)
and substituted: \\[\begin{aligned}
            \frac{1}{n}\sum_{i=1}^{n} K(\textbf{x}\_{i}, \textbf{x}\_j) \sum_{j=1}^{n} \alpha_{kj} K(\textbf{x}\_{i}, \textbf{x}\_j) &= \lambda_{k} \sum_{i=1}^{n} \alpha_{ki} K(\textbf{x}\_{i}, \textbf{x}\_j)
        \end{aligned}\\] Which can now be written in matrix notation,
using the kernel matrix \\(\textbf{K}\\) with elements
\\(\textbf{K}\_{i,j}=K(\textbf{x}\_{i}, \textbf{x}\_j)\\) as:
\\[\begin{aligned}
            \textbf{K}^{2}\boldsymbol{\alpha}\_{k} &= n \lambda_{k} \textbf{K} \boldsymbol{\alpha}\_{k}
        \end{aligned}\\] By pre-multiplying by the inverse of
\\(\textbf{K}\\) an equation that can be solved to find the vector of
weights \\(\boldsymbol{\alpha}\_{k}\\) is given: \\[\begin{aligned}
            \textbf{K}\boldsymbol{\alpha}\_{k} &= n \lambda_{k}\boldsymbol{\alpha}\_{k}
        \end{aligned}\\] Once \\(\boldsymbol{\alpha}\_{k}\\) has been
determined its elements can be used to find the eigenvectors using the original equation for \\(\textbf{v}\_{k}\\). The projection of a sample
into the new space can be computed using: \\[\begin{aligned}
            \phi(\textbf{x}\_{i})^{T} \textbf{V}^{k}  &= \sum_{i=1}^{n} \boldsymbol{\alpha}\_{i} K(\textbf{x}, \textbf{x}\_{i})
        \end{aligned}\\] Where \\(\textbf{V}^{k}\\) is the matrix of the
\\(k\\) eigenvectors with the largest eigenvalues and
\\(\boldsymbol{\alpha}\_{i}\\) are the coefficients used to form the
\\(k^{th}\\) eigenvector from samples in the original space. In this way
the projection of a sample into the reduced space requires the pair-wise
kernel matrix and weights \\(\alpha\\), thus the explicit evaluation of
\\(\phi(\textbf{x})\\) is never required. In general, the transformed
data may not be centred, this can be accounted for by centring the
kernel. It can be shown that the centred kernel depends only on the
non-centred kernel and is found by: \\[\begin{aligned}
            K_{c}(\textbf{x}\_{i}, \textbf{x}\_{j}) &= K(\textbf{x}\_{i}, \textbf{x}\_{j}) - k(\textbf{x}\_{i})\textbf{1}\_{j}^{T} - \textbf{1}\_{i} k(\textbf{x}\_{j})^{T} + \ell\textbf{1}\_{i}\textbf{1}\_{j}^{T} \\
            & \text{with } k_{i} = \frac{1}{n} \sum_{i=1}^{n} \textbf{K}\_{i,j} \\
            & \text{and } \ell = \frac{1}{n^{2}} \sum_{i,j =1}^{n}\textbf{K}\_{i,j}
        \end{aligned}\\] 

## Classical Multidimensional Scaling

This technique aims to create a map that shows the relative difference
between samples from a matrix of distances. To apply multidimensional
scaling (MDS) the exact distance matrix only needs to be estimated in
order to make a visualisation of the samples in a low dimensional space.
In practice the number of dimensions required to describe the
relationship between samples is not know and is a parameter that needs
to be tuned. However, in most cases for useful visualisation of
dissimilarity the available dimensions should be limited to at most
three. Dimensionality reduction using this method in the classical form
with Euclidean distance is the same as PCA and represents a full
spectral approach to finding the embedding preserving pair-wise
distance. For this reason it is not explicitly defined and is implicitly
formulated by using the pair-wise distance matrix in place of the
covariance matrix above. However, MDS also has several non-convex
methods that sit outside the classification of full/spare spectral and
are explored before the discussion. 

## Linear Discriminant Analysis

For the purposes of this explanation of Linear Discriminant Analysis
(LDA) samples that can be classified in a binary fashion will be
considered. However the techniques natural extension to multi-class
datasets is via the use of Multiple Discriminant Analysis.  The
overarching goal is LDA is that if two classes existing in the data a
transform that clusters like samples closely while maximising the
distance between the cluster is desired for ease of separation by some
classification boundary. Towards this end we define the between and
within class scatter matrices \\(\textbf{S}\_{B}\\) and \\(\textbf{S}\_{W}\\)
respectively, as follows: \\[\begin{aligned}
    \textbf{S}\_{B} &= (\textbf{m}\_{1}-\textbf{m}\_{2})(\textbf{m}\_{1}-\textbf{m}\_{2})^{T} \\
    \textbf{S}\_{W} &= \sum_{j=1}^{2}\sum_{i=1}^{n_{j}}(\textbf{x}-\textbf{m}\_{j})(\textbf{x}-\textbf{m}\_{j})^{T} \\
    \text{where } \textbf{m}\_{j} &= \frac{1}{n_{i}} \sum_{i=1}^{n_{i}}\textbf{x}\_{i}^{j}
    \end{aligned}\\] A vector \\(\textbf{w}\\) is defined that will be
optimised such that along its span the inter-class distance is maximised
and the intra-class variance in minimised using the following
formulation: \\[\begin{aligned}
    \textbf{Maximise  } \textbf{J}(\textbf{w}) = \frac{\textbf{w}^{T}\textbf{S}\_{B} \textbf{w}}{\textbf{w}^{T}\textbf{S}\_{W} \textbf{w}}
    \end{aligned}\\] By maximising the above function the denominator
that reflects the component of within class variance is minimised and
the numerator corresponding to between class difference is maximised
simultaneously. The optimised \\(\textbf{w}\\) represents a basis vector
in the reduced space. The optimal \\(\textbf{w}\\) can be found by finding
the number dimensions of the reduced space in eigenvectors that have the
largest eigenvalues of the matrix \\(\textbf{S}\_{W}^{-1}\textbf{S}\_{B}\\).
New samples can be embedded into the space by calculating their
projections onto \\(\textbf{w}\\): \\[\begin{aligned}
    \textbf{y}\_{i} = \textbf{w} \cdot \textbf{x}\_{i}
    \end{aligned}\\]

### Kernel

To further extend LDA to situations where the best basis of the space is
not linear the kernel trick can be used. As before the problem is
redefined in terms of \\(\phi(\textbf{x}\_{i})\\), which is the
transformation function \\(\phi\\) applied to the sample
\\(\textbf{x}\_{i}\\): \\[\begin{aligned}
    \textbf{S}\_{B}^{\phi} &= (\textbf{m}\_{1}^{\phi}-\textbf{m}\_{2}^{\phi})(\textbf{m}\_{1}^{\phi}-\textbf{m}\_{2}^{\phi})^{T} \\
    \textbf{S}\_{W}^{\phi} &= \sum_{j=1}^{2}\sum_{i=1}^{n_{j}}(\phi(\textbf{x})-\textbf{m}\_{j}^{\phi})(\phi(\textbf{x})-\textbf{m}\_{j}^{\phi})^{T} \\
    \text{where } \textbf{m}\_{j}^{\phi} &= \frac{1}{n_{j}} \sum_{i=1}^{n_{j}}\phi(\textbf{x}\_{i}^{j}) \\
    \textbf{Maximise  } \textbf{J}(\textbf{w}) &= \frac{\textbf{w}^{T}\textbf{S}\_{B}^{\phi} \textbf{w}}{\textbf{w}^{T}\textbf{S}\_{W}^{\phi} \textbf{w}}
    \end{aligned}\\] In order to reduce the need to explicitly transform
every sample in the set using the function \\(\phi\\) a form that involves
only the inner products between the transformed samples is sought.
Firstly, \\(\textbf{w}\\) is expressed as a linear combination of samples:
\\[\begin{aligned}
    \textbf{w} = \sum_{k=1}^{n} \alpha_{k}\phi(\textbf{x}\_{k})
    \end{aligned}\\] Which is then used to modify the equation for
\\(\textbf{m}\_{j}^{\phi}\\): \\[\begin{aligned}
    \textbf{w}^{T}\textbf{m}\_{j}^{\phi}= \sum_{k=1}^{n} \alpha_{k}\phi(\textbf{x}\_{k})^{T} \frac{1}{n_{j}} \sum_{i=1}^{n_{j}}\phi(\textbf{x}\_{i}^{j}) \\
    \textbf{w}^{T}\textbf{m}\_{j}^{\phi}= \frac{1}{n_{j}}\sum_{k=1}^{n}\sum_{i=1}^{n}\_{j}\alpha_{k}\phi(\textbf{x}\_{k})^{T}\phi(\textbf{x}\_{i}^{j})
    \end{aligned}\\] Then the kernel function \\(k\\) is defined as
\\(k(\textbf{x}\_{i},\textbf{x}\_{k}) = \phi(\textbf{x}\_{k})^{T}\phi(\textbf{x}\_{i})\\)
giving: \\[\begin{aligned}
    \textbf{w}^{T}\textbf{m}\_{j}^{\phi}= \frac{1}{n_{j}}\sum_{k=1}^{n}\sum_{i=1}^{n}\_{j}\alpha_{k}k(\textbf{x}\_{i},\textbf{x}\_{k}^{j})
    \end{aligned}\\] Which can be rewritten in matrix form as:
\\[\begin{aligned}
    \boldsymbol{\alpha}^{T}\textbf{M}\_{i}
    \end{aligned}\\] This can now be used to re-write the numerator in
the objective function, with
\\(\textbf{M}=(\textbf{M}\_{1}-\textbf{M}\_{2})(\textbf{M}\_{1}-\textbf{M}\_{2})^{T}\\),
as: \\[\begin{aligned}
    \textbf{w}^{T}\textbf{S}\_{B}^{\phi} \textbf{w} = \boldsymbol{\alpha}^{T}\textbf{M}\boldsymbol{\alpha}
    \end{aligned}\\] Similarly the revised form of
\\(\textbf{m}\_{j}^{\phi}\\) can be used to re-write the detonator:
\\[\begin{aligned}
    \textbf{w}^{T}\textbf{S}\_{W}^{\phi} \textbf{w} = \boldsymbol{\alpha}^{T}\textbf{N}\boldsymbol{\alpha}
    \end{aligned}\\] where
\\(\textbf{N} = \sum_{j=1}^{2} \textbf{K}\_{j}(\textbf{I}-\textbf{1}\_{n_{j}})\textbf{K}\_{j}^{T}\\)
and \\(\textbf{K{}}\\) is a \\(n \times n_{j}\\) matrix with elements
\\((\textbf{K}\_{j})_{lm}=k(\textbf{x}\_{l},\textbf{x}\_{m}^{j})\\). This
gives the reformulation of the function to be optimised as:
\\[\begin{aligned}
    \textbf{Maximise  } \textbf{J}(\boldsymbol{\alpha}) &= \frac{\boldsymbol{\alpha}^{T}\textbf{M} \boldsymbol{\alpha}}{\boldsymbol{\alpha}^{T}\textbf{N} \boldsymbol{\alpha}}
    \end{aligned}\\] This can also be solved by taking the top
eigenvectors of \\(\textbf{N}^{-1}\textbf{M}\\) to form
\\(\boldsymbol{\alpha}\\) which is then used to map samples from the
original space via a linear combination of value of the kernel function:
\\[\begin{aligned}
    \textbf{y}\_{j} = \textbf{w} \cdot \textbf{x}\_{j} = \sum_{i=1}^{n} \alpha_{i}k(\textbf{x}\_{i},\textbf{x}\_{j})
    \end{aligned}\\] By adding a multiple of the identity to the matrix
\\(\textbf{N}\\) numerically the problem becomes more stable.
\\(\textbf(N)\_{\mu} =\textbf{N}+\mu\textbf{I}\\) is suggested as a
replacement of \\(\textbf{N}\\) in literature. 

## Isometric Feature Mapping

Isomap extends the idea of multidimensional scaling by seeking to
preserve the geodesic distance between all points along some underlying
manifold. This allows for the reduction of more complex non-linear
surfaces without the lose of the manifold structure’s global schematics.
There are three main steps needed to achieve the embedding:

1.  determine the neighbours of each point. This can be achieved by
    using K-nearest neighbours or setting the radius of a hyper-sphere
    that contains a points neighbours

2.  neighbours are used to form a weighted graph connecting all the
    points by the local distances between them

3.  geodesic distance is estimated between all points by applying a
    shortest path finding algorithm to the weighted network

4.  MDS is the applied to the matrix of geodesic distances to
    reconstruct samples in a way that minimises change in the estimated
    shortest path distances along the manifolds surface

## Maximum Variance Unfolding

Another manifold learning method is Maximum Variance Unfolding (MVU). To
find an embedding that projects a lower dimensional manifold contained
in a higher dimensional space two properties are valued in this
technique; the inter-sample distances and the global variance.
Conceptually this technique is visualised by considering samples that
are attached to their nearest neighbours by inflexible rods. This linked
structure is then pulled apart to form a lower dimensional embedding.

### Primal

Formally this can be expressed as a quadratic programming problem
naively maximising the distance between all points globally under the
constraint that the distances between neighbours are preserved:
\\[\begin{aligned}
        \textbf{Maximise  } \sum_{i,j=1}^{n}||\textbf{y}\_{i}-\textbf{y}\_{j}||^{2}
    \end{aligned}\\] \\[\textbf{Constrained by } 
    \begin{cases}
        ||\textbf{y}\_{i}-\textbf{y}\_j||^{2} = ||\textbf{x}\_{i}-\textbf{x}\_{j}||^{2} \hspace{0.5cm} \boldsymbol{\forall} i,j \text{ with } \eta_{ij} = 1 \\
        \sum_{i=1}^{n} \textbf{y}\_{i}=0
    \end{cases}\\] Where \\(\textbf{y}\\) and \\(\textbf{x}\\) or the
embedded and original samples respectively and \\(\eta_{ij}\\) is one when
\\(i\\) and \\(j\\) are neighbouring or zero otherwise.  Unfortunately this
is not a convex problem and therefore cannot be guaranteed to find a
global maximum for the objective function. However, it has been shown
that the above problem can be re-written in terms of the Gram matrix
\\(\textbf{K}\\): \\[\begin{aligned}
        \textbf{Maximise }\text{Trace($\textbf{K}$)}
    \end{aligned}\\] \\[\textbf{Constrained by } 
    \begin{cases}
    \begin{multline}
        \textbf{K} \succeq 0 \\
        \sum_{i,j=1}^{n} \textbf{K}\_{ij}=0 \hspace{0.5cm} \boldsymbol{\forall} i,j \text{ with } \eta_{ij} = 1 \text{ or } [\eta^{T}\eta]\_{ij} = 1 \\
        \textbf{K}\_{ii} + \textbf{K}\_{jj} - \textbf{K}\_{ij} - \textbf{K}\_{ji} = \textbf{G}\_{ii} + \textbf{G}\_{jj} - \textbf{G}\_{ij} - \textbf{G}\_{ji}
        \end{multline}
    \end{cases}\\] Where
\\(\textbf{G}\_{ij} = \textbf{X}\_{i} \cdot \textbf{X}\_{j}\\) and
\\(\textbf{K}\_{ij} = \textbf{Y}\_{i} \cdot \textbf{Y}\_{j}\\) are the Gram
matrices for the input and output spaces respectively. Once this
optimisation is complete the resulting output Gram matrix
\\(\textbf{K}\\)’s \\(k\\) largest eigenvalue eigenvectors are used to
embed pints from the input space to the reduced space. 

# Sparse Spectral Methods

## Locally Linear Embedding

Locally Linear Embedding (LLE) seeks a reconstruction of the manifold in
a lower dimensionality space that preserves the local and thus global
structure of the high-dimensional surface. However LLE focuses on
preserving local structure to implicitly retain global ones. This
eliminates the need for estimation of geodesic distance from every point
on the manifold to each other. Local graphs are constructed by using a
neighbour identifying algorithm and then weights are assigned to each
edge. If the point is not a neighbour is will be allocated a weight of
zero, if they are connected a weight between zero and will be assigned
subject to the constraint that the weights of all edges of a sample sum
to one. This constraint gives the local embedding invariance to the
original spaces translation, rotation and scale. To calculate the exact
weight of an edge the distance squared cost function: \\[\begin{aligned}
    \epsilon(\textbf{W}) = \sum_{i=1}^{n} \left|\textbf{x}\_{i}-\sum_{j=1}^{k}\textbf{W}\_{ij}\textbf{x}\_{j}\right|^{2}
    \end{aligned}\\] Where n is the total number of samples in the
original space and k is each point’s number of neighbours, is minimised.
To construct the low dimensional embedding the weights found by in the
initial optimisation are fixed and the same distance squared cost
function is minimised for the embedded points \\(\textbf{y}\_{i}\\):
\\[\begin{aligned}
    \Phi(\textbf{y}) = \sum_{i=1}^{n} \left|\textbf{y}\_{i}-\sum_{j=1}^{k}\textbf{W}\_{ij}\textbf{y}\_{j}\right|^{2}
    \end{aligned}\\] The solution to this optimisation problem can be
found by taking the \\(k\\) lowest non-zero eigenvectors of the sparse
matrix of weights. When k-nearest neighbours is used the algorithm only
has one parameter to tune and the solutions found are analytic and as a
result globally minimise the two cost functions. 

## Laplacian Eigenmaps

In this approach local manifold structure is used to preserve
information retained in the embedding. To do this local graphs of
connectivity are constructed and represented as a Laplacian matrix which
is then decomposed into eigenvectors which represents the optimal
embedding of the points in the lower dimensional space that preserves
the local structural information contained in the Laplacian. The overall
algorithm is as follows:

1.  Identify each points local adjacency using either using k-nearest
    neighbours or a hyper-sphere of radius \\(\epsilon\\)

2.  Construct the Laplacian matrix
    \\(\textbf{L} = \textbf{D} - \textbf{W}\\) were \\(\textbf{D}\\) is the
    diagonal matrix with elements equal to the sum of the weights
    connected to the node and \\(\textbf{W}\\) is the matrix containing
    the weights of edges between nodes. Weights of the edges in the
    graph can be selected using to methods:
    
      - Heat Kernel -
        \\(W_{ij}=exp\left(\frac{||\textbf{x}\_{i}-\textbf{x}\_j||^{2}}{t}\right)\\)
        if samples \\(\textbf{x}\_i\\) and \\(\textbf{y}\_j\\) are connected
        or zero if they are not. \\(t\\) is the temperature and can be
        tuned to modify embeddings
    
      - Binary - In the limit as \\(t\rightarrow \infty\\) the heat kernel
        reduces to \\(W_{ij}=1\\) if samples \\(\textbf{x}\_i\\) and
        \\(\textbf{x}\_j\\) are connected or zero if they are not

3.  If these steps do not result in a full connected graph, the
    Laplacian matrix for each graph will need to be subjected to
    eigenvalue decomposition separately. Otherwise the full Laplacian
    for the network is decomposed. An m-dimensional embedding is
    constructed using the eigenvectors with the lowest non-zero
    eigenvalues.

The embedding formed as a result of this procedure is invariant under
translation, rotation and scaling of the original space and optimality
minimises the weighted distance square between embedded points. 

## Maximum Variance Unfolding

### Dual

The re-formulated primal problem solved to find the optimal Gram matrix
for the embeddings can also be reformulated to a dual via a Lagrange
transform to: \\[\begin{aligned}
    \textbf{Maximise } \lambda_{n-1}(\textbf{L})
    \end{aligned}\\] \\[\textbf{Constrained by } 
    \begin{cases}
    \sum_{i,j}^{n} \textbf{D}\_{ij} \textbf{W}\_{ij}=c \hspace{0.5cm} \boldsymbol{\forall} i,j \text{ with } \eta_{ij} = 1 \\
    \textbf{L} = \sum_{i,j}^{n} \textbf{W}\_{ij} \textbf{E}^{\{ij\}} \hspace{0.5cm} \boldsymbol{\forall} i,j \text{ with } \eta_{ij} = 1 \\
    c > 0
    \end{cases}\\] Where \\(\lambda_{n-1}\\) is the second smallest
eigenvalue, \\(\textbf{E}^{\{ij\}}\\) is an \\(n \times n\\) matrix with
four non-zero values,
\\(\textbf{E}^{\{ij\}}\_{ii} = \textbf{E}^{\{ij\}}\_{jj} = 1\\) and
\\(\textbf{E}^{\{ij\}}\_{ij} = \textbf{E}^{\{ij\}}\_{ji} = -1\\),
\\(\textbf{W}\\) is an \\(n \times n\\) matrix with elements
\\(\textbf{W}\_{ij}\\) are the same as the values of \\(\eta_{ij}\\) and
\\(\textbf{D}\_{ij} = Trace(\textbf{K}\textbf{E}^{\{ij\}})\\). The solution
to the dual problem is optimal for the primal if and only if they
satisfy the Karush-Kuhn-Tucker conditions of primal and dual feasibility
in addition to complementary slickness, otherwise some duality gap
exists between the solutions. 

# Non-Convex Methods

## Multidimensional Scaling - Distance Scaling

There are several different methods to reconstruct the samples from the
difference matrix \\(\Delta\\) in addition to a slue of heuristics used to
inform the construction. Each element of \\(\Delta\\) is given by the
estimated dissimilarity between two samples in the original dataset
\\(\delta_{i,j}\\), this need not be Euclidean distance and is extendible
to any dissimilarity function. The goal of MDS is to find some
\\(k\\)-dimensional vectors \\(x_{i}, x_{j}\\) such that
\\(||x_{i}- x_{j}|| \approx \delta_{i,j}\\) for all \\(i, j\\). To achieve
this the following steps are used:

1.  a set of \\(k\\)-dimensional vectors are initialised for each of the
    \\(n\\) objects that are to be reconstructed. The Euclidean distance
    is then calculated between these objects to form a set of
    inter-sample distances \\(d_{i,j}\\).

2.  observed distances are regressed against \\(\delta_{i,j}\\)

3.  disparity between the observed and actual difference is used to
    scale the observed distances to, as closely as possible, match the
    \\(\delta_{i,j}\\)

4.  resulting samples are then evaluated against a heuristic referred to
    as the stress which aims to quantify how well the current
    reconstruction approximates the true differences

5.  objects’ elements are modified to reduce the overall stress of the
    configuration.

Steps 2-5 are repeated until a minimum of stress is achieved.   
In the procedure outlined the main choices are the method in which the
observed and true distances are regressed, the choice of heuristic and
how many dimensions to allow the reconstruction of samples in. While the
choice of heuristic and dimensionality are primarily choices made either
as a result of testing or informed by information about the data, the
function used to regress the dissimilarity leads to two families of
distance scaling.

### Metric

If the regression function used to scale the observed distances is a
continuous function the resulting MDS falls into this category. This is
used in the case of dissimilarities are evaluated on a scale and will
minimise the least-squares error between the observed and true
differences. For the case when \\(\Delta\\) is compromised of differences
that are truly Euclidean distances the solution minimised the value of
stress for the current configuration.

### Non-Metric

When the dissimilarities contained in the difference matrix are not
derived from a true spatial relationship it is beneficial to relax the
constraint of the regression to being a monotonic function:
\\[\begin{aligned}
    d_{i,j} = f(\delta_{i,j}+e_{i,j})
    \end{aligned}\\] Where \\(e_{i,j}\\) is the measurement error of the
dissimilarity. This extends the method to dissimilarities in general and
can accommodate real world observations more readily. Krusal’s algorithm
for minimisation of the stress uses gradient decent to find the local
minimum from a given point of initialisation. This means that the
starting point chosen will influence the solution reached and the
technique is not robust to local minimum. One method used to help
achieve more global minimums is to first use metric MDS to find a
configuration that can be used to initialise the algorithm. 

# Discussion

## Advantages and Disadvantages

A summary of the techniques features are shown in Table
[\\[table:techn\\]](#table:techn). Of the methods examined only one uses
class labels to inform the embedding. For this reason if labelled data
is available LDA or kLDA should be used to reduced the spaces
dimensionality. Where different kernels and parameters can be used to
optimise the achieved embeddings.  
Full spectral techniques are often preferable to sparse spectral ones as
the eigen-decompositions of dense matrices is more stable numerically
than identifying the lowest eigenvalue of a sparse matrix. This is due
to the need to distinguish between near-zero eigenvalues from zero which
becomes a matter of preserving floating point accuracy to infinitesimal
levels. However, sparse matrices, when treated appropriately are more
computationally efficient to act on than their dense counterparts. For
this reason full spectral techniques are often not appropriate for very
large dataset and a sparse method could be used to reduce the
computational overhead of the task. Another consideration when selecting
between full and sparse spectral techniques is whether there is some
intrinsic value in the global structure of the data that is not
persevered via local schematics. If complex global features exists dense
full spectral methods seek to preserve these structures more explicitly
an should therefore be favoured over sparse techniques.  
Most manifold learning techniques are not appropriate to use on datasets
that do not contain a well sampled manifold within them do not generate
useful embeddings. Thus some understanding of the samples being embedded
is necessary before using a method that assumes some manifold exists. Of
equal importance is that enough samples are present to ensure the
manifold is described well enough, otherwise long connections or
disconnected portions of the manifold will cause undesired behaviour in
the reduced space that do not reflect the true nature of the manifold.
Issues also arise when samples are not evenly distributed over the
manifold and can lead to a phenomenon known as folding, which violates
the assumption that a manifold is locally linear. Folding can occur when
the selection of neighbours allocates too many relative to the local
density of samples. To avoid the issue of folding adaptive neighbourhood
selection can be used, but this comes at the price of complexity.  
Sparse manifold learning techniques suffer from some specific short
comings. If the local structure of the manifold does not also represent
its global structure the method will over-fitting the manifold causing
embeddings that do not generalise. When neighbour assignment is
preformed by the k-nearest neighbours algorithm outlier samples can have
a strong impact on the embedding achieved due to their violation of the
condition that neighbours should be local to the sample. Additionally,
all sparse techniques suffer from a modified version of the curse of
dimensionality that relates to the intrinsic dimensions of the manifold.
Dubbed the curse of intrinsic dimensionality the number of samples
required to properly describe a manifolds structure grows rapidly with
respect to its number of intrinsic dimensionality.  
Classical full spectral methods in general do not suffer from many of
the shortcomings seen previously, but are often unable to model simple
non-linear manifolds often used to benchmark manifold learning
techniques such as the Swiss roll. These techniques rely heavily on the
selection or development of the appropriate kernel for the data to be
reduced well. Full spectral manifold learning methods are able to deal
with these more complex structures but again many of the disadvantages
seen in their sparse counter parts return, such as over-fitting, outlier
sensitivity and the curse of dimensionality. Additionally these methods
are susceptible to short-circuiting, which occurs when points on one
section of the manifolds surface are labelled as neighbours to a sample
on a parallel section of the surface. This can have severe impacts on
the embeddings created by these methods. Specifically Isomap will find
very different shortest paths to approximate geodesic distance leading
to a completely different mapping and MVU will create an inflexible rod
between the surfaces preventing the complete unfolding of the manifold.
  
In some cases the curse of dimensionality is avoided for manifold
learning techniques when the embedding that is to be extracted is lower
than the true dimensionality of the complete manifold. Often because the
embedding sought will contain only a few intrinsic dimensions of the
overall manifold. This can be seen in many of the cited papers when the
techniques are applied to image databases and meaningful reconstruction
of the objects posed direction can be found. This indicates that for
sufficiently well defined data these techniques are still able to create
meaningful embeddings from features that exist within a more complex
complete manifold. This also runs parallel to their application on data
with underlying clusters, exemplified by the creation of meaningful
reduced spaces for word similarities from higher dimensional frequency
data.  &   
The drawback of non-convex techniques were touched on in the section
covering distance scaling MDS, specifically that local minima exist that
are not global. In some senses this issue of global optimality is
divisive as it is explicit that solutions found are not guaranteed to be
optimal or necessarily verifiable as optimal. However the necessity for
optimality has been challenged by many successful non-convex techniques
that use iterative reduction of an objective function to achieve some
embedding. These may offer solutions that although are not universally
true or unique are calculable for large spaces in a definable time by
limiting the number of iterations before a solution is returned. These
techniques do not guarantee the discovery of fundamental intrinsic
dimensionality that is often sought when seeking an embedding
either.  

| **Method**   | **Convex** | **Full Spectral** | **Manifold Learning** | **Non-Linear** | **Supervised** |
| :----------- | :--------: | :---------------: | :-------------------: | :------------: | :------------: |
| PCA          | \\(\times\\) |    \\(\times\\)     |          \-           |       \-       |       \-       |
| kPCA         | \\(\times\\) |    \\(\times\\)     |          \-           |   \\(\times\\)   |       \-       |
| LDA          | \\(\times\\) |    \\(\times\\)     |          \-           |       \-       |   \\(\times\\)   |
| kLDA         | \\(\times\\) |    \\(\times\\)     |          \-           |   \\(\times\\)   |   \\(\times\\)   |
| LLE          | \\(\times\\) |        \-         |      \\(\times\\)       |   \\(\times\\)   |       \-       |
| IsoMap       | \\(\times\\) |    \\(\times\\)     |      \\(\times\\)       |   \\(\times\\)   |       \-       |
| MVU - Primal | \\(\times\\) |    \\(\times\\)     |      \\(\times\\)       |   \\(\times\\)   |       \-       |
| MVU - Dual   | \\(\times\\) |        \-         |      \\(\times\\)       |   \\(\times\\)   |       \-       |
| LEM          | \\(\times\\) |        \-         |      \\(\times\\)       |   \\(\times\\)   |       \-       |
| cMDS         | \\(\times\\) |    \\(\times\\)     |          \-           |       \-       |       \-       |
| MDS - DS     |     \-     |        \-         |          \-           |   \\(\times\\)   |       \-       |

A summary of salient aspects of each of the previously outlined
dimensionality reduction techniques. Notes: If the technique is labelled
as non-convex it is not a spectral technique and MDS distance scaling
can be non-linear if a non-linear regression function is
used<span label="table:techn"></span>

## Similarities

It was been discussed in previous works that these methods form a
ensemble of closely related approaches to dimensionality reduction. In
fact manifold learning techniques Isomap, LE and LLE have all been
related via the use of specific kernels in kPCA, up to some scaling
factor. It has been speculated that this is a result of the manifold
learning techniques are an attempt to warp the space such that the
manifold becomes flat. So designing a kernel function that uses a
similar approach to the manifold learning technique the input space can
be perturbed in a way that the underlying manifold becomes flat in the
transformed kernel space. PCA can then be applied to this flat manifold
to find the low dimensional embedding. It is especially interesting when
considering that LE and LLE are sparse spectral techniques as opposed to
Isomap and kPCA which are both full spectral. Perhaps the utilisation of
LE and LLE via kPCA would eliminate some of the problems when selecting
the lowest non-zero eigenvalues from sparse matrices. In addition the
kernels found to enable manifold learning in kPCA maybe transferable to
kLDA, with some modification to the neighbourhood assignment that would
not allow different classes to be connected, to achieve structured
clustering. Enabling a more informed interpretation of the reduced
space, when manifolds for each class exist.  
This utilisation of one method to form another is also observed in when
considering MVU in its Primal and Dual forms. In formulating the problem
from Primal to its Dual the nature of the technique shifts from full to
sparse spectral. When in the primal form Isomap can be viewed as an
approximate solution to the MVU problem using geodesic distance as an
approximate to Euclidean. After transformation to its dual form, closer
connections to sparse spectral methods LLE and LE are seen. LLE’s
mission to find local linearity is embedded in the optimality condition
of complements present in the dual MVU’s formulation. Solving for the
optimal elements that provide the maximised lowest eigenvalue is
analogous to choosing a specific method of weight assignment when using
LE, providing an additional method of weight assignment sitting along
side the heat kernel and using binary values.   
When considering both kPCA’s extension to manifold learning and MVU’s
duality enabling its interpretation to the same manifold learning
techniques; Isomap, LE and LLE, an implicit relationship between MVU and
PCA begins to emerge. Both of the techniques ultimately use some
eigen-decomposition to maximise the variance in the reduced spaces. MVU
sets itself apart from PCA as it is explicitly assuming the existing of
a low dimensional manifold that can be preserved by fixing
inter-neighbour distances. When this same assumption is added to kPCA
via an appropriate kernel the two become similar. In this way, PCA can
be seen as a manifold agnostic MVU or conversely MVU a specific manifold
learning extension of PCA.

# Conclusion

Dimensionality reduction is very much an open problem with a wide
variety of approaches that share many similarities and differences. Most
manifold learning and non-linear techniques work very well in situations
where the underlying relationship is known prior to the informed
application of embedding techniques. This is why is it important when
applying a given method that the underlying nature of the data is
considered. If a low dimensional manifold doesn’t exist, density over
the surface varies or it is under sampled, which is the case for many
real-world datasets, the manifold learning techniques explored will not
preform. Likewise the application of a linear technique in situations
where higher order relationships exist also leads to poor embeddings.
Parallels between many of the techniques have been explored. The use of
appropriate kernels in PCA has been seen to enable manifold learning
techniques Isomap, LE and LEE to be implemented within its framework.
Additionally, Isomap, LE and LLE can be interpreted through MVU’s primal
and dual formulations. This lead to the interpretation of MVU as PCA
that preserves the existing of a low dimensional manifold in its
formulation of embeddings.
