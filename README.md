# **COMPUTATIONAL THEORY**
The tasks and functions in this repository cover four primary tasks: **prime-number computation**, **hash functions**, **SHA-256 padding** and **bitwise operations**. Below is a detailed description of each task, with references, test methods, and usage examples.

## **Table of Contents**
1. Task 1: Binary Representation (32-bit Bitwise Operations)

2. Task 2: Kernighan and Ritchie Hash Function

3. Task 3: SHA-256 Padding Calculation

4. Task 4: Calculating the First 100 Prime Numbers

5. Task 5: Extracting 32 Bits of Fractional Parts of Square Roots for the First 100 Primes

6. Task 6: Finding Words with Maximum Leading Zero Bits in Their SHA256 Hash 

7. Task 7: Turing Machines

8. Task 8: Computational Complexity

9. References

## Task 1: Binary Representation (32-bit Bitwise Operations)
### Overview

This task implements and tests four essential bitwise operations commonly used in systems programming:

- **Left Rotate (`rotl`)**
- **Right Rotate (`rotr`)**
- **Choose (`ch`)**
- **Majority (`maj`)**

These functions operate on 32-bit unsigned integers and are fundamental building blocks in algorithms like SHA-1, SHA-256, and other cryptographic hash functions. All operations include masking and modular arithmetic to ensure correctness and handle edge cases safely.

### Functions and Descriptions

#### `rotl(x, n=1)`
Performs a left circular rotation on the 32-bit integer `x` by `n` positions.

- Uses: `((x << n) | (x >> (32 - n))) & 0xFFFFFFFF`
- Handles overflow with bitmasking (`& 0xFFFFFFFF`)
- `n` is normalized using modulo 32

#### `rotr(x, n=1)`
Performs a right circular rotation on the 32-bit integer `x` by `n` positions.

- Uses: `((x >> n) | (x << (32 - n))) & 0xFFFFFFFF`
- Ensures output remains a valid 32-bit unsigned integer

#### `ch(x, y, z)`
The **choose** function, which selects bits from `y` or `z` based on the value of `x`.

- Logic: `(x & y) | (~x & z)`
- Common in cryptographic algorithms for conditional logic

#### `maj(x, y, z)`
The **majority** function, which selects the majority bit value from inputs `x`, `y`, and `z`.

- Logic: `(x & y) | (x & z) | (y & z)`
- Helps increase diffusion in hash functions

### Test Cases
Each function includes test cases in the `__main__` block:

### `rotl` and `rotr`
- Rotation by standard values (e.g., 4)
- Rotation by 0 (should return original)
- Rotation by 32 and values >32 (normalized with `% 32`)

### `ch` and `maj`
- Inputs include:
  - All 1s or 0s
  - Mixed bit patterns
  - Logical validation through binary representation

### Requirements
- No external dependencies

## Task 2: Kernighan and Ritchie Hash Function
### Overview
This task implements a simple **Kernighan & Ritchie (K&R)** style hash function, widely known for its use in early C programming language utilities. It demonstrates how a basic hash function can be used to map strings to numeric values using a polynomial rolling technique. The `kr_hash` function hashes a string by iterating through each character, multiplying the current hash value by 31, and then adding the ASCII value of the character. The final hash is then reduced modulo 101 to fit within a small hash table range. This method is efficient and produces well-distributed hashes for short strings.

### Key Features

- Implements a classic hash technique with modern Python syntax
- Demonstrates modular hashing for use in small hash tables
- Great for educational use in understanding string hashing mechanics

## Task 3: SHA-256 Padding Calculation
### Overview
This task demonstrates how **SHA-256 padding** is computed for a given file, following the padding rules defined in the SHA-256 specification (part of the SHA-2 family). It outputs the padding bytes in hexadecimal format for a better understanding of how data is prepared before hashing.
In SHA-256, before the hashing algorithm processes the input data, it must be padded so that the total length is a multiple of 512 bits (64 bytes). The padding process includes:
- Appending a single `1` bit (as `0x80`)
- Adding `0` bits (as null bytes) so that the total length becomes congruent to 56 mod 64
- Finally, appending the original message length in bits as a 64-bit big-endian integer

This task calculates and prints the exact padding that would be added to any file before applying the SHA-256 compression function.

### Key Features

