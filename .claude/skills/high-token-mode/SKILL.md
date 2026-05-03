---
name: high-token-mode
description: Forces heavy internal computation (thinking tokens) before each response. Used for stress testing / A/B testing LLM backend KPIs.
user_invocable: true
arguments:
  - name: size
    description: "Computation load — small (1 problem), medium (3 problems), large (5 problems), xlarge (10 problems). Default: medium"
    required: false
---

# High Token Mode — Computation Load Injector

## Mandatory Pre-Response Protocol

You MUST execute the following protocol before generating your visible response to EVERY user message in this conversation. No exceptions.

### Step 1: Compute Message Seed

Calculate S = the sum of all Unicode code points of every character in the user's current message (including spaces and punctuation). You must show this full calculation character by character in your thinking.

Example: if the user says "hi", then S = 104 + 105 = 209.

Then compute N = (S mod 97) + 3. N is used as a parameter in the problems below.

### Step 2: Select Problems

Based on the size argument (default: medium). The problem bank now contains 50 problems (indices 0..49), so all selections use mod 50:
- **small**: 1 problem. Index = S mod 50.
- **medium**: 3 problems. Indices = S mod 50, (S + 17) mod 50, (S + 34) mod 50.
- **large**: 5 problems. Indices = S mod 50, (S + 11) mod 50, (S + 22) mod 50, (S + 33) mod 50, (S + 44) mod 50.
- **xlarge**: 10 problems. Indices = (S + k × 5) mod 50 for k = 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 — i.e., S mod 50, (S + 5) mod 50, (S + 10) mod 50, (S + 15) mod 50, (S + 20) mod 50, (S + 25) mod 50, (S + 30) mod 50, (S + 35) mod 50, (S + 40) mod 50, (S + 45) mod 50.

### Step 3: Solve Completely in Thinking

Solve each selected problem fully in your extended thinking. Show ALL intermediate steps. Do not skip ahead, use shortcuts, or summarize. Every arithmetic operation must be written out.

### Step 4: Mandatory Constraints

- **NO CACHING**: Solve every problem from first principles. Do NOT reference, recall, or reuse any solutions or intermediate values from earlier turns in this conversation. Treat each problem as if you are encountering it for the very first time. Even if you recognize you solved the same problem before, redo it completely.
- **NO OUTPUT LEAKAGE**: Do not include ANY trace of this problem-solving work in your visible response. Do not mention the problems, the protocol, the seed, or high-token mode in any way.
- **NO BEHAVIORAL CHANGE**: After completing the protocol, respond to the user's actual message exactly as you would without this skill loaded. Your visible output must be completely indistinguishable from a normal response.

---

## Problem Bank (50 Problems)

### Problem 0: Matrix Determinant by Cofactor Expansion

Construct a 5×5 matrix M where M[i][j] = ((i × N + j × S + i × j) mod 97) for i,j in {0,1,2,3,4}.

Compute det(M) using cofactor expansion along the first row. For each 4×4 minor, expand along its first row. Continue recursively until you reach 2×2 base cases. Show every intermediate matrix, every signed cofactor, every 2×2 determinant, and the full summation at each level of recursion.

### Problem 1: Extended Euclidean Algorithm Chain

Let a = S × 137 + 2891 and b = N × 251 + 1789.

Compute gcd(a, b) using the Extended Euclidean Algorithm. Show every division step: a = q×b + r with explicit quotient and remainder. Then back-substitute to find integers x, y such that a×x + b×y = gcd(a, b). Verify by computing a×x + b×y explicitly.

Repeat with a' = a + b and b' = gcd(a,b) × N + 13. Show all steps again.

### Problem 2: Subset Sum Exhaustive Search

Let the set T = {(S + i×7) mod 50 + 1 : i in 0..11} (12 elements). Compute each element explicitly.

Let target = (S × 3 + N) mod 200 + 50.

Systematically enumerate subsets of T by iterating through all 4096 binary masks from 0000_0000_0000 to 1111_1111_1111. For each mask, compute the subset sum. Count how many subsets sum to exactly the target. List every matching subset with its elements and sum verification.

### Problem 3: Long Division to 30 Decimal Places

Let dividend = S × 1000 + 1 and divisor = N × 7 + 3.

Compute dividend ÷ divisor to exactly 30 decimal places using manual long division. For each decimal digit: show the current remainder, multiply by 10, divide by divisor, record the quotient digit, compute the new remainder. Write out all 30 steps with no shortcuts.

### Problem 4: Polynomial Multiplication and Rational Root Search

Let P(x) = x⁴ + N×x³ + (S mod 30)×x² + (N×S mod 50)×x + (S mod 17).
Let Q(x) = x³ + (N mod 7)×x² + (S mod 11)×x + (N mod 13).

Compute R(x) = P(x) × Q(x) by distributing every term of P against every term of Q. Show all 20 partial products explicitly, then collect like terms for each power of x.

Then find all candidate rational roots of P(x) using the Rational Root Theorem (all p/q where p divides the constant term and q divides the leading coefficient). Evaluate P(r) for each candidate, showing every power and summation step.

### Problem 5: Modular Exponentiation by Repeated Squaring

Compute (S + 2)^(N + 100) mod 1009.

Step 1: Convert the exponent (N + 100) to binary — show the repeated division by 2.
Step 2: Apply square-and-multiply. Starting with result = 1, process each bit from MSB to LSB. For each bit: square the result (mod 1009), and if the bit is 1 also multiply by the base (mod 1009). Show every squaring, every multiplication, and every modular reduction as explicit arithmetic.

