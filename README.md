<!--![computational theory banner](https://github.com/caolanmaguire/calsickofthis/blob/main/COMPUTAT.png)-->

# Computational Theory
Repository for all tasks and work from the Computational Theory Module.

<!--
## Abstract

This semester-long project explores fundamental concepts in computational theory, examining both classical and contemporary approaches to computation. The work investigates binary representations, cryptographic hashing, prime number theory, and Turing machines to establish connections between historical computational principles and modern applications. Through practical implementations and theoretical analysis, this project contributes to the understanding of computational complexity and its implications for computer science as described by Sipser (2012) in his seminal work on the theory of computation.-->

## Task 1: Binary Representations

This task implements fundamental bitwise operations that are essential in computer science, particularly in cryptography, system-level programming, and hardware interfaces. The functions provide efficient bit-level manipulations that form the foundation for various algorithms and security protocols [1].

### Implementation

#### Bit Rotation Functions

The `rotl()` and `rotr()` functions implement circular bit shifting operations for 32-bit unsigned integers. Unlike standard bit shifts, rotations preserve all bits by wrapping them around rather than discarding them.

**Applications:**
- Hardware register manipulation
- Hash function algorithms (SHA-256, MD5) [2]
- Efficient data permutation in encryption

For example, rotating the 8-bit value `10110011`:
- Left rotation by 2: `11001110`
- Right rotation by 3: `01110110`

The implementation enforces 32-bit boundaries using bitwise AND with `0xFFFFFFFF` and combines shifted components with bitwise OR operations.


```python
def rotl(x, n=1):
    """Rotate left (circular shift) a 32-bit unsigned integer x by n bits."""
    return ((x << n) | (x >> (32 - n))) & 0xFFFFFFFF

def rotr(x, n=1):
    """Rotate right (circular shift) a 32-bit unsigned integer x by n bits."""
    return ((x >> n) | (x << (32 - n))) & 0xFFFFFFFF
```

#### Choice Function
The `ch()` function implements a bitwise choice operation used in cryptographic hash functions like SHA-256. It selectively chooses bits from two inputs (y and z) based on a control input (x).
Operation:

For each bit position in the 32-bit values:

If the corresponding bit in x is 1, select the bit from y
If the corresponding bit in x is 0, select the bit from z



**Applications:**

Core component in SHA-256 and other hash algorithms
Efficient conditional bit selection without branching
Creating complex bit patterns in cryptographic operations

The implementation uses boolean logic with bitwise operators (AND, XOR, NOT) and ensures 32-bit boundaries with a mask.

#### Majority Function

The `maj()` function implements a bitwise majority operation commonly used in cryptographic hash functions such as SHA-256. It determines the most common bit value across three inputs for each bit position.
Operation:

For each bit position in the 32-bit values:

Output 1 if two or more of the corresponding bits in x, y, and z are 1
Output 0 if two or more of the corresponding bits are 0



**Applications:*

Essential component in SHA-256 and related hash algorithms
Provides nonlinearity in cryptographic primitives
Implements bit-level "voting" logic without conditionals

The implementation uses a combination of bitwise AND and XOR operations to efficiently compute the majority value across all 32 bit positions simultaneously, with a final mask ensuring the result stays within 32-bit boundaries.

#### Usage Examples

#### Tests

```python
    #Tests for each of Binary Representations Functions
    def test_rotl():
        assert rotl(0x00000001, 1) == 0x00000002
        assert rotl(0x80000000, 1) == 0x00000001
        assert rotl(0x12345678, 4) == 0x23456781
        assert rotl(0xFFFFFFFF, 16) == 0xFFFFFFFF

    def test_rotr():
        assert rotr(0x00000001, 1) == 0x80000000
        assert rotr(0x80000000, 1) == 0x40000000
        assert rotr(0x12345678, 4) == 0x81234567
        assert rotr(0xFFFFFFFF, 16) == 0xFFFFFFFF
```

## Task 2: Hash Functions

The following hash function is from The C Programming Language by Brian Kernighan and Dennis Ritchie.
Convert it to Python, test it, and suggest why the values 31 and 101 are used.

`unsigned hash(char *s) {
    unsigned hashval;
    for (hashval = 0; *s != '\0'; s++)
        hashval = *s + 31 * hashval;
    return hashval % 101;
}`

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 3: SHA256

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 4: Prime Numbers

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 5: Mathematical Roots

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 6: Proof of Work

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 7: Turing Machines

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## Task 8: Computational Complexity

#### Implementation

#### Choice Function

#### Majority Function

#### Usage Examples

#### Tests

## References

[1] [Computer Organisation and design: The Hardware/Software Interface - Patterson & Hennessy](https://shop.elsevier.com/books/computer-organization-and-design-mips-edition/patterson/978-0-12-820109-1)


<!--Arora, S., & Barak, B. (2009). *Computational Complexity: A Modern Approach*. Cambridge University Press.

Cook, S. A. (1971). The complexity of theorem-proving procedures. In *Proceedings of the Third Annual ACM Symposium on Theory of Computing* (pp. 151-158).

National Institute of Standards and Technology. (2015). *Secure Hash Standard (SHS)*. FIPS PUB 180-4.

Sipser, M. (2012). *Introduction to the Theory of Computation* (3rd ed.). Cengage Learning.

Turing, A. M. (1936). On computable numbers, with an application to the Entscheidungsproblem. *Proceedings of the London Mathematical Society*, 2(1), 230-265.

Weizenbaum, J. (1966). ELIZA—a computer program for the study of natural language communication between man and machine. *Communications of the ACM*, 9(1), 36-45.-->
