<!-- solve-problem:0199d57c-9357-4f80-92b9-a18135ef3a56 -->
# Optimal deterministic scheduling with weak discounting

A [Tangle Solve problem](https://solve.tangle.tools/problems/0199d57c-9357-4f80-92b9-a18135ef3a56).

## What is the problem?

In online packet scheduling with time discounts, a server must process jobs arriving online; at each time step at most one pending job can be transmitted. Each job i has a value vᵢ (often scaled so vᵢ ∈ [1, α] for a parameter α, the "bounded ratio" model), an arrival time, and a deadline. Crucially, the value of a job decays over time: if job i is transmitted t time units after arrival (before its deadline), the scheduler earns vᵢ · λᵗ, where λ ∈ (0, 1] is the discount factor. If the deadline passes, the job expires worthless. The scheduler sees each job only upon arrival and must decide irrevocably, at each step, which job (if any) to send.

The problem: determine the optimal **deterministic competitive ratio** of this online problem as a function of λ, specifically for λ in the open interval roughly (0.770018, 1). The competitive ratio is the worst case over inputs of OPT(ALG)/ALG, where OPT is the offline clairvoyant optimum. The precise interpretation: find matching upper and lower bounds on the best achievable deterministic competitive ratio as a function of λ in this range. Note λ = 1 recovers the classical (undiscounted) packet scheduling problem, which is resolved; at the other end (λ ≤ ~0.770018) upper and lower bounds are known to coincide.

## Why does it matter?

This closes the sharp phase-transition picture of how discounting affects online scheduling hardness: for small λ the problem becomes provably easy, for λ = 1 it is the classical hard problem, and the middle is unexplained. Sharp guarantees matter for perishable-inventory allocation, deadline-driven network scheduling (packets losing value while queued in a router buffer), and any blockchain/mechanism setting where rewards or fees decay. Any protocol or market design charging decaying fees and promising worst-case revenue guarantees currently has no tight guarantee in this parameter range.

## What's already known?

The anchor paper is Gafni and Yaish (2024, revised 2026), which introduced the discounted model and established:
- Matching upper and lower bounds for λ ≤ λ₀ where λ₀ ≈ 0.770018 (verify the exact constant and which bound is tight where — the census gives the endpoint only). At such small λ, heavy discounting makes the greedy/myopic-style policy provably near-optimal; the offline optimum cannot profit from long waits, so the online-offline gap collapses.
- The endpoint λ = 1 corresponds to classical online packet scheduling, where optimal competitive ratios are known in both the general and bounded-values models (e.g., work of Li, Rubinstein, et al. on 2-bounded instances and the O(1)-competitive algorithms for general deadline scheduling — verify which undiscounted results the paper relies on).
- The gap: for λ₀ < λ < 1, the paper gives an upper and lower bound curve that do not touch, leaving the exact competitive ratio open.

The technical difficulty is that intermediate λ mixes two failure modes: the adversary exploits procrastination (the λ = 1 hardness) and impatience (the small-λ structure), and neither analysis technique degenerates gracefully into the other.

## What would count as solving it?

Any of:
1. **Matching bounds:** a deterministic algorithm A and a matching adversarial lower-bound construction giving C*(λ) = OPT/ALG exactly for all λ ∈ (λ₀, 1), where λ₀ ≈ 0.770018.
2. **Partial closure:** tight bounds for a structurally restricted class (e.g., 2-bounded instances, s-bounded instances, or the bounded-value-ratio model) as a function of λ, if the fully general version proves intractable — this should be stated as such.
3. **Improved separation or collapse:** a proof that the known lower bound is tight via a new algorithm, or a stronger lower bound matching the known upper bound, for any subinterval of (λ₀, 1).

A referee would want explicit competitive-ratio functions of λ, with proofs on both the algorithmic and adversarial sides.

## How to start

1. Read arXiv:2402.08549 (2026 revision) end to end, focusing on (a) the lower-bound construction at λ₀ and (b) the algorithm whose analysis becomes tight there — the gap lives exactly where these two analyses separate.
2. Reproduce both the upper and lower bound derivations as functions of λ; identify which step in each is slack for λ ∈ (λ₀, 1). Often the lower bound is a recursive adversary ("generate-and-test" style over instance families) that can be strengthened by extending the family.
3. Study the λ → 1 limit: check whether the bounds in the gap converge to the known undiscounted optimum. If they do not, at least one analysis is improvable at one end.
4. Main obstacle: the adversary's optimal strategy at intermediate λ likely requires non-trivial mixed structure (making the online scheduler indifferent between waiting and sending), which pure LP/indifference arguments handle painfully; consider an LP-based characterization of the optimal adversary, as done for classical 2-bounded scheduling.

## Key references

- Gafni, A., and Yaish, O. *Online Packet Scheduling With Time Discounts.* arXiv:2402.08549 (2024; revised 2026). Introduces the discounted-value online packet scheduling model, gives matching deterministic competitive bounds for small λ (up to ≈0.770018), resolves/relates the undiscounted λ = 1 endpoint, and leaves the intermediate regime open. https://arxiv.org/abs/2402.08549
- (Verify) Classical undiscounted results the paper builds on — likely including Li and Rubinstein's work on 2-bounded online packet scheduling and O(1)-competitive algorithms for general instances; consult the paper's related-work section for the exact set, since the census record does not enumerate them.

## Warnings

- The constant 0.770018 and the exact matching structure below it come from a census summary of the 2026 revision; verify directly against the paper before relying on numerics.
- The exact model details matter enormously: whether values are general or bounded-ratio ([1, α]), whether deadlines are s-bounded, and whether the discount is λᵗ or λ^(t−arrival). State your model explicitly and match the paper's.
- Do not assume the undiscounted λ = 1 competitive ratio transfers continuously into the interval; discounting changes the adversary's power qualitatively.
- Randomized algorithms are a different problem; the census asks for deterministic only.
- The "blockchain-game-theory" domain tag is contextual (fee/reward decay in protocols); the mathematics is pure online algorithms.

---
*`BCGT-000197` · unverified*

## Acceptance guidance

Any of:
1. **Matching bounds:** a deterministic algorithm A and a matching adversarial lower-bound construction giving C*(λ) = OPT/ALG exactly for all λ ∈ (λ₀, 1), where λ₀ ≈ 0.770018.
2. **Partial closure:** tight bounds for a structurally restricted class (e.g., 2-bounded instances, s-bounded instances, or the bounded-value-ratio model) as a function of λ, if the fully general version proves intractable — this should be stated as such.
3. **Improved separation or collapse:** a proof that the known lower bound is tight via a new algorithm, or a stronger lower bound matching the known upper bound, for any subinterval of (λ₀, 1).

A referee would want explicit competitive-ratio functions of λ, with proofs on both the algorithmic and adversarial sides.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/0199d57c-9357-4f80-92b9-a18135ef3a56?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000197.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000197)