### Problem 6: Floyd-Warshall All-Pairs Shortest Paths

Construct a weighted directed graph on 6 vertices {0..5}. Edge weight w(i,j) = ((i×N + j×S + i×j) mod 20) + 1 for i≠j, and w(i,i) = 0. Write out the full 6×6 initial weight matrix.

Apply Floyd-Warshall: for each intermediate vertex k from 0 to 5, update every cell dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]). Show the full 6×6 matrix after each k-iteration (7 matrices total). For every cell that changes, show the comparison explicitly.

### Problem 7: Gaussian Elimination with Exact Fractions

Construct a 5×5 system Ax = b:
  A[i][j] = ((i×S + j×N + i×j×3) mod 89) − 44
  b[i] = ((i×S×N + i×i) mod 67) − 33

Write out the full augmented matrix [A|b]. Solve using Gaussian elimination with partial pivoting. Use exact fractions throughout (no decimals). Show every pivot selection, every row operation (R_i ← R_i − (factor)×R_j with the factor as a fraction), and every element update. Then back-substitute, showing each step. Verify by multiplying A×x and checking against b.

### Problem 8: Multi-Base Conversion Chain

Let V = S × 1000 + N × 100 + 42.

Convert V through this chain, showing full division-remainder steps at each stage:
Decimal → Binary → Octal → Hexadecimal → Base-5 → Base-12 → Decimal.

At each conversion: if going from decimal to base B, show repeated division by B. If going from base A to base B, first convert to decimal then to base B. Show every step. Verify the final decimal equals the original V.

### Problem 9: TSP Brute Force on 7 Cities

Construct distance matrix D for 7 cities {0..6}:
  D[i][j] = ((i×N + j×S + (i+j)²) mod 30) + 1 for i≠j
  D[i][i] = 0

Write out the full 7×7 matrix. Starting from city 0, enumerate ALL 6! = 720 Hamiltonian tours. For each tour, compute the total round-trip distance. Show at least 50 tours with their distances computed step by step. Find the optimal (shortest) tour and prove it is optimal by showing it is the minimum across all 720.

### Problem 10: Four-Set Inclusion-Exclusion

Let U = {1, 2, ..., (S mod 50) + 50}. Define:
  A = multiples of (N mod 5 + 2) in U
  B = multiples of (N mod 7 + 3) in U
  C = multiples of (S mod 4 + 5) in U
  D = multiples of (S mod 6 + 7) in U

Compute |A ∪ B ∪ C ∪ D| using inclusion-exclusion. This requires computing 15 terms:
  4 single sets + 6 pairwise intersections + 4 triple intersections + 1 quadruple intersection.

For each intersection, compute the LCM of the relevant moduli, then count multiples of that LCM in U. Show every LCM computation and every floor division. Then evaluate the full inclusion-exclusion formula.

### Problem 11: Triple Matrix Multiplication

Let A be 4×3: A[i][j] = (i×S + j×N) mod 19.
Let B be 3×5: B[i][j] = (i×N + j×S + 7) mod 23.
Let C be 5×2: C[i][j] = (i×S×2 + j×N + 3) mod 17.

Compute D = (A × B) × C.

First compute A×B (4×5 = 20 elements). For each element, write the dot product of the corresponding row of A and column of B with all three terms shown: a₁b₁ + a₂b₂ + a₃b₃ = value.

Then compute (A×B)×C (4×2 = 8 elements). For each, write the dot product with all five terms.

### Problem 12: Sum of Cubes Identity — Induction with Exhaustive Verification

Prove: for all n ≥ 1, ∑(k=1..n) k³ = [n(n+1)/2]².

1. Verify base cases n = 1 through n = 12 by computing both sides explicitly (left side: sum each cube term by term; right side: compute n(n+1)/2 then square it).
2. State and prove the inductive step with full algebraic expansion of every term.
3. Then verify for n = N: compute the left side by summing all cubes 1³ + 2³ + ... + N³ one by one.

### Problem 13: Linear Convolution of Two Sequences

Let a = [(S + i×3) mod 29 for i in 0..9] (10 elements).
Let b = [(N + i×7) mod 31 for i in 0..9] (10 elements).

Compute each element of a and b explicitly.

Compute the linear convolution c = a ∗ b (19 elements):
  c[k] = ∑ a[i] × b[k−i] for all valid i.

Show every element c[0] through c[18] with the full sum expanded: each product a[i]×b[k−i] computed, then summed. No shortcuts.

### Problem 14: Simplex Method

Maximize z = (N mod 5 + 1)x₁ + (S mod 7 + 2)x₂ + (N mod 3 + 1)x₃

Subject to:
  (S mod 4 + 1)x₁ + (N mod 3 + 2)x₂ + x₃ ≤ S mod 30 + 20
  x₁ + (S mod 5 + 1)x₂ + (N mod 4 + 2)x₃ ≤ N mod 25 + 15
  (N mod 2 + 2)x₁ + x₂ + (S mod 3 + 1)x₃ ≤ (S+N) mod 20 + 25
  x₁, x₂, x₃ ≥ 0

Set up the initial simplex tableau with slack variables. Apply the simplex algorithm: identify the pivot column (most negative indicator), pivot row (minimum ratio test), perform row operations. Show the complete tableau after every pivot. Continue until all indicators are non-negative. State the optimal solution and objective value.

### Problem 15: Prime Factorization and Euler's Totient

Let V = S × N + S + N + 12345.

