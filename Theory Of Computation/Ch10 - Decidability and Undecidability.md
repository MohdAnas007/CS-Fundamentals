# Chapter 10: Decidability and Undecidability

## Introduction

Computability theory classifies problems according to whether they can be solved algorithmically. A problem is *decidable* if there exists a Turing machine (TM) that always halts with a correct yes/no answer. Problems that lack such a machine are *undecidable*. This chapter establishes the fundamental hierarchy of languages, proves the existence of undecidable problems via diagonalisation, and develops reduction techniques for transferring undecidability. We conclude with Rice’s Theorem and canonical undecidable problems: emptiness, regularity, and the Post Correspondence Problem.

---

## 1. Recursive (Decidable) vs. Recursively Enumerable Languages

### Definitions

> [!NOTE]
> The following definitions distinguish decidable and recursively enumerable languages.


- A language `L subseteq Sigma*` is **recursive (decidable)** if there exists a Turing machine `M` such that:
    - For every `w in Sigma*`, `M` halts.
    - `M` accepts `w` iff `w in L`.
    - `M` rejects `w` iff `w notin L`.

- A language `L` is **recursively enumerable (r.e.)** if there exists a Turing machine `M` such that:
    - `M` accepts `w` iff `w in L`.
    - For `w notin L`, `M` may either reject or loop forever (that is, it need not halt).

### Relationship

- Every decidable language is r.e., but the converse is false.
- A language is decidable iff both it and its complement are r.e.
- There exist r.e. languages whose complement is not r.e.; these are strictly undecidable.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f0f0f0', 'primaryTextColor': '#000', 'primaryBorderColor': '#333', 'lineColor': '#333', 'tertiaryColor': '#e0e0e0'}}}%%
flowchart TD
    A[All Languages over Σ] --> B[Non-R.E. Languages]
    A --> C[R.E. Languages]
    C --> D[Decidable Languages]
    C --> E[R.E. but not Decidable]
    D --> F[Co-R.E. Languages]
    note1["A language is decidable ⇔ it is R.E. and Co-R.E."]
    style D fill:#b3d9ff,stroke:#333
    style E fill:#ffb3b3,stroke:#333
    style F fill:#d9b3ff,stroke:#333
```

---

## 2. Decidable and Undecidable Languages – Formal Characterisation

A decision problem `P` is identified with the language `L_P = { w | w is a positive instance of P }`.

- **Decidable**: There exists a TM `M` that, for every input, halts in an accepting or rejecting state. Membership is computationally tractable in principle.
- **Undecidable**: No such TM exists. For any candidate TM, there is at least one input on which it either gives the wrong answer or fails to halt.

**Example of a Decidable Problem:**  
*Does a given DFA accept the empty language?* This is decidable by checking reachability of accepting states.

**Example of an Undecidable Problem:**  
*Does a given TM halt on a given input?* – the Halting Problem.

---

## 3. The Halting Problem – Statement and Diagonalisation Proof

### Problem Statement

Define the Halting Language:

```text
H = { <M, w> | M is a Turing machine and M halts on input w }
```

**Theorem:** `H` is undecidable.

### Proof by Diagonalisation

Assume, for contradiction, that `H` is decidable. Then there exists a TM `H_dec` that, on input `<M, w>`, halts and accepts if `M` halts on `w`, and rejects otherwise.

Construct a new TM `D` as follows:
1. On input `<M>` (a description of a TM), run `H_dec` on `<M, <M>>`.
2. If `H_dec` accepts (that is, `M` halts on its own description), then `D` loops forever.
3. If `H_dec` rejects (that is, `M` does not halt on its own description), then `D` halts.

Now consider the behavior of `D` on its own description `<D>`:
- If `D` halts on `<D>`, then by construction (step 2) it must loop - contradiction.
- If `D` loops on `<D>`, then by construction (step 3) it must halt - contradiction.

Hence `H_dec` cannot exist, so `H` is undecidable.

### Mermaid Flowchart
```mermaid
flowchart TD
    subgraph Assumption
        A["H_dec exists"] --> B["H_dec(<M,w>) accepts iff M halts on w"]
    end
    subgraph Construction["Construct D"]
        C["D on <M>"] --> D["Run H_dec(<M, <M>>)"]
        D -->|accepts| E["D loops forever"]
        D -->|rejects| F["D halts"]
    end
    subgraph Contradiction["Apply D to <D>"]
        G["D on <D>"] --> H["Run H_dec(<D, <D>>)"]
        H -->|accepts| I["D loops"] 
        H -->|rejects| J["D halts"]
        I --> K["But if H_dec accepts, D should halt (by construction) → contradiction"]
        J --> L["But if H_dec rejects, D should loop (by construction) → contradiction"]
    end
    B --> C
    C --> G