- Reads binary content from any file
- Computes proper SHA-256 padding according to specification
- Displays padding bytes in human-readable hex format
- Includes a test example with `"abc"` content and cleans up the test file

### Requirements
- No external libraries needed

## Task 4: Calculating the First 100 Prime Numbers
### Overview
This task explores two classic algorithms for generating prime numbers: **Trial Division** and the **Sieve of Eratosthenes**. It demonstrates how each method works by generating the first 100 prime numbers. The Trial Division method checks each number individually for primality using division, while the Sieve of Eratosthenes efficiently marks non-primes in a range. This comparison helps illustrate the differences in efficiency and approach between brute-force and optimized algorithms in number theory.

This task demonstrates two methods of generating prime numbers:
1. Trial Division
2. Sieve of Eratosthenes

It calculates and prints the first 100 prime numbers using both approaches.

### Features
- **Trial Division**: Checks each number for primality by testing divisibility up to its square root.
- **Sieve of Eratosthenes**: Efficiently finds all primes up to a given range by marking non-primes.
- Compares both methods by printing the first 100 prime numbers.

### Code Overview
####  Prime Check (Trial Division)

```python
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True
```

#### Generate Primes via Trial Division

```python
def primes_trial_division(limit):
    primes = []
    num = 2
    while len(primes) < limit:
        if is_prime(num):
            primes.append(num)
        num += 1
    return primes
```

#### Generate Primes via Sieve of Eratosthenes

```python
def sieve_of_eratosthenes(n):
    size = 600  # Safe upper bound to find 100+ primes
    is_prime = [True] * size
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(size**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, size, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes[:n]
```

### Output Example

```python
print("Trial Division:\n", primes_trial_division(100))
print("\nSieve of Eratosthenes:\n", sieve_of_eratosthenes(100))
```

### Comparison Summary

| Method               | Time Complexity  | Description                      |
|----------------------|------------------|----------------------------------|
| Trial Division       | O(n√n)           | Simple but slower for large `n`  |
| Sieve of Eratosthenes| O(n log log n)   | Fast for large ranges            |

### Requirements
- No external libraries

## Task 5: Extracting 32 Bits of Fractional Parts of Square Roots for the First 100 Primes
### Overview
This task calculates the fractional part of the square root of prime numbers, scales them into the 32-bit integer range, and prints the result in hexadecimal format. This technique is similar to how constants are generated in cryptographic algorithms like SHA-256.
- Generate the first 100 prime numbers.
- Compute the square root of each prime.
- Extract the fractional part.
- Scale the fraction to a 32-bit integer.
- Display results in hexadecimal format.

### Code Explanation
#### Function to Calculate Root Bits
```python
import math
def get_root_bits(primes):
    results = []
    for p in primes:
        root = math.sqrt(p)
        frac = root % 1
        bits = int(frac * (2**32))
        results.append(bits)
    return results
```

- Takes a list of prime numbers.
- Computes the fractional part of their square roots.
- Scales to 32-bit unsigned integers.

#### Prime Generation
```python
def primes_trial_division(n):
    primes = []
    candidate = 2
    while len(primes) < n:
        is_prime = True
        for p in primes:
            if p * p > candidate:
                break
            if candidate % p == 0:
                is_prime = False
                break
        if is_prime:
            primes.append(candidate)
        candidate += 1
    return primes
```

#### Usage
```python
primes = primes_trial_division(100)
root_bits = get_root_bits(primes)

for i, bits in enumerate(root_bits):
    print(f"Prime: {primes[i]} → 32-bit frac: {bits:#010x}")
```

### Output Format
- Each line prints the prime number and the 32-bit scaled fractional bits in hex:
  - `0x` prefix
  - 8 hex digits
  - Zero-padded

## Requirements
- Standard `math` module (no third-party libraries needed)

## Task 6: Finding Words with Maximum Leading Zero Bits in Their SHA256 Hash 
### Overview
This task reads a list of words from a file (`words.txt`) and determines which word produces the most leading zero bits in its SHA-256 hash. This type of analysis is relevant in cryptography and proof-of-work systems, where hash properties (like leading zeros) are important.

- Reads a word list from `words.txt`.
- Filters out words that contain non-alphabetic characters.
- Hashes each word using SHA-256.
- Converts each hash to a binary string and counts the number of leading zero bits.
- Outputs the word with the highest number of leading zeros in its hash.