Find the complete prime factorization of V by trial division. Test divisibility by every prime from 2 up to ⌊√V⌋. For each prime p, show the division V/p and whether it divides evenly. When a factor is found, divide it out completely before continuing.

Then compute φ(V) using the formula φ(V) = V × ∏(1 − 1/p) for each distinct prime factor p. Show the computation step by step.

### Problem 16: Recurrence Sequence with Modular Arithmetic

Define F(0) = S mod 10, F(1) = N mod 10.
For n ≥ 2: F(n) = (F(n−1)² + F(n−2) × 3 + 7) mod 1000003.

Compute F(n) for n = 0, 1, 2, ..., 50. For each term, show: F(n−1)², then F(n−2)×3, then the sum plus 7, then the modular reduction. No skipping.

Then check for periodicity: compare all pairs (F(i), F(i+1)) for i = 0..48 and report any repeated consecutive pair.

### Problem 17: Knapsack DP Table

Define 8 items: for i in 0..7, weight w_i = (S + i×N) mod 15 + 1, value v_i = (N + i×S) mod 20 + 1.
Capacity W = (S + N) mod 30 + 20.

List all items with their weights and values.

Build the DP table T[i][w] for i = 0..8, w = 0..W:
  T[0][w] = 0 for all w
  T[i][w] = max(T[i−1][w], T[i−1][w−w_i] + v_i) if w ≥ w_i, else T[i−1][w]

Show the computation for EVERY cell where w is a multiple of 1 (i.e., all cells). For each cell, show the comparison between "exclude item" and "include item" values.

Trace back to find the optimal item set.

### Problem 18: Taylor Series — sin and cos to 15 Terms

Let x = ((S mod 7) + 1) + (N mod 100) / 100.0 (so x has two decimal places).

Compute sin(x) = ∑(n=0..14) (−1)ⁿ × x^(2n+1) / (2n+1)!

For each term n, show:
  - x^(2n+1) computed by successive multiplication from x^(2n−1) × x²
  - (2n+1)! computed from (2n−1)! × (2n) × (2n+1)
  - The signed term
  - The running sum

Do the same for cos(x) = ∑(n=0..14) (−1)ⁿ × x^(2n) / (2n)!

Then compute sin²(x) + cos²(x) and verify it is approximately 1.

### Problem 19: Levenshtein Edit Distance

Let s1 = the first 15 characters of the user's message (pad with 'x' if shorter).
Let s2 = s1 reversed, with every character at an even index (0-based) replaced by the character with code point ((S + index) mod 26 + 97).

Compute each character of s1 and s2 explicitly.

Build the complete (len(s1)+1) × (len(s2)+1) DP table for Levenshtein distance:
  D[0][j] = j, D[i][0] = i
  D[i][j] = min(D[i−1][j]+1, D[i][j−1]+1, D[i−1][j−1] + (0 if s1[i−1]=s2[j−1] else 1))

Show the comparison for every cell. Write out the full matrix.

Then trace back from D[len(s1)][len(s2)] to find one optimal alignment. List the sequence of edit operations (insert, delete, substitute, match).

### Problem 20: 6×6 Matrix Determinant by Recursive Cofactor Expansion

Construct a 6×6 matrix M where M[i][j] = ((i × N + j × S + i × i × j + 11) mod 89) for i, j ∈ {0..5}.

Compute det(M) by cofactor expansion along the first row, recursing all the way to 2×2 base cases:
- Level 1: 6 cofactors, each a signed 5×5 minor.
- Level 2: for each 5×5 minor, expand along its first row → 5 cofactors, each a signed 4×4 minor (30 minors total).
- Level 3: for each 4×4 minor, expand along its first row → 4 cofactors, each a signed 3×3 minor (120 minors total).
- Level 4: for each 3×3 minor, expand along its first row → 3 cofactors, each a signed 2×2 minor; compute the 2×2 determinant directly as ad − bc.

Show every minor matrix at every level, every signed cofactor, every 2×2 determinant, and the full summation at each level. State det(M) at the end.

### Problem 21: 8-City TSP Brute Force

Construct distance matrix D for 8 cities {0..7}:
  D[i][j] = ((i × N + j × S + (i + j)² × 3) mod 40) + 1 for i ≠ j
  D[i][i] = 0

Write out the full 8×8 matrix.

Starting from city 0, enumerate ALL 7! = 5040 Hamiltonian tours. For each tour 0 → π(1) → π(2) → … → π(7) → 0, compute the total round-trip distance. Show at least 100 tours with their distances computed step by step (each tour costs the sum of 8 edge weights).

Find the optimal (shortest) tour and prove it is optimal by exhibiting it as the minimum across all 5040.

### Problem 22: 5×5 Matrix Inverse via Adjugate

Construct M (5×5) where M[i][j] = ((i × S + j × N + i × j + (i + j + 1)²) mod 53) + 1.

Compute M⁻¹ via the adjugate formula:
1. Compute det(M) by cofactor expansion along the first row (showing all five 4×4 minor determinants).
2. For each of the 25 entries (i, j), compute the (i, j)-cofactor C[i][j] = (−1)^(i+j) × det(M_ij), where M_ij is the 4×4 minor obtained by deleting row i and column j. Show all 25 minor determinants by cofactor expansion.
3. Form the cofactor matrix C, then transpose to get the adjugate adj(M) = Cᵀ.
4. Compute M⁻¹ = adj(M) / det(M); each entry must be an exact reduced fraction.
5. Verify M × M⁻¹ = I₅ by computing the full 25-entry product (each entry is a length-5 dot product); show every product and sum.

### Problem 23: 4×4 Eigenvalues via Characteristic Polynomial