```

---

## 4. Reductions and Reduction Techniques

### Many-One (Mapping) Reductions

A language `A` is **many-one reducible** to language `B` (notation `A <=m B`) if there exists a computable total function `f: Sigma* -> Sigma*` such that:

```text
For every w, w in A iff f(w) in B.
```

**Properties:**
- If `A <=m B` and `B` is decidable, then `A` is decidable.
- If `A <=m B` and `A` is undecidable, then `B` is undecidable.
- The reduction preserves membership status; it is a "yes-instance maps to yes-instance, no-instance maps to no-instance" transformation.

### Mermaid Diagram
```mermaid
flowchart LR
    subgraph Domain
        w1["w ∈ A"] --> f1["f(w) ∈ B"]
        w2["w ∉ A"] --> f2["f(w) ∉ B"]
    end
    subgraph Computable["Computable f"]
        f["f : Σ* → Σ*"]
    end
    w1 --> f
    w2 --> f
    f --> f1
    f --> f2
    B_Dec["B decider"] -->|accept/reject| f1
    B_Dec -->|accept/reject| f2
```

**Example Reduction:**  
The problem *Does a TM accept the empty string?* can be reduced from the Halting Problem. Given `<M, w>`, construct a new TM `M_w` that ignores its input, writes `w` on the tape, and simulates `M` on `w`. Then `M_w` halts on empty input iff `M` halts on `w`. Thus `H <=m EMPTY_TM`, proving the latter undecidable.

---

## 5. Rice’s Theorem

### Statement

Let `P` be a set of recursively enumerable languages (that is, a property of r.e. languages). Suppose:
1. `P` is non-trivial: there exists at least one r.e. language `L1` that satisfies `P`, and at least one r.e. language `L2` that does not satisfy `P`.
2. `P` is a property of the language itself, not of the particular TM description (that is, if `L(M1) = L(M2)`, then `P` holds for `M1` iff it holds for `M2`).

Then the problem of deciding, for an arbitrary TM `M`, whether `L(M) in P`, is undecidable.

### Proof Sketch

Assume `P` is non-trivial. Let `L_empty` be the empty language (which may or may not satisfy `P`). We reduce from the Halting Problem.

- If the empty set is not in `P`, pick an r.e. language `L_P in P`. Given `<M, w>`, construct a TM `M'` that, on input `x`:
    1. Simulates `M` on `w`.
    2. If `M` halts, simulate a TM for `L_P` on `x` and accept iff that accepts.
    Then `M'` accepts exactly `L_P` if `M` halts on `w`, otherwise it accepts the empty language. Therefore `M' in P` iff `M` halts on `w`, so `H <=m Property_P`.

- If the empty set is in `P`, take its complement property (which is also non-trivial) and apply the same logic.

Thus any non-trivial, extensional property is undecidable.

### Consequences

Rice's Theorem immediately implies the undecidability of:
- Emptiness: `L(M) = empty set`?
- Finiteness: `L(M)` finite?
- Regularity: `L(M)` regular?
- Context-freeness, etc.

All are undecidable for arbitrary TMs.

### Mermaid Diagram
```mermaid
flowchart TD
    subgraph Assumption["Assume P is non-trivial"]
        L1["L1 ∈ P"] 
        L2["L2 ∉ P"]
    end
    subgraph Reduction["From H to Property P"]
        Input["<M,w>"] --> Build["Build M'"]
        Build --> Sim["M' on x: simulate M on w; if halts, simulate TM for L1 on x"]
        Sim --> Case1["If M halts on w → L(M') = L1 ∈ P"]
        Sim --> Case2["If M does not halt on w → L(M') = ∅ ∉ P (or opposite)"]
        Case1 --> Yes["M' has property P iff M halts on w"]
        Case2 --> Yes
    end
    Yes --> H_reduce["H ≤m Property_P"]
    H_reduce --> Undec["Property P is undecidable"]
```

---

## 6. Proofs of Undecidability for Emptiness, Regularity, and PCP

### 6.1 Emptiness Problem

**Problem:** Given a TM `M`, is `L(M) = empty set`? (Language: `E_TM = {<M> | L(M) = empty set}`)

**Proof of Undecidability:** Reduce from the Halting Problem. For any `<M, w>`, construct a TM `M_w`:
- On input `x`, `M_w` ignores `x` and simulates `M` on `w`.
- If `M` halts on `w`, `M_w` accepts `x` (so `L(M_w) = Sigma*`).
- If `M` does not halt on `w`, `M_w` accepts nothing (so `L(M_w) = empty set`).

