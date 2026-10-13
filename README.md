<!-- solve-problem:ed8e3dd5-de52-4aa9-84c6-44b68f698122 -->
# Classical polynomial-time prime-field discrete logarithms

A [Tangle Solve problem](https://solve.tangle.tools/problems/ed8e3dd5-de52-4aa9-84c6-44b68f698122).

## What is the problem?

Given a prime \(p\), a generator (or arbitrary element) \(g \in \mathbb{F}_p^*\), and a target \(h \in \mathbb{F}_p^*\), the discrete logarithm problem (DLP) asks for an integer \(x\) such that \(g^x \equiv h \pmod{p}\). The open question: does there exist a **classical (non-quantum) deterministic or randomized algorithm** that computes \(x\) for *every* prime \(p\) in time polynomial in \(\log p\) — i.e., \((\log p)^{O(1)}\) bit operations?

Formal setting: input is the triple \((p, g, h)\) in binary, \(\log h \le \log p\); output is \(x \in [0, \operatorname{ord}(g))\). The group \(\mathbb{F}_p^*\) is cyclic of order \(p-1\). No restriction on \(p\) — it must work for *arbitrary* primes, not just primes of special form (e.g., not just \(p = 2^n \pm k\), smooth \(p-1\), or Kummer primes).

Ambiguity note: "polynomial time" most reasonably means polynomial in the input length \(\log p\), in the standard Turing/RAM bit-complexity model. Randomized algorithms with bounded error would already be a major breakthrough; the census does not restrict to deterministic. This is the multiplicative-group case; extension fields \(\mathbb{F}_{p^n}\) are a related but distinct problem.

## Why does it matter?

A classical polynomial-time DLP algorithm would break Diffie–Hellman key exchange, DSA/ECDSA's finite-field ancestors, ElGamal encryption, and any system whose hardness rests on DLP — a foundational collapse for cryptography comparable to factoring falling. On the positive side, it would likely transform computational number theory (e.g., faster algorithms for related algebraic problems). If DLP is provably hard for some natural complexity class, that would be the first strong lower bound of its kind; currently we cannot even prove DLP is not in P.

## What's already known?

- **Generic algorithms** (Shanks's baby-step giant-step, Pollard's rho): \(O(\sqrt{p})\) time — exponential in \(\log p\). A matching lower bound \(\Omega(\sqrt{q})\) exists in the *generic group model* (Shoup, 1997), so any polynomial algorithm must exploit the representation of \(\mathbb{F}_p^*\).
- **Index calculus** (Adleman 1979; Coppersmith, Odlyzko, Schroeppel for prime fields; the Number Field Sieve DLP, Gordon 1993; Joux–Lercier 2006 refinement): heuristically \(L_p[1/3, c] = \exp((c+o(1))(\log p)^{1/3}(\log\log p)^{2/3})\), with the best constant \(c = (64/9)^{1/3}\) matching the factoring NFS. This is the state of the art for arbitrary prime fields and is *subexponential but not polynomial*.
- **Special structure helps**: \(p-1\) smooth → Pohlig–Hellman is polynomial; small-characteristic fields → quasi-polynomial algorithms (Barbulescu–Gaudry–Joux–Thomé 2013), but these exploit \(\mathbb{F}_{p^n}\) with small \(p\), not prime fields.
- **Shor's quantum algorithm** solves DLP in quantum polynomial time, but the census explicitly excludes quantum.
- Index calculus depends on heuristic assumptions about smoothness of numbers derived from random residues; making it polynomial would require drastically better smoothness guarantees than the linear forms considered in practice.

## What would count as solving it?

1. **Upper bound (positive resolution):** A classical randomized (BPP or even Las Vegas) algorithm and proof that, for all primes \(p\), it outputs \(\log_g h\) in \((\log p)^{O(1)}\) expected time. Heuristic analyses alone do not suffice; conditional results (e.g., under GRH or smoothness conjectures) would count as major partial progress.
2. **Lower bound (negative resolution):** A proof that no classical polynomial-time algorithm exists — but note this is far beyond current technique (it would imply P ≠ NP-type separations are not directly needed, yet no such unconditional lower bound for any natural algebraic problem is known; likely out of reach).
3. **Intermediate wins referees would value:** improving the \(L_p[1/3]\) constant or exponent for prime fields; rigorous (unconditional) subexponential bounds; polynomial time for a provably dense family of primes.

## How to start

1. Read Shoup (1997) §1–2 to internalize *why* representation matters and what generic lower bounds leave open; then Odlyzko's survey (1994) on finite-field DLP for the classical index-calculus picture.
2. Study the NFS-DL structure (Joux–Lercier 2006; Gordon 1993): the bottleneck is finding enough smooth elements in a factor base built from small primes / small-norm algebraic numbers. Ask: can lattice reduction (LLL/BKZ) or Coppersmith-type techniques find unusually smooth linear forms deterministically or with better probability?
3. Examine the analogy with factoring: any polynomial-time DLP via index calculus would likely yield related progress on factoring; check whether the two problems could be separated (results by Boneh–Lipton 1996 on "black-box" equivalence under certain algorithm types).
4. Main obstacle: the smoothness probability of "random-looking" integers near size \(p^{\alpha}\) is governed by the Dickman function, and no known technique beats this; a genuinely new algebraic or analytic idea (e.g., exploiting \((\mathbb{Z}/p)^*\)-structure beyond cyclicity) is likely required.

## Key references

- L. Adleman, "A subexponential algorithm for the discrete logarithm problem with applications to cryptography," FOCS 1979. First subexponential (index calculus) algorithm for prime-field DLP; founded the area. (verify exact venue/pages)
- D. Gordon, "Discrete logarithms in GF(p) using the number field sieve," SIAM J. Discrete Math. 6 (1993), 124–138. Adapts the number field sieve to prime-field DLP, achieving \(L_p[1/3]\).
- A. Joux, R. Lercier, "The function field sieve in the medium prime case," ANTS 2006. Best practical and asymptotic \(L[1/3, (64/9)^{1/3}]\) methods for medium/large characteristic. (verify)
- V. Shoup, "Lower bounds for discrete logarithms and related problems," EUROCRYPT 1997. Proves \(\Omega(\sqrt{q})\) queries in the generic group model, showing representation must be exploited.
- A. Odlyzko, "Discrete logarithms in finite fields and their cryptographic significance," EUROCRYPT '94. Survey of index calculus methods and the smoothness heuristics underpinning them.
- P. P. Buhler, H. W. Lenstra, C. Pomeroy, "Factoring integers with the number field sieve" (Lenstra & Lenstra, *The Development of the Number Field Sieve*, LNM 1554, 1993) — background on the factoring analogue. (verify editors/authors as listed)
- S. Pohlig, M. Hellman, "An improved algorithm for computing logarithms over GF(p) and its cryptographic significance," IEEE Trans. IT 24 (1978). Reduces DLP to prime-order subgroups; polynomial when \(p-1\) is smooth.

## Warnings

- Don't conflate small-characteristic results (quasi-polynomial FFS/BGJT) with prime fields — they do not transfer.
- Index calculus analyses are heuristic; don't present a heuristic polynomial bound as a solution.
- Baby-step/giant-step and Pollard rho improvements are exponential in \(\log p\) and structurally capped by Shoup's generic bound.
- "Polynomial in \(\log p\)" is the input-length measure; \(O(\sqrt{p})\) is NOT polynomial in the input.
- A lower-bound resolution almost certainly requires machinery far beyond current complexity theory; budget effort accordingly.

---
*`ALGO-000202` · unverified*

## Acceptance guidance

1. **Upper bound (positive resolution):** A classical randomized (BPP or even Las Vegas) algorithm and proof that, for all primes \(p\), it outputs \(\log_g h\) in \((\log p)^{O(1)}\) expected time. Heuristic analyses alone do not suffice; conditional results (e.g., under GRH or smoothness conjectures) would count as major partial progress.
2. **Lower bound (negative resolution):** A proof that no classical polynomial-time algorithm exists — but note this is far beyond current technique (it would imply P ≠ NP-type separations are not directly needed, yet no such unconditional lower bound for any natural algebraic problem is known; likely out of reach).
3. **Intermediate wins referees would value:** improving the \(L_p[1/3]\) constant or exponent for prime fields; rigorous (unconditional) subexponential bounds; polynomial time for a provably dense family of primes.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/ed8e3dd5-de52-4aa9-84c6-44b68f698122?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000202.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000202)