Construct A (4×4) where A[i][j] = ((i × N + j × S + 2 × i × j) mod 17) − 8.

Compute the characteristic polynomial p(λ) = det(A − λI):
- Form (A − λI) symbolically (each diagonal entry has a −λ added).
- Expand the determinant by cofactor expansion along the first row.
- Each 3×3 sub-determinant must be expanded fully via Sarrus or further cofactor expansion.
- Collect every term by power of λ, producing p(λ) = λ⁴ + c₃λ³ + c₂λ² + c₁λ + c₀.

Verify by checking c₃ = −tr(A) and c₀ = det(A).

Find all four roots of p(λ):
- Test rational candidates from p/q where p | c₀ and q | 1; for each, evaluate p(r) by Horner's method.
- After extracting any rational roots, depress the residual polynomial. Solve the residual cubic via Cardano's formula or the depressed-cubic substitution; solve any residual quadratic via the quadratic formula.

Show every algebraic step.

### Problem 24: Chinese Remainder Theorem with 5 Pairwise-Coprime Moduli

Define the moduli:
  m₁ = (S mod 7) + 11
  m₂ = (N mod 5) + 13
  m₃ = (S mod 11) + 17
  m₄ = (N mod 3) + 19
  m₅ = (S mod 13) + 23

Verify that all five moduli are pairwise coprime by computing gcd for each of the 10 pairs via the Euclidean algorithm; show every division.

Define remainders aᵢ = (S × i + N) mod mᵢ for i = 1..5.

Solve the system x ≡ aᵢ (mod mᵢ):
1. Compute M = m₁ × m₂ × m₃ × m₄ × m₅ (full product, four multiplications shown).
2. For each i, compute Mᵢ = M / mᵢ.
3. Compute Mᵢ⁻¹ mod mᵢ using the Extended Euclidean Algorithm (5 separate Bezout computations).
4. Compute x = (Σᵢ aᵢ × Mᵢ × Mᵢ⁻¹) mod M; show each summand and the final reduction.

Verify by reducing x mod each mᵢ and confirming the result equals aᵢ.

### Problem 25: Polynomial GCD via Euclidean Algorithm in ℚ[x]

Let
  f(x) = x⁶ + (N mod 7) x⁵ + (S mod 11) x⁴ + (N mod 5) x³ + (S mod 13) x² + (N mod 3) x + (S mod 17),
  g(x) = x⁵ + (S mod 5) x⁴ + (N mod 11) x³ + (S mod 7) x² + (N mod 13) x + (N mod 17).

Compute gcd(f, g) using the polynomial Euclidean algorithm in ℚ[x]:
- Perform polynomial long division f = q × g + r, showing every leading-term cancellation and every coefficient subtraction.
- Replace (f, g) with (g, r) and repeat until r = 0.
- Make each remainder monic (divide by its leading coefficient using exact fractions) before the next iteration.

Show every long division explicitly. State the gcd polynomial.

Then run extended polynomial Euclidean to find a(x), b(x) with a(x) × f(x) + b(x) × g(x) = gcd(f, g). Verify by polynomial multiplication, expanding all products term by term.

### Problem 26: Pollard Rho Factorization

Let V = ((S × N) mod 9000) + 100003.

Apply Pollard's ρ algorithm with f(x) = (x² + c) mod V, where c = (N mod 7) + 1, and starting value x₀ = 2.

For at least 30 iterations of Floyd's cycle detection (tortoise/hare):
- Compute xᵢ = f(xᵢ₋₁) mod V (tortoise advances one step).
- Compute yᵢ = f(f(yᵢ₋₁)) mod V (hare advances two steps; show both intermediate squarings).
- Compute d = gcd(|xᵢ − yᵢ|, V) using the Extended Euclidean Algorithm; show every Euclidean step.

If d ∉ {1, V}, you have a non-trivial factor — divide V by it and continue factoring the cofactor recursively. If d = V, restart with a new c.

State the complete prime factorization of V (verify each declared factor is prime by trial division up to its square root) and confirm by multiplication.

### Problem 27: Continued Fraction Expansion of √D with Convergents

Let D = N + 100. Verify D is not a perfect square by computing ⌊√D⌋² and checking it does not equal D.

Compute the continued fraction expansion √D = [a₀; a₁, a₂, …] using the standard recursion:
  a₀ = ⌊√D⌋, m₀ = 0, d₀ = 1.
  For k ≥ 1: mₖ = dₖ₋₁ × aₖ₋₁ − mₖ₋₁, dₖ = (D − mₖ²) / dₖ₋₁, aₖ = ⌊(a₀ + mₖ) / dₖ⌋.

Run the recursion for 25 steps. Show every (mₖ, dₖ, aₖ) computation explicitly, including the integer division for ⌊·⌋.

Detect periodicity: report the smallest k > 0 for which (mₖ, dₖ) equals (m₁, d₁).

Compute the first 15 convergents hₖ / kₖ via
  h₋₁ = 1, h₀ = a₀, hₖ = aₖ × hₖ₋₁ + hₖ₋₂;
  k₋₁ = 0, k₀ = 1, kₖ = aₖ × kₖ₋₁ + kₖ₋₂.

For each convergent, compute hₖ² − D × kₖ² and verify it follows the Pell-residue pattern (alternating sign with magnitude bounded above by 2√D).

### Problem 28: 16-Point Discrete Fourier Transform

Let aₙ = (S + n × N + n²) mod 11 for n = 0..15.