Then:
```text
<M, w> in H iff L(M_w) is not empty iff <M_w> not in E_TM.
```
Thus `H <=m complement(E_TM)`. Since `complement(E_TM)` is undecidable (as the complement of a decidable language would be decidable), `E_TM` is undecidable.

### 6.2 Regularity Problem

**Problem:** Given a TM `M`, is `L(M)` regular? (Language: `R_TM = {<M> | L(M) is regular}`)

**Proof of Undecidability:** Reduce from the Halting Problem. For `<M, w>`, construct a TM `M'` that, on input `x`:
- If `x` is of the form `0^n 1^n` (a non-regular pattern), accept `x` immediately.
- Otherwise, simulate `M` on `w`. If `M` halts, accept `x`; else loop.

Now analyse `L(M')`:
- If `M` halts on `w`: `L(M') = Sigma*` (all strings accepted) - regular.
- If `M` does not halt on `w`: `L(M') = {0^n 1^n | n >= 0}` - non-regular.

Therefore:
```text
<M, w> in H iff <M'> in R_TM.
```
Hence `H <=m R_TM`, so `R_TM` is undecidable.

### 6.3 Post Correspondence Problem (PCP)

**Problem Statement:**  
An instance of PCP consists of a finite set of dominoes (tiles) of the form `[u_i / v_i]` where `u_i, v_i` are strings over some alphabet `Sigma`. The question is: does there exist a finite sequence of indices `i1, i2, ..., ik` (with repetitions allowed) such that
```text
u_i1 u_i2 ... u_ik = v_i1 v_i2 ... v_ik
```
The top string equals the bottom string.

**Theorem:** PCP is undecidable.

**Proof Outline:** The proof reduces from the Halting Problem via a simulation of a Turing machine's computation history. The idea is:

1. Given a TM `M` and input `w`, construct a set of dominoes that encode the initial configuration, the transition rules, and the final accepting configuration of `M` on `w`.
2. The dominoes are designed so that a match exists iff `M` halts on `w`. The match essentially produces a sequence of configurations separated by markers, simulating the computation step by step.
3. The construction forces the top and bottom strings to be identical only when the simulated computation reaches an accepting state.

This reduction is intricate but standard. The critical point is that the existence of a match is equivalent to the existence of a finite halting computation.

### Mermaid Diagram
```mermaid
flowchart LR
    subgraph Dominoes["Domino Set Construction"]
        D1["start: [# / #q0w#]"]
        D2["copy: [a / a] for all a"]
        D3["transition: [q a / b q'] if δ(q,a)=(q',b,R)"]
        D4["transition: [c q a / q' c b] if δ(q,a)=(q',b,L)"]
        D5["accept: [q_accept / ]"]
        D6["cleanup: [a / ]"]
    end
    subgraph Match["Existence of a Match"]
        Top["Top string: concatenated u_i"]
        Bottom["Bottom string: concatenated v_i"]
        Top --"equal iff M halts"--> Bottom
    end
    Dominoes --> Match
    Match --> H_Red["H ≤m PCP"]
```

**Key Insight:** The match forces a sequence of configurations where each configuration's boundary is synchronised. The undecidability of PCP is a powerful tool because it is a "pure string rewriting" problem with no explicit machine model, yet it captures all computational power.

---

> [!IMPORTANT]
> The summary table below is an excellent last-minute revision aid.

## Summary Table

| Problem | Language | Decidability | Technique |
|---------|----------|--------------|-----------|
| Halting Problem | `H = {<M,w> | M halts on w}` | Undecidable | Diagonalisation |
| Emptiness | `E_TM = {<M> | L(M) = empty set}` | Undecidable | Reduction from H |
| Regularity | `R_TM = {<M> | L(M) is regular}` | Undecidable | Reduction from H |
| Post Correspondence | `PCP = {set of dominoes | exists match}` | Undecidable | Reduction from H (computation history) |

Rice's Theorem subsumes the first two and many others, but PCP stands apart because it does not directly involve TMs, yet it is a quintessential undecidable problem in formal language theory.

---

## Conclusion

Decidability and undecidability form the bedrock of theoretical computer science. The diagonalisation proof for the Halting Problem established the existence of unsolvable problems. Reductions provide a systematic method to propagate undecidability. Rice's Theorem offers a sweeping generalisation: any non-trivial semantic property of r.e. languages is undecidable. Finally, problems like emptiness, regularity, and PCP illustrate the breadth of undecidable phenomena, from machine properties to pure combinatorics on strings. These results compel us to recognise the inherent limits of algorithmic computation, shaping our understanding of what is computable in principle.