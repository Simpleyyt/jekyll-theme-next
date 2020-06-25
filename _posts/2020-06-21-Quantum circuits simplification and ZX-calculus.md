---
layout: post
title: GSoC 2020&#58; Quantum circuit simplification and ZX-calculus
description: Implement ZX-calculus with Julia.
author: Chen Zhao
tags:
- GSoC
- ZX-calculus
---

First of all, I would like to thank my mentor [Roger Luo](https://github.com/Roger-luo) and [Jinguo Liu](https://github.com/GiggleLiu) for supervising me during Google Summer of Code 2020. In this GSoC project, I will develop a new Julia package `ZXCalculus.jl` which implements ZX-calculus, and integrate it as a circuit simplification engine with `YaoLang.jl`, the next DSL (domain-specific language) for `Yao.jl` and quantum programs. `Yao.jl` has achieved a state-of-art quantum simulator with many advanced features such as automatic differentiation and symbolic computation. As a user of `Yao.jl`, I'm so glad to have the oppotunities to make contributions to it. In this blog post, I will summerize what we have done before the first evaluation.

## Quantum circuits and ZX-calculus

Quantum computing uses the principles of quantum mechanics for computing. To describe quantum algorithms, we often use the quantum circuit model which is the analog of the classical logic circuit model. Quantum circuits are consist of a set of basic quantum gates such as Pauli X, Y, Z gate, Hadamard gate, T gate, CNOT gate.

In general, there will be lots of equivalent quantum circuits representing the same operation. At the viewpoint of quantum hardware, one wishes to find the circuit that minimizing the usage of hardware resources. Unfortunately, finding the most desired circuit is a very hard problem in computational complexity, as checking the equivalence between two quantum circuits is coQMA-hard (quantum analog of coNP) [^1],[^2]. Despite this, we still have some heuristic efficient algorithms for quantum circuits simplification. In this blog post, I will introduce a powerful tool for this problem, ZX-calculus.

A quantum circuit is a graph representing tensor products and matrices products. For example, this is the circuits representing 
$$
\mathrm{CNOT}(2,3)\cdot\mathrm{CNOT}(1,2)\cdot(H\otimes I\otimes I).
$$
![](\assets\blog_res\ZX\cir1.png)

Also, quantum circuits can be regarded as a special type of tensor network. In ZX-calculus, we introduce two kinds of tensors, Z-spiders, and X-spiders. Tensor networks that consist of these tensors are called ZX-diagrams. As all basic quantum gates can be represented by ZX-diagrams, we can transform all quantum circuits to ZX-diagrams. 
![](\assets\blog_res\ZX\zxd1.svg)
Moreover, by the definition of Z-spiders and X-spiders, there are basic equivalent rules with which one can simplify ZX-diagrams. 
![rules](\assets\blog_res\ZX\rules.png)
After simplifying ZX-diagrams, we should transform them back to circuits. In the paper [^3], they developed algorithms for the above scheme. 

During GSoC 2020, I'm going to achieve a Julia implementation of ZX-calculus. In the following sections, I will explain the reasons and methods briefly for this project.

## Proposal for this GSoC project

The main proposal of this project is to implement ZX-calculus and its related algorithm in Julia language. And we will release a Julia package `ZXCalculus.jl`. There exists a Python implementation of ZX-calculus, [`PyZX`](https://github.com/Quantomatic/pyzx). Like `PyZX`, `ZXCalculus.jl` will provide APIs for rewriting ZX-diagrams, extracting circuits from ZX-diagrams, simplifying circuits. Visualization of ZX-diagrams will be available in `ZXCalculus.jl` too. Most functions of `PyZX` will be implemented in `ZXCalculus.jl`. Besides these, we will integrate `ZXCalculus.jl` in the quantum compiler [`YaoLang.jl`](https://github.com/QuantumBFS/YaoLang.jl) and implement a compilation-level circuit simplification engine.

In `YaoLang.jl`, one can define hybrid quantum programs with classical information (e.g. classical parameters, classical control flows). These quantum programs will be transformed into an intermediate representation (the Yao IR), which contains quantum circuits along with SSA (static single assignment) form of the classical program. These circuits will be simplified with `ZXCalculus.jl`. Then `YaoIR`s will be compiled to backend instructions, like QASM or the simulator instructions of Yao.

To achieve the best performance in the compilation and enable further customization of the simplification procedure, a pure Julia implementation of ZX-calculus is necessary. 

## Methods

The circuits extraction method is a canonical method of ZX-calculus based circuits simplification algorithms. I will mainly discuss how I implement this method.

### Data structures

To implement these algorithms, we need to implement data structures for storing ZX-diagrams and rules on how ZX-diagrams change. A ZX-diagram can be regarded as a multigraph with extra information on its vertices. Each vertex is one of Z-spider, X-spider, or H-box. In the original ZX-diagrams, open edges are allowed. For simplicity, we added 2 special kinds of vertices, input boundary, and output boundary, for recording these edges. We use the adjacency list representation for storing multigraphs. An adjacency list is stored as `Dict` whose keys are the ids of vertices. 
```julia
struct Multigraph{T<:Integer} <: AbstractMultigraph{T}
    adjlist::Dict{T, Vector{T}}
end
```
Because we need to rewrite multigraphs with ZX-calculus rules, it will be more convenient to fix vertices ids. So we use `Dict` instead of `Array`. There are phases for Z-spiders and X-spiders. We need another Dict for storing these phases. The data struct of ZX-diagram is similar to follow:
```julia
struct ZXDiagram{T<:Integer, P} <: AbstractZXDiagram{T, P}
    mg::Multigraph{T}
    st::Dict{T, SpiderType.SType}
    ps::Dict{T, P}
    ...
end
```

In the paper, they proposed graph-like ZX-diagrams. Graph-like ZX-diagrams have only Z-spiders and have different types of edges, Hadamard edges, and non-Hadamard edges. We can also use multigraphs for representing graph-like ZX-diagrams. We can use different edge multiplicities for representing different edge types. We define `ZXGraph` for graph-like ZX-diagrams:
```julia
struct ZXGraph{T<:Integer, P} <: AbstractZXDiagram{T, P}
    mg::Multigraph{T}
    st::Dict{T, SpiderType.SType}
    ps::Dict{T, P}
    ...
end
```

From now, we have built up data structs we need.

### Rewriting rules

To simplify ZX-diagrams, we need to implement rewriting rules in ZX-calculus. There are two steps, matching rules and rewriting ZX-diagrams with corresponding rules. We used the holy traits tricks for Julia multiple dispatch for matching and rewriting. The APIs are like these:
```julia
match(r::AbstractRule, zxd::AbstractZXDiagram)
rewrite!(r::AbstractRule, zxd::AbstractZXDiagram{T, P}, matches::Vector{Match{T}}) where {T, P}
```
Here `match` will return all matched vertices which will be stored in the struct
```julia
struct Match{T<:Integer}
    vertices::Vector{T}
end
```
And `rewrite!` will try to use a rule to rewrite a ZX-diagram on the all matched vertices. Since some matched vertices may become unsatisfied with the corresponding rule after rewriting some vertices. We need to check whether a rule is still available on matched vertices. 
```julia
check_rule(r::AbstractRule, zxd::AbstractZXDiagram{T, P}, vs::Vector{T}) where {T, P}
```

For simpilifying ZX-diagram with a rule, we defined `simplify!` which is like
```julia
function simplify!(r::AbstractRule, zxd::ZXDiagram)
    matches = match(r, zxd)
    while length(matches) > 0
        rewrite!(r, zxd, matches)
        matches = match(r, zxd)
    end
    return zxd
end
```

In the simplification scheme of [^3], we need to convert a ZX-diagram to a graph-like ZX-diagram with rules i1, i2, h and f. Then simplify the graph-like ZX-diagram with local complementary rule and pivoting rule. We can accomplish these steps with the above functions. Simplified graph-like ZX-diagrams will be only small skeletons comparing to original large circuits. The only thing remained is extracting circuits from ZX-diagrams.

### Circuit extraction

This is the most complicated part of the simplification scheme. For more detail, one can read the original paper. I will only explain the circuit extraction algorithm briefly.

There is a property for any ZX-diagram of a quantum circuit. That is we can define a g-flow on it. With a g-flow, we can know the order of all spiders. The rules we used for simplifying the ZX-diagram will preserve this good property. It gives us the possibility to extracting circuits from simplified ZX-diagrams. 

Moreover, any graph-like ZX-diagrams are CNOT + H circuits locally. We can extract these CNOT circuits with Gaussian elimination over $F_2$. Together with the remaining H gates, CZ gates, and Z-rotations, we get a circuit equivalent to the original one.

### Demo

The demo circuit is from the appendix of the paper [^3]. All the codes can be found in `examples\ex1.jl` of `ZXCalculus.jl`.
```julia
# this is the original circuit.
zxd = generate_example()
ZXplot(zxd)
```
![](/assets/blog_res/ZX/1.svg)
```julia
# convert a ZX-diagram to a graph-like ZXdiagram
zxg = ZXGraph(zxd)
ZXplot(zxg, linetype = "curve")
```
![](/assets/blog_res/ZX/2.svg)
```julia
# simplify with local complementary rule 
simplify!(Rule{:lc}(), zxg)
ZXplot(zxg, linetype = "curve")
```
![](/assets/blog_res/ZX/3.svg)
```julia
# simplify with pivoting rule
simplify!(Rule{:p1}(), zxg)
ZXplot(zxg, linetype = "curve")
```
![](/assets/blog_res/ZX/4.svg)
```julia
# removing all Paulis adjancent to boundaries
replace!(Rule{:pab}(), zxg)
ZXplot(zxg, linetype = "curve")
```
![](/assets/blog_res/ZX/5.svg)
```julia
# extract circuit
cir = circuit_extraction(zxg)
ZXplot(cir)
```
![](/assets/blog_res/ZX/6.svg)
Finally we got the simplified circuit.

## What I have done

In the past phases during GSoC 2020, I have built up the main part of `ZXCalculus.jl`, including
* data structures for representing ZX-diagrams,
* rewriting rules,
* circuits simplification algorithms [^3],[^4],
* APIs for constructing ZX-diagrams from quantum gates,
* basic visualization of ZX-diagrams.

With the above functions, one can construct a circuit and simplify it. This is our main goal before the first evaluation. In the next phase, we will focus on the integration of `ZXCalculus.jl` and `YaoLang.jl`.

[^1]: ["NON-IDENTITY-CHECK" IS QMA-COMPLETE](https://www.worldscientific.com/doi/abs/10.1142/S0219749905001067)
[^2]: [Exact Non-identity check is NQP-complete](https://arxiv.org/abs/0903.0675)
[^3]: [Graph-theoretic Simplification of Quantum Circuits with the ZX-calculus](https://arxiv.org/abs/1902.03178)
[^4]: [Reducing T-count with the ZX-calculus](https://arxiv.org/abs/1903.10477)
