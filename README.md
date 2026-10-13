<!-- solve-problem:8f7a68d3-cb4e-4da6-942d-9d50d69ed6e5 -->
# Existence of a dynamically optimal online binary search tree

A [Tangle Solve problem](https://solve.tangle.tools/problems/8f7a68d3-cb4e-4da6-942d-9d50d69ed6e5).

## What is the problem?

A **binary search tree (BST)** on keys {1, …, n} serves an access sequence σ = (σ₁, …, σₘ); each access starts at the root and follows a pointer-walk down the tree. Between accesses, the algorithm may perform **rotations** at nodes it has just touched (the standard BST model: rotations are only allowed on the access path, at unit cost each, and pointer starts at root each access). The cost of an execution is the total number of node touches plus rotations.

- **Offline optimum, OPT(σ):** the minimum cost of *any* BST algorithm (even one knowing σ in advance) on σ.
- **Online BST:** must decide rotations without seeing future accesses.
- **Dynamically optimal BST:** an explicit online BST algorithm A and a universal constant C such that for all n and all σ,
  cost_A(σ) ≤ C · OPT(σ) + O(n) (the additive term covers initialization).

Candidates conjectured to be dynamically optimal: **splay trees** (Sleator–Tarjan) and the **Geometric Greedy / Greedy algorithm** (independently discovered by Lucas, Munro, and Demaine–Harmon–Iacono– Pătraşcu). Both are known to satisfy many desirable properties (working-set bounds, static optimality, dynamic finger, etc.) but constant-competitiveness is unproven for both.

Ambiguity note: the precise cost model (access-path rotations only vs. arbitrary rotations, pointer memory between accesses) matters; the standard formalization is the **geometric BST model** of Demaine et al. (2007), in which the known upper bounds are cleanest.

## Why does it matter?

It is arguably the flagship open problem in comparison-based data structures. A resolution would settle whether a single self-adjusting search structure adapts optimally to *every* access pattern (locality, working sets, finger structure) simultaneously — with consequences for caches, database indexes, and any dictionary facing adversarial workloads. It would also resolve the dynamic optimality conjectures for splay trees and Greedy, which have driven 40 years of data-structure theory. Progress tools (e.g., the geometric view, Wilber-type lower bounds) have already proven useful elsewhere.

## What's already known?

- **Splay trees (Sleator–Tarjan 1985):** amortized O(log n) worst case; static optimality, working-set bound, dynamic-finger (Cole et al.), static-finger, sequential access in O(n). Each of these matches OPT on restricted sequence families, but no unified constant-factor bound.
- **Wilber's lower bounds (1986):** two crossing-sensitive lower bounds in the geometric model; any online BST respecting them is O(1)-competitive. **Tango trees (DHIP 2007):** O(log log n)-competitiveness via a refinement of Wilber's first bound — best known competitive ratio.
- **Chain decomposition / multi-tango variants (Derryberry et al., 2009-ish)** improved constants but not the log log n barrier.
- **Greedy (geometric) BST:** proven to satisfy working-set and affine-analysis bounds; shown equivalent in the geometric model to "GreedyASS"; its executions are *exactly* the independence-based access paths. Recent progress via **matrix ansatz / geometric inclusions** (Chalermsook, Goswami, Kozma, Mehlhorn, Saranurak and others, ~2015–2020) proved Greedy beats several long-standing barriers, e.g., it is "lazy" and satisfies improveddeque-like bounds — but constant competitiveness remains open.
- **Why approaches fall short:** Wilber's bounds are known to be too weak — Tango trees exactly meet Wilber-1 yet are not constant-competitive; the optimal algorithm must exploit structure no known lower bound captures.

## What would count as solving it?

- **Positive:** an explicit online BST algorithm A and constant C with a proof that cost_A(σ) ≤ C·OPT(σ) + O(n) for all sequences in the standard (geometric or rotation) BST model.
- **Negative/separation:** a proof that no online BST achieves constant competitiveness (e.g., an asymptotic separation between the best online ratio and 1 — even showing the ratio is ω(1) or ≥ c log log n for explicit sequences would be major).
- **Intermediate milestones a referee would value:** proving GreedyFuture/Greedy is O(1)-competitive; proving splay is O(1)-competitive; any o(log log n)-competitive algorithm; or a stronger-than-Wilber offline lower bound that some algorithm matches.

## How to start

1. Read Demaine, Harmon, Iacono, Pătraşcu, *Dynamic Optimality—Almost* (SODA 2004 / SIAM J. Comput. 2007) for the geometric BST model, Wilber's bounds, and Tango trees — this is the required formal framework.
2. Read Sleator–Tarjan 1985 for the splay conjecture and the catalogue of special-case bounds.
3. Study the geometric Greedy algorithm and the Chalermsook et al. line of work on Greedy's analysis (access-lemma style arguments, geometric inclusions, matrix/potential ansätze) — the most active attack path.
4. Main obstacle: all known analyses are instance-specific (working set, finger, etc.); a competitive analysis needs a single potential/argument dominating OPT itself. A natural angle: find a combinatorial characterization of OPT's geometric executions and show Greedy's arborally satisfied superset property implies constant-factor domination — this is exactly the (open) "arboral satisfaction vs. BST execution" gap.

## Key references

- D. Sleator, R. Tarjan, *Self-Adjusting Binary Search Trees*, J. ACM 32(3), 1985. Introduces splay trees and poses dynamic optimality; proves amortized bounds (static optimality, working set). https://doi.org/10.1145/3828.3835
- E. Demaine, D. Harmon, J. Iacono, M. Pătraşcu, *Dynamic Optimality—Almost*, SIAM J. Comput. 37(1), 2007. Defines the geometric BST model, reinterprets Wilber's lower bounds, and gives Tango trees with O(log log n) competitiveness. https://doi.org/10.1137/S0097539705447347
- R. Wilber, *Lower Bounds for Accessing Binary Search Trees with Rotations*, SIAM J. Comput. 18(1), 1989. Two lower-bound funnels on BST access costs; the foundation of all competitiveness proofs. (verify year/issue)
- P. Chalermsook, M. Goswami, L. Kozma, K. Mehlhorn, T. Saranurak, *The Mango Tree: A Case for Trees in the Buffer Tree*, and related Greedy-analysis papers (~2015–2020), e.g., *Greedy is an Optimally Competitive Algorithm for Non-Uniform Paged Access* (verify exact titles/venues) — modern geometric analyses of Greedy.
- (For special cases) R. Cole, B. Mishra, J. Schmidt, A. Siegel, *On the Dynamic Finger Conjecture for Splay Trees*, SIAM J. Comput. 30(1), 2000 — proves the dynamic finger theorem for splay trees. (verify)

## Warnings

- **Model sensitivity:** competitiveness claims are model-relative; the rotation-BST model and the geometric model differ by constant factors, but sloppy translations cause errors. State the model explicitly.
- **Wilber-1 is a dead end:** Tango trees already match it exactly; any new algorithm must beat Wilber's bounds, so don't build on them directly.
- Amortized-vs-worst-case: dynamic optimality is inherently amortized; don't try for worst-case per-access bounds.
- Beware "proofs" that only show cost ≤ C·(known lower bound on OPT) for sequences where that lower bound is tight — this is the classic failure mode. Also verify the Chalermsook et al. citations above; the Greedy-analysis literature is fast-moving and I may have imprecise titles.

---
*`ALGO-000085` · unverified*

## Acceptance guidance

- **Positive:** an explicit online BST algorithm A and constant C with a proof that cost_A(σ) ≤ C·OPT(σ) + O(n) for all sequences in the standard (geometric or rotation) BST model.
- **Negative/separation:** a proof that no online BST achieves constant competitiveness (e.g., an asymptotic separation between the best online ratio and 1 — even showing the ratio is ω(1) or ≥ c log log n for explicit sequences would be major).
- **Intermediate milestones a referee would value:** proving GreedyFuture/Greedy is O(1)-competitive; proving splay is O(1)-competitive; any o(log log n)-competitive algorithm; or a stronger-than-Wilber offline lower bound that some algorithm matches.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/8f7a68d3-cb4e-4da6-942d-9d50d69ed6e5?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000085.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000085)
