# ⚡ Bitwise Tricks for Competitive Programming

For competitive programming, bitwise operations are extremely useful for masks, subsets, parity, powers of two, XOR problems, and optimization.

A CP-focused bitwise cheat sheet, from basic → advanced.

---

## 1. The 6 operators you must know

For integers `a` and `b`:

```cpp
a & b   // AND
a | b   // OR
a ^ b   // XOR
~a      // NOT
a << k  // left shift
a >> k  // right shift
```

Example:

```
a = 5  = 101
b = 3  = 011

a & b  = 001 = 1
a | b  = 111 = 7
a ^ b  = 110 = 6
```

---

## 2. Check whether a number is odd/even

Even:

```cpp
if ((n & 1) == 0)
```

Odd:

```cpp
if (n & 1)
```

Why?

```
5 = 101
    ↑
last bit = 1 → odd

6 = 110
    ↑
last bit = 0 → even
```

---

## 3. Multiply/divide by powers of 2

Multiply by 2:

```cpp
n << 1
```

Multiply by 2^k:

```cpp
n << k
```

Example:

```
5 << 3

means:

5 × 2³
= 5 × 8
= 40
```

Divide by 2^k:

```cpp
n >> k
```

Example:

```
40 >> 3
40 / 8 = 5
```

> For non-negative integers, this is equivalent to integer division by 2^k.

---

## 4. Check whether n is a power of 2 ⭐⭐⭐

One of the most important tricks:

```cpp
n > 0 && (n & (n - 1)) == 0
```

Examples:

```
8  = 1000
7  = 0111

1000
&
0111
----
0000
```

Therefore 8 is a power of 2.

But:

```
10 = 1010
 9 = 1001

1010
&
1001
----
1000
```

Not zero → not a power of 2.

---

## 5. Remove the lowest set bit ⭐⭐⭐

```cpp
n = n & (n - 1);
```

Example:

```
n = 12

12 = 1100
11 = 1011

1100
&
1011
----
1000
```

So:

```
12 → 8
```

This is extremely useful for counting set bits.

---

## 6. Count number of set bits ⭐⭐⭐

```cpp
int cnt = 0;

while (n) {
    n &= (n - 1);
    cnt++;
}
```

Example:

```
n = 13

13 = 1101

Set bits = 3.
```

Each iteration removes one `1`.

**Built-in:**

C++:

```cpp
__builtin_popcount(n);
```

For `long long`:

```cpp
__builtin_popcountll(n);
```

---

## 7. Get the lowest set bit ⭐⭐⭐

```cpp
n & -n
```

Example:

```
n = 12

12 = 1100
-12 = ...0100

12 & -12 = 0100
```

Result:

```
4
```

So:

```cpp
int lowbit = n & -n;
```

This is heavily used in Fenwick Trees (BIT).

---

## 8. Find the position of the lowest set bit

```cpp
__builtin_ctz(n)
```

`ctz` = count trailing zeros.

Example:

```
n = 12
12 = 1100

There are two trailing zeros:

__builtin_ctz(12) // 2
```

Therefore the lowest set bit is at index 2 if zero-indexed.

> ⚠️ Don't call `__builtin_ctz(0)` because its behavior is undefined.

For `long long`:

```cpp
__builtin_ctzll(n);
```

---

## 9. Find the highest set bit ⭐⭐⭐

```cpp
31 - __builtin_clz(n)
```

For `unsigned int`.

Example:

```
n = 13

13 = 1101

Highest set bit index:

3
```

Modern C++20 also provides:

```cpp
std::bit_width(n) - 1
```

---

## 10. Check whether the kth bit is set ⭐⭐⭐

Suppose:

```
n = 13

13 = 1101
          ↑
index: 3 2 1 0
```

Check bit k:

```cpp
if (n & (1 << k))
```

Example:

```cpp
if (n & (1 << 2))
```

Bit 2 is 1.

**Better with 64-bit numbers:**

```cpp
if (n & (1LL << k))
```

---

## 11. Set the kth bit

Turn bit k into 1:

```cpp
n |= (1 << k);
```

Example:

```
n = 8

1000

set bit 1:

1000
0010
----
1010

= 10
```

---

