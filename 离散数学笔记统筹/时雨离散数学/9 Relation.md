# 9.Relation

注意看关系的定义，计数不同性质关系(reflexive, symmetric, antisymmetric)的, 偏序关系

## Definition

> A binary relation from 𝐴 to 𝐵 is a subset of 𝐴×𝐵

The graph of function 𝑓:𝐴→𝐵is a relation. 但反之不成立

> A relation on a set A is a relation from A to A

A relation can be expressed as

- Connection matrix $𝑚_𝑖j=[(𝑎𝑖,𝑏𝑗)∈𝑅]$ 可以理解成图的邻接矩阵,只有0和1
- Directed graphs

## Properties

| Properties    | Definition                                      | Connection Matrix           | Digraph                                   |
| ------------- | ----------------------------------------------- | --------------------------- | ----------------------------------------- |
| reflexive     | ∀𝑥∈𝐴,(𝑥,𝑥)∈𝑅                               | 对角线全为1                      | 每个结点都有自环                                  |
| irreflexive   | ∀𝑥∈𝐴,(𝑥,𝑥)∉𝑅                               | 对角线全为0                      | 每个结点都没有自环                                 |
| symmetric     | ∀𝑥,𝑦∈𝐴,(𝑥,𝑦)∈𝑅→(𝑦,𝑥)∈𝑅                 | 𝑀=𝑀𝑇                     | 每一条边都有反向边                                 |
| antisymmetric | ∀𝑥,𝑦∈𝐴,((𝑥,𝑦)∈𝑅∧(𝑦,𝑥)∈𝑅)→𝑥=𝑦         | 对角线可以为0/1, 非对角线上的对称位置不能同时为1 | 每一条边都没有反向边，**可以有自环**                      |
| asymmetric    | ∀𝑥∈𝐴,(𝑥,𝑦)∈𝑅→(𝑦,𝑥)∉𝑅                    | 对角线全为0,非对角线上的对称位置不能同时为1     | 每一条边都没有反向边,**不能有自环**                      |
| transitive    | ∀𝑥,𝑦,𝑧∈𝐴((𝑥,𝑦)∈𝑅∧(𝑦,𝑧)∈𝑅)→(𝑦,𝑧)∈𝑅) | 𝑀2∨𝑀=𝑀                   | 对于每一条从𝑢到𝑣长度为2的路径，有一条边(𝑢,𝑣). (即𝑅2⊆𝑅) |


- 也就是说如果且(𝑥,𝑦)∈𝑅且𝑥≠𝑦则(𝑦,𝑥)∉𝑅 . 如果R是antisymmetric的，那么可以包括(a,a).

A relation can be both symmetric and antisymmetric: $$𝑀=\begin{vmatrix}1&0\\0&1\\\end{matrix}$$

> symmetric + transitive=reflexive?

No, for example 𝑀=0

错在symmetric不代表对于任意的𝑎都存在𝑏使得(𝑎,𝑏)∈𝑅

#### Counting relations on set

> |A|=n, how many relation on set A is
> 
> - reflexive: 2𝑛2−𝑛
> - symmetric: 2(𝑛2−𝑛)/2×2𝑛=2(𝑛(𝑛+1)/2) 注意对角线上可以是0或1
> - **antisymmetric**: 3𝑛(𝑛−1)/2×2𝑛 对于矩阵非对角线上的一对对称的元素, 只能是(0,0),(0,1),(1,0). 对角线上可以是0或1
> - asymmetric 3𝑛(𝑛−1)/2，对角线上全为0

### Operations

> 𝑅−1={(𝑏,𝑎)|(𝑎,𝑏)∈𝑅}

𝑀𝑅−1=𝑀𝑅𝑇

> 𝑆∘𝑅={(𝑎,𝑐)|(𝑎,𝑏)∈𝑅∧(𝑏,𝑐)∈𝑆}

**注意顺序，S在后面！！！！**

对应邻接矩阵的乘法(用与运算代替乘法，或运算代替加法)

𝑀𝑆∘𝑅=𝑀𝑅×𝑀𝑆

𝑅𝑛=𝑅∘𝑅∘…𝑅

或者对应有向图中长度小于等于n的路径(Floyd传递闭包（确信))

> The relation R on a set A is transitive if and only if 𝑅𝑛⊆𝑅 for n = 1, 2, 3,...

### Closures

> If there is a relation S with property P containing R, such that S is a subset of every relation with property P containing R, then S is called the closure of R with respect to P

证明分3步

- 𝑅⊆𝑆
    
- 𝑆 满足性质𝑃
    