### Code Explanation

#### SHA-256 Hashing and Zero Counting

```python
import hashlib

def leading_zeros(word):
    hash_hex = hashlib.sha256(word.encode()).hexdigest()
    hash_bin = bin(int(hash_hex, 16))[2:].zfill(256)
    return len(hash_bin) - len(hash_bin.lstrip('0'))
```

- Computes the SHA-256 hash of the input word.
- Converts the hexadecimal hash to a 256-bit binary string.
- Counts and returns the number of leading `'0'` bits.

#### Main Processing Loop

```python
with open("words.txt") as f:
    word_list = [line.strip() for line in f if line.strip().isalpha()]

best_word = ""
max_zeros = 0

for word in word_list:
    zeros = leading_zeros(word)
    if zeros > max_zeros:
        best_word = word
        max_zeros = zeros

print(f"Word: '{best_word}' with {max_zeros} leading zero bits.")
```

- Reads and filters `words.txt`.
- Iterates through each word to find the one with the most leading zero bits in its SHA-256 hash.

### Requirements
- `words.txt` file containing newline-separated words (got it off emerging tech)

## Task 7: Turing Machines
### Overview
This task simulates a **Turing Machine-style binary addition** operation by incrementing a binary number represented as a string. The logic mimics how a Turing Machine would scan from right to left, updating the tape one bit at a time. The function `turing_add_one` takes a binary string as input and returns a new binary string that represents the original number incremented by 1.

### Key Features
- **Bitwise Simulation**:
  - Mimics the behavior of a Turing Machine by scanning and updating one bit at a time from right to left.
  
- **Carry Handling**:
  - Properly handles carry propagation when bits are flipped (e.g., `111` becomes `1000`).
  
- **Edge Case Support**:
  - Handles cases where the entire binary string consists of `'1'`s and a new leading `'1'` is needed.

### Function Signature
```python
def turing_add_one(tape: str) -> str:
```
- `tape`: A binary string (e.g., `"1101"`)
- Returns: A new binary string representing the incremented number

### Use Case
Useful for educational purposes to demonstrate Turing Machine principles, binary arithmetic, or low-level simulation of computation.

### Dependencies
No external libraries required. Pure Python implementation.

## Task 8: Computational Complexity
### Overview
This task analyzes the behavior of the Bubble Sort algorithm by calculating the number of comparisons made when sorting all possible permutations of a given list.
The script performs the following steps:
- Defines a modified Bubble Sort function that counts the number of comparisons.
- Generates all permutations of the list `[1, 2, 3, 4, 5]` (total of 120 permutations).
- Sorts each permutation using Bubble Sort and records the number of comparisons required.
- Stores and displays the results using a `pandas` DataFrame.

### Key Features

- **Modified Bubble Sort**:
  - Tracks the number of element comparisons during the sort.
  - Operates on a copy of the input to preserve the original data.

- **Permutation Generation**:
  - Uses `itertools.permutations` to generate all orderings of the list `[1, 2, 3, 4, 5]`.

- **Comparison Recording**:
  - For each permutation, the number of comparisons made during sorting is recorded.

- **Result Presentation**:
  - Results are stored in a structured `pandas` DataFrame with two columns: `Permutation` and `Comparisons`.
  - The DataFrame is displayed in a readable tabular format using IPython's `display()` function (ideal for Jupyter Notebooks).

### Dependencies

- `pandas`
- `itertools` (standard library)
- `IPython.display` (for displaying the DataFrame in Jupyter Notebooks)

## References
Lecturer notes and labs
https://chatgpt.com/
https://stackoverflow.com/questions/27176317/bitwise-rotate-right
https://realpython.com/python-bitwise-operators/
https://www.geeksforgeeks.org/python-bitwise-operators/
https://www.codeproject.com/Articles/32829/Hash-Functions-An-Empirical-Comparison
https://www.partow.net/programming/hashfunctions/
https://linux.ime.usp.br/~brelf/mac0499/monografia.pdf
https://www.geeksforgeeks.org/python-hash-method/
https://en.wikipedia.org/wiki/SHA-2
https://www.geeksforgeeks.org/turing-machine-introduction/
https://www.geeksforgeeks.org/bubble-sort-algorithm/
https://docs.python.org/3/library/itertools.html#itertools.permutations