## 12. Clear the kth bit

Turn bit k into 0:

```cpp
n &= ~(1 << k);
```

Example:

```
n = 14

1110

clear bit 2:

1110
1011
----
1010

= 10
```

---

## 13. Toggle the kth bit ⭐⭐

If 0 → 1 and 1 → 0:

```cpp
n ^= (1 << k);
```

Example:

```
n = 10

1010

toggle bit 1:

1010
0010
----
1000

= 8
```

---

## 14. XOR properties ⭐⭐⭐⭐⭐

These are extremely important.

| Property | Formula |
| --- | --- |
| 1 | `a ^ a = 0` |
| 2 | `a ^ 0 = a` |
| 3 | `a ^ b = b ^ a` |
| 4 | `(a ^ b) ^ c = a ^ (b ^ c)` |

Therefore:

```
a ^ b ^ a

becomes:

(a ^ a) ^ b
= 0 ^ b
= b
```

---

## 15. Find the number appearing once ⭐⭐⭐⭐

Given:

```
2 4 3 2 4
```

Every number appears twice except 3.

```cpp
int ans = 0;

for (int x : nums) {
    ans ^= x;
}
```

Why?

```
2 ^ 4 ^ 3 ^ 2 ^ 4

2 ^ 2 = 0
4 ^ 4 = 0

answer = 3
```

Very common CP technique.

---

## 16. Swap two numbers using XOR

```cpp
a ^= b;
b ^= a;
a ^= b;
```

Example:

```
a = 5
b = 7

After:

a = 7
b = 5
```

But in normal competitive programming, just use:

```cpp
swap(a, b);
```

> The XOR trick is mainly useful to understand XOR.

---

## 17. Get all bits below kth bit

```cpp
(1 << k) - 1
```

Example:

```
k = 4

1 << 4 = 10000

10000 - 1
= 01111
```

So:

```cpp
(1 << 4) - 1
```

gives:

```
15
```

This is very useful for bit masks.

---

## 18. Get the lowest k bits ⭐⭐⭐

```cpp
n & ((1 << k) - 1)
```

Example:

```
n = 29

29 = 11101

Get lowest 3 bits:

00111

11101
00111
-----
00101
```

Answer:

```
5
```

---

## 19. Turn all bits after kth bit off

```cpp
n & ((1 << k) - 1)
```

This keeps bits:

```
0 ... k-1
```

and clears everything above them.

---

## 20. Create a mask from bit l to bit r

Suppose you want bits `l ... r`.

A common mask:

```cpp
((1 << (r - l + 1)) - 1) << l
```

Example:

```
l = 2
r = 5

Number of bits:

5 - 2 + 1 = 4

Mask:

1111 << 2
00111100
```

---

## 21. Subset representation ⭐⭐⭐⭐⭐

This is one of the biggest uses of bitwise operations in CP.

Suppose:

```
items = {A, B, C, D}
```

Represent a subset using bits:

```
0000 → {}
0001 → {A}
0010 → {B}
0011 → {A,B}
0100 → {C}
0101 → {A,C}
...
1111 → {A,B,C,D}
```

For n items, there are:

```
2^n
```

subsets.

Loop through all subsets:

```cpp
for (int mask = 0; mask < (1 << n); mask++) {
    for (int i = 0; i < n; i++) {
        if (mask & (1 << i)) {
            // item i is selected
        }
    }
}
```

This pattern is fundamental for subset problems.

---

## 22. Iterate through set bits efficiently ⭐⭐⭐⭐

Instead of checking every bit:

```cpp
while (mask) {
    int bit = __builtin_ctz(mask);

    // process bit

    mask &= mask - 1;
}
```

Example:

```
mask = 101100
```

You process only the positions containing `1`.

Very useful in bitmask DP and graph problems.

---

## 23. Enumerate all submasks ⭐⭐⭐⭐⭐

Very important advanced CP trick:

```cpp
for (int sub = mask; sub; sub = (sub - 1) & mask) {
    // sub is a submask of mask
}
```

Example:

```
mask = 1101
```

The loop generates:

```
1101
1100
1001
1000
0101
0100
0001
```

This appears in:

- Bitmask DP
- Subset DP
- Partition problems
- SOS DP

---

## 24. Enumerate submasks including zero

```cpp
for (int sub = mask;; sub = (sub - 1) & mask) {
    // process sub

    if (sub == 0)
        break;
}
```

---

## 25. Check if a is a submask of b

```cpp
(a & b) == a
```

Example:

```
a = 0101
b = 1101

0101
&
1101
----
0101
```

Therefore a is a submask of b.

---

## 26. XOR range trick ⭐⭐⭐⭐

XOR from 1 to n has a pattern:

| `n % 4` | Result |
| --- | --- |
| 0 | `n` |
| 1 | `1` |
| 2 | `n + 1` |
| 3 | `0` |

So:

```cpp
int xor1ToN(int n) {
    if (n % 4 == 0) return n;
    if (n % 4 == 1) return 1;
    if (n % 4 == 2) return n + 1;
    return 0;
}
```

Then XOR from l to r:

```cpp
xor1ToN(r) ^ xor1ToN(l - 1)
```

Because:

```
(1 ^ 2 ^ ... ^ l-1) ^ (1 ^ 2 ^ ... ^ r)
```

leaves:

```
l ^ l+1 ^ ... ^ r
```

---

## 27. XOR of two numbers tells differing bits

```cpp
a ^ b
```

has:

```
1 → bits are different
0 → bits are same
```

Example:

```
a = 1010
b = 1100

a ^ b

1010
1100
----
0110
```

Bits 1 and 2 differ.

This is useful in:

- Hamming distance
- Binary transformations
- XOR basis
- Bit manipulation problems

---

## 28. Hamming distance

Number of differing bits:

```cpp
__builtin_popcount(a ^ b)
```

Example:

```
a = 10 = 1010
b = 12 = 1100

a ^ b = 0110

Number of 1s:

2
```

So Hamming distance = 2.

---

## 29. Clear the lowest set bit ⭐⭐⭐

```cpp
n &= (n - 1);
```

Remember this one.

It is probably one of the most important bit tricks in CP.

---

## 30. Isolate the lowest set bit ⭐⭐⭐

```cpp
n & -n
```

Remember the difference:

```cpp
n & (n - 1)   // REMOVE lowest set bit
n & -n        // GET lowest set bit
```

---

## 31. Two's complement identity

For signed integers:

```cpp
-n = ~n + 1
```

Therefore:

```cpp
n & -n
```

isolates the lowest set bit.

Understanding this makes many bit tricks much easier.

---

## 32. Power of 2 using highest bit

For positive n, if:

```cpp
(n & (n - 1)) == 0
```

then exactly one bit is set.

Therefore:

```
1
2
4
8
16
32
64
...
```

are powers of 2.

---

## 33. Bitwise NOT ⚠️

```cpp
~n
```

flips every bit.

For example with 8-bit visualization:

```
5  = 00000101
~5 = 11111010
```

With signed integers, the result can look surprising because C++ uses two's-complement representation for signed integers.

> Don't casually use `~` without understanding the integer type.

---

## 34. Common bitmask constants

You'll frequently see:

```cpp
1 << k
(1 << k) - 1
~(1 << k)
n & (1 << k)
n | (1 << k)
n ^ (1 << k)
n & (n - 1)
n & -n
```

Memorize these.

---

# 🧠 CP Bitwise Roadmap

Since you're targeting higher Codeforces ratings, learn them in this order.

**Level 1 — Must know**

```
& | ^
<< >>
```

Then:

- odd/even
- power of 2
- set/clear/toggle bit
- check kth bit
- count set bits
- `n & (n-1)`
- `n & -n`

**Level 2 — Very important**

- XOR properties
- single number
- XOR prefix/range
- bit masks
- subset enumeration
- submask enumeration

**Level 3 — Advanced**

- Bitmask DP
- SOS DP
- XOR basis / Linear Basis
- Trie + XOR
- Gray Code
- Bitset optimization
- Meet-in-the-middle + masks

For your current ~800–900 Codeforces level, don't jump straight into XOR basis/SOS DP. First become extremely comfortable with masks + XOR + set-bit operations + subset/submask enumeration. Those will start appearing naturally in problems you solve.
