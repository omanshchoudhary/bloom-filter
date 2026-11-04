# Bloom Filter

Answers "have I seen this key?" using a bit array instead of storing every key. It never reports an inserted key as absent, but can report an absent key as present.

## Implementation

- **Bit array:** `vector<uint64_t>`, with bit `i` stored in word `i / 64` at position `i % 64`.
- **Hashing:** Double hashing uses `(h1 + k * h2) % m` to generate `k` bit positions from two hashes.
- **Sizing:** Given an expected item count and target false-positive rate, the filter calculates the bit count and number of hashes. The bit count is rounded up to a multiple of 64.

## Build and run

Requires a C++17 compiler and GNU Make.

```bash
make && ./bloom
```

The program runs correctness checks, measures false-positive rates for several targets, and compares measured results with the formula. Use `make clean` to remove the generated executable.

## Example results

For 100,000 inserted keys and 1,000,000 queries:

```text
target FPR   hashes   predicted   measured
10%              3     10.0694%   10.0291%
1%               7      1.0038%    1.0048%
0.1%            10      0.1000%    0.0989%
0.01%           13      0.0100%    0.0108%
```

The results vary with the queried keys. The bit array is sized for the expected number of inserted items; exceeding that count increases the false-positive rate.

## Limitation

Individual keys cannot be removed. Clearing a bit could also clear a bit used by another key.