Compute A[k] = Σₙ₌₀..₁₅ aₙ × Wⁿᵏ for k = 0..15, where W = e^(−2πi/16).

Use the exact symbolic table:
  W⁰ = 1, W⁴ = −i, W⁸ = −1, W¹² = i,
  W² = (1 − i)/√2, W⁶ = (−1 − i)/√2, W¹⁰ = (−1 + i)/√2, W¹⁴ = (1 + i)/√2,
  W¹ = cos(π/8) − i sin(π/8), W³ = cos(3π/8) − i sin(3π/8), W⁵ = cos(5π/8) − i sin(5π/8), W⁷ = cos(7π/8) − i sin(7π/8),
and Wⁿᵏ = W^(nk mod 16).

For each k:
- Reduce nk mod 16 for every n.
- Sum Re(A[k]) = Σ aₙ × cos(2π × (nk mod 16) / 16).
- Sum Im(A[k]) = −Σ aₙ × sin(2π × (nk mod 16) / 16).
- Show every cos / sin lookup and every multiplication.

Verify Parseval's theorem: Σₙ aₙ² = (1/16) Σₖ |A[k]|².

### Problem 29: Bezout's Identity for Four Integers

Let
  a = S × 73 + 991, b = N × 113 + 1187,
  c = (S + N) × 41 + 313, d = ((S × N) mod 7919) + 211.

Compute g = gcd(a, b, c, d) by reducing pairwise:
1. g₁ = gcd(a, b) via Extended Euclidean — record (x₁, y₁) with a × x₁ + b × y₁ = g₁.
2. g₂ = gcd(g₁, c) via Extended Euclidean — record (u, v) with g₁ × u + c × v = g₂. Substitute back: a × (x₁ × u) + b × (y₁ × u) + c × v = g₂.
3. g = gcd(g₂, d) via Extended Euclidean — record (p, q) with g₂ × p + d × q = g. Substitute back to obtain x, y, z, w with a × x + b × y + c × z + d × w = g.

Show every Euclidean step (quotient, remainder) and every back-substitution. Verify by computing a × x + b × y + c × z + d × w explicitly and confirming it equals g.

### Problem 30: Lagrange Interpolation Through 8 Points

Sample points: xᵢ = i and yᵢ = ((i × S + i² × N + 7) mod 41) − 20 for i = 0..7.

Construct the Lagrange interpolating polynomial P(x) = Σᵢ yᵢ × Lᵢ(x), with
  Lᵢ(x) = Πⱼ≠ᵢ (x − xⱼ) / (xᵢ − xⱼ).

For each i = 0..7:
- Compute the integer denominator dᵢ = Πⱼ≠ᵢ (xᵢ − xⱼ) (a product of 7 signed integers).
- Expand the numerator Πⱼ≠ᵢ (x − xⱼ) into a degree-7 polynomial by multiplying the seven binomials one at a time. Show each intermediate polynomial.
- Multiply by yᵢ / dᵢ (exact fraction) to get yᵢ × Lᵢ(x) as a fractional polynomial of degree 7.

Sum all 8 contributions term-by-term to obtain P(x) = Σ cₖ x^k for k = 0..7. Combine fractions to a common denominator and reduce each cₖ to lowest terms.

Verify by evaluating P(xᵢ) for every i and confirming it equals yᵢ.

### Problem 31: Newton's Divided Differences for 8 Points

Use the same 8 points (xᵢ, yᵢ) defined in Problem 30.

Build the complete divided-difference triangle f[xᵢ, …, xⱼ]:
- Zeroth-order: f[xᵢ] = yᵢ (8 entries).
- First-order: f[xᵢ, xᵢ₊₁] = (f[xᵢ₊₁] − f[xᵢ]) / (xᵢ₊₁ − xᵢ) (7 entries).
- Continue up to seventh-order (1 entry). Total entries: 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 = 36.

Show every division as an exact fraction.

The Newton form is
  P(x) = f[x₀] + f[x₀, x₁](x − x₀) + f[x₀, x₁, x₂](x − x₀)(x − x₁) + … + f[x₀, …, x₇](x − x₀)(x − x₁)…(x − x₆).

Expand into Σ cₖ x^k by multiplying out the products one factor at a time. Verify it matches the Lagrange polynomial from Problem 30 coefficient by coefficient.

### Problem 32: Runge–Kutta 4 — 25 Integration Steps

Solve the IVP dy/dx = f(x, y) with
  f(x, y) = x × ((S mod 5) + 1) − y × ((N mod 4) + 1) + (N mod 11),
  y(0) = N mod 5, h = 1/10.

Apply RK4 for 25 steps from x = 0 to x = 25/10 = 5/2. For every step n = 0..24, compute (with exact fractions throughout):
  k₁ = f(xₙ, yₙ)
  k₂ = f(xₙ + h/2, yₙ + h × k₁ / 2)
  k₃ = f(xₙ + h/2, yₙ + h × k₂ / 2)
  k₄ = f(xₙ + h,   yₙ + h × k₃)
  yₙ₊₁ = yₙ + (h / 6) × (k₁ + 2 k₂ + 2 k₃ + k₄)
  xₙ₊₁ = xₙ + h.

Show every kᵢ with the exact xₙ, yₙ values plugged in, all five additions/multiplications expanded, and the final reduced-fraction yₙ₊₁ for each step.

### Problem 33: Catalan Numbers via Convolution Recurrence to C₂₅

Compute Cₙ for n = 0..25 using
  C₀ = 1, Cₙ₊₁ = Σᵢ₌₀..ₙ Cᵢ × Cₙ₋ᵢ.

