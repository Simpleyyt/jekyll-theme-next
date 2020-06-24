---
layout: post
title: Quantum circuits simplification and ZX-calculus
description: Implement ZX-calculus with Julia.
---

Quantum computing uses the principles of quantum mechanics for computing. To describe quantum algorithms, we often use the quantum circuit model which is the analog of the classical logic circuit model. Quantum circuits are consist of a set of basic quantum gates such as Pauli X, Y, Z gate, Hadamard gate, T gate, CNOT gate.

In general, there will be lots of equivalent quantum circuits representing the same operation. At the viewpoint of quantum hardware, one wishes to find the circuit that minimizing the usage of hardware resources. Unfortunately, finding the most desired circuit is a very hard problem in computational complexity, as checking the equivalence between two quantum circuits is coQMA-hard (quantum analog of coNP). Despite this, we still have some heuristic efficient algorithms for quantum circuits simplification. In this blog post, I will introduce a powerful tool for this problem, ZX-calculus.

A quantum circuit is a graph representing tensor products and matrices products. For example, this is the circuits representing 
$$
\mathrm{CNOT}(2,3)\cdot\mathrm{CNOT}(1,2)\cdot(H\otimes I\otimes I).
$$
![](\assets\blog_res\ZX\cir1.png)

Also, quantum circuits can be regarded as a special type of tensor network. In ZX-calculus, we introduce two kinds of tensors, Z-spiders, and X-spiders. Tensor networks that consist of these tensors are called ZX-diagrams. As all basic quantum gates can be represented by ZX-diagrams, we can transform all quantum circuits to ZX-diagrams. 
![](\assets\blog_res\ZX\zxd1.svg)
Moreover, by the definition of Z-spiders and X-spiders, there are basic equivilant rules with which one can simplify ZX-diagrams. 
![rules](\assets\blog_res\ZX\rules.png)

After simplifying ZX-diagrams, we should transform them back to circuits. In the paper [arXiv:1902.03178](https://arxiv.org/abs/1902.03178), they developed algorithms for the above scheme. We will use Julia to implement this scheme for simplifying quantum circuits.

## Data structures

To implement these algorithms, we need to implement data structures for storing ZX-diagrams and rules on how ZX-diagrams change. A ZX-diagram can be regarded as a multigraph with extra information on its vertices. Each vertex is one of Z-spider, X-spider, or H-box. In the original ZX-diagrams, open edges are allowed. For simplicity, we added 2 special kinds of vertices, input boundary, and output boundary, for recording these edges. We use the adjacency list representation for storing multigraphs. An adjacency list is stored as `Dict` whose keys are the ids of vertices. 
```julia
struct Multigraph{T<:Integer} <: AbstractMultigraph{T}
    adjlist::Dict{T, Vector{T}}
end
```
Because we need to rewrite multigraphs with ZX-calculus rules, it will be more convenient to fix vertices ids. So we use `Dict` instead of `Array`. There are phases for Z-spiders and X-spiders. We need another Dict for storing these phases. The data struct of ZX-diagram is similar as follow:
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

## Rewriting rules

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
And `rewrite!` will try to use a rule to rewrite a ZX-diagram on the all matched vertices. Since it is possible that some matched vertices become unsatisfied with the corresponding rule after rewriting some vertices. We need to check whether a rule is still available on matched vertices. 
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

In the simplification scheme of [arXiv:1902.03178](https://arxiv.org/abs/1902.03178), we need to convert a ZX-diagram to a graph-like ZX-diagram with rules i1, i2, h and f. Then simplify the graph-like ZX-diagram with local complementary rule and pivoting rule. We can accomplish these steps with above functions. Simplified graph-like ZX-diagrams will be only small skeletons comparing to original large circuits. The only thing remained is extracting circuits from ZX-diagrams.

## Circuit extraction

This is the most complicated part of the simplification scheme. For more detail, one can read the original paper. I will only explain the circuit extraction algorithm briefly.

There is a property for any ZX-diagram of a quantum circuit. That is we can define a g-flow on it. With a g-flow, we can know the order of all spiders. The rules we used for simplifying the ZX-diagram will preserve this good property. It gives us the possibility to extracting circuits from simplified ZX-diagrams. 

Moreover, any graph-like ZX-diagram are CNOT + H circuits locally. We can extract these CNOT circuits with gaussian elimination over $F_2$. Together with the remaining H gates, CZ gates and Z-rotations, we get a circuit equivilant to the original one.

## Demo

The demo circuit is from the appendix of the paper [arXiv:1902.03178](https://arxiv.org/abs/1902.03178). All the codes can be find in `examples\ex1.jl`.
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