- 证明S是最小的，可以反证，或者证明任意一个满足P的关系都包含S
    
    ![image-20230530095535513](https://birchtree2.github.io/assets/images/image-20230530095535513.png)
    

reflexive closure: 𝑅∪𝐼𝑎

symmetric closure:𝑅∪𝑅−1

transitive closure: ∪𝑖=1∞𝑅𝑖

Warshall's Algorithm

手算，每次取第k行和第k列. 用第k列(`a[i][k]`)和第k行`a[k][j]`两两匹配,如果有2个都是1的,就把`a[i][j]`设为

## Equivalence Relations

**reflexive, symmetric and transitive**-> equivalence relation

#### Equivalence class

Equivalence class of 𝑥: [𝑥]𝑅 or [𝑥] The set of all elements that are related to an element 𝑥

> Eg. 𝑅={(𝑎,𝑏)|𝑎≡𝑏(mod3)}

[0]={3𝑘|𝑘∈𝑍},[1]={3𝑘+1|𝑘∈𝑍},[2]={3𝑘+2|𝑘∈𝑍}

#### Partitions of a set and Relations
*集合的分划*

> Theorem![a71d20e55427d08766a69c287943dd0f.png](https://birchtree2.github.io/assets/images/a71d20e55427d08766a69c287943dd0f.png) 左推右很好证。考虑右推左

𝑅={(𝑎,𝑏)|𝑎,𝑏∈𝐴𝑖} 𝑝𝑟(𝐴)=𝐴1,𝐴2…𝐴𝑛

- reflexive: ∀𝑎∈𝐴,\existssss𝐴𝑖,𝑎∈𝐴𝑖
- symmetric
- transitive: 𝑎𝑅𝑏,𝑏𝑅𝑐, 𝑎,𝑏∈𝐴𝑖  𝑏,𝑐∈𝐴𝑗. because 𝐴𝑖∩𝐴𝑗=∅(𝑖≠𝑗), so 𝑖=𝑗

[4a48fe0446513dfdb3c3cc67cb93c5e7.png](https://birchtree2.github.io/_resources/4a48fe0446513dfdb3c3cc67cb93c5e7.png)

> |𝐴|=𝑛, how many different equivalence relations on the set 𝐴 are there

Consider the partition of A. ∑𝑘=0𝑛𝑆(𝑛,𝑘)

#### Operation of equivalence relations

𝑅1,𝑅2 is equivalence relation, 𝑅1∪𝑅2 is not

![ee1f7479a56640ef06df0847fd708fda.png](https://birchtree2.github.io/assets/images/ee1f7479a56640ef06df0847fd708fda.png)

因为不满足Transitive, 对𝑅1∪𝑅2 再跑传递闭包就可以

## Partial Orderings
*偏序关系*
a **reflexive, antisymmetric and transitive** relation is a partial order.

- 验证是partial order,前两项容易，注意transitive relation要枚举3个点

S is a set, R is a partial order on S, (𝑆,𝑅) is called a **poset**

The elements a and b of a poset (𝑆,≤) are called **comparable** if either 𝑎≤𝑏 or 𝑏≤𝑎. (这里的≤代表抽象的大小关系，不是实数的比大小)

If (𝑆,≤) is a poset and every two elements of S are comparable, S is called a totally ordered or linearly ordered set, or a chain.

#### Hasse Diagrams

先把偏序关系表示成有向图，根据传递性去掉不需要的边。再分层画出节点，去掉箭头。

![5ceac7f835e6d568a49b66c276c061ae.png](https://birchtree2.github.io/assets/images/5ceac7f835e6d568a49b66c276c061ae.png)

![5d015e38a865247e1e8f11417b8973cb.png](https://birchtree2.github.io/assets/images/5d015e38a865247e1e8f11417b8973cb.png)

画(𝐴,|)的Hasse diagram，可以不必用通用的方法

1. 如果有1,则1在最底部
2. 接下来一层是质数
3. 依次放上质数的倍数

#### Chain and antichain

A subset of a poset is called a chain if every two elements of this subset are comparable.

A subset of a poset is called an **antichain** if **every two elements** of this subset are incomparable

> Dilworth's theorem:
> 
> every finite poset can be partitioned into k chains, where k is the largest number of elements in an antichain in this poset

#### Special Elements

- maximal element 𝑚: 不存在𝑎∈𝑆,𝑎≥𝑚 （可能有多个，也可能不存在) (top element of Hasse diagram)
    
- minimal element: (bottom element of Hasse diagram)
    
- greatest element: ∀𝑎∈𝑆,𝑚≥𝑎 (唯一)
    
- least element: (唯一)
    
- upper bounds: for 𝐴⊆𝑆, \existssss𝑎,∀𝑏∈𝐴,𝑎≥𝑏, then 𝑎 is upper bound of 𝐴
    
    - least upper bound(lub)
- lower bounds
    
    - greatest lower bound(glb).

![165b7177d8e14eb3fc8d55cce93fc393.png](https://birchtree2.github.io/assets/images/165b7177d8e14eb3fc8d55cce93fc393.png)

#### Lattice

a poset is called a lattice if **each pair** of elements have a glb and lub.

every totally ordered set is a lattice

**(𝑍+,|) is a lattice. (glb=两个数的最大公因数，lub=最小公倍数) (𝑃(𝑆),⊆) is a lattice (取并集、交集)**

#### Well-ordered set

a well-ordered set is a poset that every nonempty subset of A has a least element. a well-ordered set is a totally ordered set.(choose subset of size 2)

mathmatical induction(见Section 5)

#### Topological Sorting