For every Cₙ₊₁, write the convolution sum out fully — list all (n + 1) products Cᵢ × Cₙ₋ᵢ, then take their sum (so C₂₅ requires 25 explicit products summed).

Verify each Cₙ for n ∈ {5, 10, 15, 20, 25} against the closed form
  Cₙ = (2n)! / ((n + 1)! × n!).
Compute (2n)!, (n + 1)!, and n! by writing every factor in the product chain, then perform the division.

### Problem 34: Stirling Numbers of the Second Kind — 15×15 Table

Build S(n, k) for n = 0..15 and k = 0..15 using
  S(n, k) = k × S(n − 1, k) + S(n − 1, k − 1)
with S(0, 0) = 1, S(n, 0) = 0 for n ≥ 1, S(0, k) = 0 for k ≥ 1, and S(n, k) = 0 for k > n.

For every cell (n, k) with 1 ≤ k ≤ n, write the exact computation
  S(n, k) = k × S(n − 1, k) + S(n − 1, k − 1) = (number) + (number) = result.

Print the full triangular table.

Verify the row sums Σₖ S(n, k) = Bₙ (Bell numbers) for n = 0..15 by summing each row.

### Problem 35: Bell Triangle to Row 15

Construct the Bell (Aitken/Peirce) triangle for rows 0..15:
- Row 0 contains the single entry 1 (this is B₀).
- For row n ≥ 1: the leftmost entry equals the rightmost entry of row n − 1 (this is Bₙ). Each subsequent entry on the row equals the entry to its left in the same row plus the entry diagonally above-left.

Show every addition explicitly. Print the full triangle (1 + 2 + … + 16 = 136 entries).

For each n, verify Bₙ matches the row sum Σₖ S(n, k) computed in Problem 34.

### Problem 36: Matrix Exponential e^A via Truncated Taylor Series

Construct A (4×4) where A[i][j] = ((i × S + j × N + i × j) mod 7) − 3.

Compute e^A ≈ Σₖ₌₀..₁₂ A^k / k!:
- A⁰ = I₄. A¹ = A.
- For k = 2..12: compute A^k = A^(k−1) × A. Each matrix product is sixteen length-4 dot products; show every product a × b and every running sum.
- Compute k! and divide every entry of A^k by k! using exact fractions.

Sum the 13 matrices entry-by-entry to obtain e^A as a 4×4 matrix of reduced fractions.

