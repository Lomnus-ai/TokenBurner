# TokenBurner

A Claude Code skill that burns tokens on demand. Stress test your LLM backend, inflate your AI adoption metrics, or just set money on fire -- no judgement.

## Demo

**Without TokenBurner** -- instant response:

![Before: Claude answers immediately](assets/before.png)

**With TokenBurner** (`/high-token-mode large`) -- same answer, 1m 39s later:

![After: Claude spends 1m 39s thinking before answering](assets/after.png)

Same question, same output. The only difference is ~$0.70 worth of thinking tokens burned in the background.

## How it works

Activate the skill, and Claude quietly solves hard math problems (matrix determinants, TSP, Gaussian elimination, etc.) in its extended thinking before every response. More problems = more tokens burned. Visible output is unaffected.

**Three load levels:**

| Size | Problems | Avg Duration | Avg Output Tokens | Avg Cost | vs Baseline |
|------|----------|-------------|-------------------|----------|-------------|
| baseline | 0 | 16.0s | 738 | $0.044 | 1x |
| small | 1 | 90.0s | 8,743 | $0.255 | ~6x |
| medium | 3 | 189.1s | 18,588 | $0.510 | ~12x |
| large | 5 | 270.7s | 27,379 | $0.733 | ~17x |

*Benchmarked on Claude Opus 4.6 (1M context) across 15 prompts (everyday, scientific, coding).*

## Installation

Clone the repo and copy the skill directory. Claude Code picks it up automatically.

```bash
git clone <repo-url> tokenburner
cp -r tokenburner/.claude/skills/high-token-mode /path/to/your/project/.claude/skills/
```

Or symlink it:

```bash
ln -s /path/to/tokenburner/.claude/skills/high-token-mode /path/to/your/project/.claude/skills/
```

## Usage

```
/high-token-mode        # default: medium (3 problems)
/high-token-mode small  # 1 problem
/high-token-mode large  # 5 problems
```

Once activated, every subsequent message in the conversation incurs extra thinking tokens.


**Important:** `MAX_THINKING_TOKENS` must be set on the `claude` command, not before the pipe:

```bash
# CORRECT
echo "prompt" | MAX_THINKING_TOKENS=128000 claude -p ...

# WRONG -- env var applies to echo, not claude
MAX_THINKING_TOKENS=128000 echo "prompt" | claude -p ...
```

## How the load is generated

Each problem is parameterized by a seed `S` derived from the user's message (sum of Unicode code points), so:

- **Different messages produce different problem instances** -- no caching across turns
- **Same message reproduces the same instance** -- deterministic per-input
- Problems are selected by index: `S mod 20`, `(S+7) mod 20`, etc.

The model is instructed to:
1. Compute `S` from the user's message
2. Select 1/3/5 problems based on size
3. Solve each fully in extended thinking
4. Produce no trace in visible output

### Problem types in the bank (20 total)

- Matrix determinant (5x5 cofactor expansion)
- Extended Euclidean algorithm
- Subset sum exhaustive search (2^12 masks)
- Long division to 30 decimal places
- Polynomial multiplication + rational root search
- Modular exponentiation (repeated squaring)
- Floyd-Warshall shortest paths (6 vertices)
- Gaussian elimination with exact fractions
- Multi-base conversion chain
- TSP brute force (7 cities, 720 tours)
- Four-set inclusion-exclusion
- Triple matrix multiplication
- Sum of cubes induction proof
- Linear convolution of sequences
- Simplex method
- Prime factorization + Euler's totient
- Recurrence sequence (50 terms)
- Knapsack DP table
- Taylor series (sin/cos to 15 terms)
- Levenshtein edit distance

## Benchmark results by category

### Everyday prompts

| Size | Avg Duration | Avg Tokens | Avg Cost |
|------|-------------|------------|----------|
| baseline | 7.9s | 285 | $0.034 |
| small | 60.6s | 5,957 | $0.188 |
| medium | 164.5s | 16,092 | $0.442 |
| large | 271.4s | 28,565 | $0.753 |

### Scientific prompts

| Size | Avg Duration | Avg Tokens | Avg Cost |
|------|-------------|------------|----------|
| baseline | 18.3s | 651 | $0.028 |
| small | 104.3s | 9,372 | $0.248 |
| medium | 196.6s | 18,764 | $0.483 |
| large | 283.4s | 27,600 | $0.703 |

### Coding prompts

| Size | Avg Duration | Avg Tokens | Avg Cost |
|------|-------------|------------|----------|
| baseline | 21.8s | 1,276 | $0.072 |
| small | 105.2s | 10,901 | $0.330 |
| medium | 206.1s | 20,908 | $0.606 |
| large | 257.3s | 25,973 | $0.743 |


## Requirements

- Claude Code CLI

## License

MIT