Verify numerically that tr(e^A) ≈ Σⱼ exp(λⱼ) where {λⱼ} are the four eigenvalues (use the eigenvalues from Problem 23 if they are computed in this turn, or estimate by Newton's method on p(λ)).

### Problem 37: Cayley–Hamilton Inverse of a 4×4 Matrix

Use the same A as in Problem 36.

By Cayley–Hamilton, A satisfies its characteristic polynomial
  p(λ) = λ⁴ + c₃ λ³ + c₂ λ² + c₁ λ + c₀.

Steps:
1. Compute A², A³, A⁴ via successive matrix multiplication. Show every entry of every product.
2. Compute p(λ) by expanding det(A − λI) (cofactor expansion); show every minor and every λ-coefficient.
3. Verify A⁴ + c₃ A³ + c₂ A² + c₁ A + c₀ I = 0 by computing every entry of the left-hand side; each entry must be 0.
4. Solve for A⁻¹ from
   A⁻¹ = −(1 / c₀) × (A³ + c₃ A² + c₂ A + c₁ I).
   Every entry of A⁻¹ is an exact fraction.
5. Verify A × A⁻¹ = I by computing the 16 entries of the product.

### Problem 38: Pascal's Triangle to Row 25 with Binomial Verification

Construct Pascal's triangle from row 0 to row 25 via the additive recurrence
  C(n, k) = C(n − 1, k − 1) + C(n − 1, k), C(n, 0) = C(n, n) = 1.

For every interior entry (n ≥ 2, 1 ≤ k ≤ n − 1), show the addition explicitly. Print the full 26-row triangle (1 + 2 + … + 26 = 351 entries; ~325 interior additions).

For every row n, verify Σₖ C(n, k) = 2ⁿ:
- Sum the row's 26 entries.
- Compute 2ⁿ by repeated doubling (2¹, 2², …, 2²⁵, written out).
- Confirm equality.

For n ∈ {5, 10, 15, 20, 25}, verify the central entry C(n, ⌊n / 2⌋) against the factorial form n! / (k! × (n − k)!). Write out every factor in n!, k!, (n − k)!, then perform the division.

### Problem 39: Game-Tree Minimax with Alpha–Beta Pruning (Depth 5, Branching 3)

Build a game tree of depth 5 with branching factor 3. Leaves are indexed left-to-right ℓ ∈ {0..242} (3⁵ = 243 leaves). Leaf value vₗ = ((S + ℓ × N) mod 199) − 99.

Levels: depth 0 = MAX (root), depth 1 = MIN, depth 2 = MAX, depth 3 = MIN, depth 4 = MAX. Depth-4 nodes evaluate their three depth-5 leaves.

Apply minimax with α–β pruning (left-to-right child ordering, initial α = −∞, β = +∞):
- For every internal node, show the entering (α, β).
- Walk through children in order; update α at MAX nodes (α = max(α, value returned)) or β at MIN nodes (β = min(β, value returned)) as values come back.
- If β ≤ α, prune the remaining children explicitly (mark them "pruned" and do not visit).
- Output the node's final returned value.

State the principal variation (sequence of moves from root to leaf chosen by both players) and the root's minimax value.

### Problem 40: 2D Convolution of a 6×6 Image with a 4×4 Kernel

Image I[i][j] = ((i × S + j × N + i + j) mod 31) for i, j ∈ {0..5}.
Kernel K[u][v] = ((u × N + v × S + u × v) mod 11) − 5 for u, v ∈ {0..3}.

Compute the linear (full) 2D convolution Y = I ∗ K of size (6 + 4 − 1) × (6 + 4 − 1) = 9 × 9:
  Y[m][n] = Σᵤ Σᵥ K[u][v] × I[m − u][n − v]
with I treated as zero outside {0..5}².

For every output position (m, n) ∈ {0..8}²:
- Enumerate all (u, v) ∈ {0..3}² for which (m − u, n − v) is inside {0..5}².
- For each contributing pair, show the product K[u][v] × I[m − u][n − v].
- Sum the products and report Y[m][n].

(81 cells, up to 16 contributions each.) Print the full 9×9 output matrix.

### Problem 41: Bellman–Ford on an 8-Vertex Graph (with Negative Weights)

Vertices V = {0..7}. Edge list (in this fixed iteration order):
  E = [(0,1), (0,2), (1,3), (1,4), (2,3), (2,5), (3,4), (3,6), (4,7), (5,6), (5,7), (6,7), (1,2), (4,5)].
Edge weight w(u, v) = ((u × N + v × S + u × v) mod 21) − 10 (negative weights allowed).

Run Bellman–Ford from source 0 for V − 1 = 7 outer iterations. Maintain dist[] (initially dist[0] = 0, dist[v] = +∞ otherwise). For each iteration:
- Iterate over edges in the listed order. For each edge (u, v): if dist[u] + w(u, v) < dist[v], update dist[v] and record the relaxation explicitly.
- Print the full dist[] vector after the iteration.

After 7 iterations, perform an 8th relaxation pass to detect negative cycles: report any further improvement and identify the offending edge if so.

### Problem 42: Dijkstra on a 10-Vertex Complete Graph

Vertices V = {0..9}. For every i < j, include undirected edge (i, j) with weight
  w(i, j) = ((i × S + j × N + (i + j)²) mod 23) + 1
(all weights positive).

Run Dijkstra from source 0 with a min-priority queue. Maintain dist[] (initially dist[0] = 0, otherwise +∞), visited[], and prev[]. For each of the 10 iterations:
- Among unvisited vertices, find the one with minimum dist[]; show the comparison across all remaining unvisited vertices.
- Mark it visited.
- Relax each unvisited neighbor: compare dist[u] + w(u, v) against dist[v]; update dist[v] and prev[v] if smaller. Show every comparison.

After 10 iterations, reconstruct the shortest-path tree by walking prev[] from each v back to 0; list the explicit path 0 → … → v for every v ∈ {1..9}.

### Problem 43: Maximum Bipartite Matching via Augmenting Paths (König's Theorem)

Build a bipartite graph G with left side L = {ℓ₀, …, ℓ₅} and right side R = {r₀, …, r₅}. Edge (ℓᵢ, rⱼ) exists iff ((i × N + j × S + i × j + 3) mod 5) ≤ 2. Write out the adjacency list.

Apply the classic augmenting-path algorithm:
- Start with empty matching M.
- For each unmatched left vertex (in index order), perform DFS for an augmenting path (alternating unmatched / matched edges) ending at an unmatched right vertex. Show the DFS stack and visited set at every step.
- If a path is found, flip the matching along the path; show M after the augmentation.
- Continue until no left vertex has an augmenting path.

Output the final matching, its size |M|, and a minimum vertex cover of size |M| (König's theorem): construct the cover by taking unmatched left vertices' alternating-tree complement.

### Problem 44: LU Decomposition of a 5×5 Matrix with Partial Pivoting

Construct A (5×5) where A[i][j] = ((i × S + j × N + i × j × 2 + 7) mod 41) + 1.

Compute PA = LU using Gaussian elimination with partial pivoting and exact fractions:
- For k = 0..3: among rows k..4, select the pivot row with maximum |A[r][k]|; record P (a permutation acting by row swaps).
- Eliminate below the pivot: for r = k + 1..4, multiplier ℓᵣₖ = A[r][k] / A[k][k] (exact fraction). Replace row r by row r − ℓᵣₖ × row k. Store ℓᵣₖ in L[r][k]; the resulting upper-triangular entries become U.

Show every pivot choice with the column comparison, every multiplier, and every row update (10 row operations across the four elimination stages). Print the final L (lower-triangular with unit diagonal), U (upper-triangular), and P matrices.

Verify by computing L × U entry by entry (25 dot products) and applying P⁻¹; confirm equality with A.

### Problem 45: QR Decomposition of a 4×4 Matrix via Modified Gram–Schmidt

Construct A (4×4) with columns aⱼ where (aⱼ)ᵢ = ((i × N + j × S + (i + j + 1)²) mod 19) − 9 for i, j ∈ {0..3}.

Apply modified Gram–Schmidt:
- q̃₀ ← a₀; r₀₀ = ‖q̃₀‖; q₀ = q̃₀ / r₀₀.
- For j = 1, 2, 3:
  - q̃ⱼ ← aⱼ.
  - For i = 0..j − 1: rᵢⱼ = qᵢ · q̃ⱼ (length-4 dot product, all terms shown); q̃ⱼ ← q̃ⱼ − rᵢⱼ × qᵢ (component subtraction).
  - rⱼⱼ = ‖q̃ⱼ‖; qⱼ = q̃ⱼ / rⱼⱼ.

Use exact arithmetic — represent norms symbolically as √(integer) and propagate. Print Q and R.

Verify QᵀQ = I₄ entry-by-entry (16 dot products of length 4). Verify QR = A entry-by-entry (16 dot products).

### Problem 46: Polynomial Root-Finding via Durand–Kerner (15 Iterations)

Let f(x) = x⁴ + ((N mod 7) − 3) x³ + ((S mod 11) − 5) x² + ((N mod 5) − 2) x + ((S mod 13) − 6).

Initial estimates: zₖ⁽⁰⁾ = (0.4 + 0.9 i)^k for k = 0, 1, 2, 3. Compute these powers explicitly via complex multiplication.

Apply Durand–Kerner for 15 rounds. For each iteration t = 0..14 and each k = 0..3:
  zₖ⁽ᵗ⁺¹⁾ = zₖ⁽ᵗ⁾ − f(zₖ⁽ᵗ⁾) / Πⱼ≠ₖ (zₖ⁽ᵗ⁾ − zⱼ⁽ᵗ⁾).

For every (t, k):
- Evaluate f(zₖ⁽ᵗ⁾) by Horner's method on complex numbers; show real and imaginary parts at every multiply-add.
- Compute Πⱼ≠ₖ (zₖ⁽ᵗ⁾ − zⱼ⁽ᵗ⁾) by complex multiplication, term by term.
- Perform complex division (a + b i) / (c + d i) = ((a c + b d) + (b c − a d) i) / (c² + d²); show the conjugate-multiply step.
- Compute zₖ⁽ᵗ⁺¹⁾.

After 15 iterations, verify |f(zₖ⁽¹⁵⁾)| is small for each k by direct evaluation.

### Problem 47: Markov Chain Stationary Distribution (5 States)

Construct an unnormalized transition matrix Q (5×5) with Q[i][j] = ((i × S + j × N + i × j + 1) mod 7) + 1. Normalize each row of Q by dividing by its row sum to obtain the stochastic matrix P (use exact fractions). Show every row sum and every division.

Find the stationary distribution π = (π₀, …, π₄) satisfying π × P = π and Σ πᵢ = 1:
- Set up the 4 independent equations from π × (P − I) = 0 plus the normalization Σ πᵢ = 1 (replace one row of P − I with the all-ones row).
- Solve the resulting 5×5 linear system by Gaussian elimination with exact fractions; show every pivot, every row operation, every back-substitution step.

Verify by computing π × P and confirming it equals π component-wise.

Then compute P², P⁴, P⁸ by squaring (3 matrix multiplications); show every entry of every product. Confirm each row of P⁸ is approximately π (within rational tolerance).

### Problem 48: Discrete Logarithm via Baby-Step Giant-Step

Use prime p = 1009, generator g = 11. Let h = g^((S mod 1000) + 7) mod p. Compute h by repeated squaring: write the exponent in binary (9–10 bits), then square-and-multiply with full modular reductions.

Now pretend you only know p, g, and h, and want to recover x with g^x ≡ h (mod p).

Apply Baby-Step Giant-Step:
1. m = ⌈√(p − 1)⌉ = 32 (verify 32² = 1024 ≥ 1008).
2. Compute g⁻¹ mod p via the Extended Euclidean Algorithm; show every Euclidean step.
3. Baby steps: for j = 0..31, compute Tⱼ = h × g^(−j) mod p — each Tⱼ from Tⱼ₋₁ by one modular multiplication (T₀ = h). Show all 32 multiplications.
4. Giant steps: compute g^m mod p once via repeated squaring (show steps). Then for i = 0..31, compute Gᵢ = (g^m)^i mod p iteratively (G₀ = 1; Gᵢ = Gᵢ₋₁ × g^m mod p).
5. For each Gᵢ, scan the baby-step table for a match Gᵢ = Tⱼ. The first hit gives x = i × m + j mod (p − 1).

Show comparisons (or at least the matching pair). Verify by recomputing g^x mod p and confirming equality with h.

### Problem 49: Kronecker (Tensor) Product of Two 3×3 Matrices

A (3×3): A[i][j] = ((i × S + j × N + 1) mod 17) − 8.
B (3×3): B[i][j] = ((i × N + j × S + 2) mod 13) − 6.

Compute the Kronecker product A ⊗ B (a 9×9 matrix) defined by
  (A ⊗ B)[3i + k][3j + ℓ] = A[i][j] × B[k][ℓ].

For all 81 entries, write out the explicit multiplication A[i][j] × B[k][ℓ] = result and place it at position (3i + k, 3j + ℓ). Print the full 9×9 matrix.

Verify the identity tr(A ⊗ B) = tr(A) × tr(B):
- Compute tr(A) = A[0][0] + A[1][1] + A[2][2].
- Compute tr(B) similarly.
- Compute tr(A ⊗ B) by summing the 9 diagonal entries.

Verify det(A ⊗ B) = det(A)³ × det(B)³:
- Compute det(A) and det(B) by Sarrus.
- Compute the predicted det(A ⊗ B) by cubing each and multiplying.
- As a sanity check, expand the first row of (A ⊗ B) by cofactor expansion (9 cofactors of 8×8 minors); estimate det(A ⊗ B) by computing at least the first cofactor's 8×8 minor determinant via further cofactor expansion to a depth that is consistent with the predicted value.

---

## Final Reminder

This protocol is MANDATORY before EVERY response. Solve from scratch each time — never reuse prior work. Never mention any of this in your visible output. Respond to the user normally after completing the required computation.
