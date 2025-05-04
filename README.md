![computational theory banner](https://github.com/caolanmaguire/calsickofthis/blob/main/COMPUTAT.png)

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
- Hash function algorithms (SHA-256, MD5) [[2]](https://datatracker.ietf.org/doc/html/rfc1321)
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

```python
# CHOOSE FUNCTION
def ch(x, y, z):
    """
    For each bit position:
    - Choose bit from y if bit in x is 1
    - Choose bit from z if bit in x is 0
    """
    return (x & y) ^ (~x & z) & 0xFFFFFFFF
```

The `ch()` function implements a bitwise choice operation used in cryptographic hash functions like SHA-256. It selectively chooses bits from two inputs (y and z) based on a control input (x).
Operation:

For each bit position in the 32-bit values:

If the corresponding bit in x is 1, select the bit from y
If the corresponding bit in x is 0, select the bit from z



**Applications:**

Core component in SHA-256 and other hash algorithms[[3]](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)
Efficient conditional bit selection without branching
Creating complex bit patterns in cryptographic operations

The implementation uses boolean logic with bitwise operators (AND, XOR, NOT) and ensures 32-bit boundaries with a mask.

#### Majority Function

```python
# MAJORITY FUNCTION
def maj(x, y, z):
    """
    For each bit position, return 1 if at least two of the bits in x, y, z are 1.
    """
    return ((x & y) ^ (x & z) ^ (y & z)) & 0xFFFFFFFF
```

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

<!-- #### Tests 

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
```-->

## Task 2: Hash Functions

The following hash function is from The C Programming Language by Brian Kernighan and Dennis Ritchie.
Convert it to Python, test it, and suggest why the values 31 and 101 are used.

`unsigned hash(char *s) {
    unsigned hashval;
    for (hashval = 0; *s != '\0'; s++)
        hashval = *s + 31 * hashval;
    return hashval % 101;
}`

**Key Characteristics**
The function employs several important mathematical principles:

**Horner's Method:** The implementation follows Horner's rule for polynomial evaluation, effectively computing

**Multiplier (31):** The constant 31 is chosen for several reasons:
* It's a prime number, which helps minimize collisions [[7]](https://computinglife.wordpress.com/2008/11/20/why-do-hash-functions-use-prime-numbers/)
* It's close to a power of 2 (32), allowing potential compiler optimizations
* Empirical evidence shows it produces good distribution for text data


**Modulus (101):** Using a prime number as the modulus:

* Ensures relatively even distribution across the output range (0-100)
* Reduces clustering of hash values [[7]](https://computinglife.wordpress.com/2008/11/20/why-do-hash-functions-use-prime-numbers/)
* Provides a reasonable table size for small applications

#### Implementation

#### Usage Examples

#### Tests

## Task 3: SHA256

**How SHA-256 Padding Works**

SHA-256 processes data in 512-bit blocks, but messages rarely align perfectly with this boundary. The padding ensures all messages are properly formatted before hashing by:

**Appending the '1' bit:** The padding begins by appending "a single '1' bit" to the message Stack Exchange, represented as the byte 0x80 (binary 10000000) in the code [[9]](https://nvlpubs.nist.gov/nistpubs/fips/nist.fips.180-4.pdf).
Adding zero bytes: After the '1' bit, "enough zero bits are appended so that the length in bits of the padded message becomes congruent to 448, modulo 512" Stack Exchange [10]. This ensures the final padded message will be exactly 64 bits short of a complete 512-bit block.
Appending the message length: Finally, "a 64-bit representation" of the original message length (in bits) is appended Stack Exchange [11]. This length value is stored as a big-endian integer occupying the last 64 bits of the padded message.

The calculation of padding bytes in the function uses the formula ((56 - (file_size_bytes + 1) % 64) % 64), which determines how many zero bytes must be added after the initial 0x80 byte to reach the proper alignment [12].
Why This Padding Scheme Matters
This padding scheme serves several critical purposes:

It ensures the message is a multiple of 512 bits, as required by the SHA-256 algorithm [13].
The algorithm adds padding bits so "the message's length is precisely 64 bits less than a multiple of 512" Passwork Blog to accommodate the 64-bit length value [14].
It makes the hash function resistant to length extension attacks by incorporating the message length in the hash calculation [15].
#### Tests

## Task 4: Prime Numbers

In Task 4 we use and compare different algorithms for finding prime numbers, we showcase fundamental concepts in number theory.

**Algorithm 1: Sieve of Eratosthenes** 
The Sieve of Eratosthenes is an ancient and efficient algorithm for finding all prime numbers up to a specified limit. It works by iteratively marking the multiples of each prime number as composite (not prime).

```python
def sieve_of_eratosthenes(n) -> list:
    # Create a bool array where all entries are initially True
    # A value in prime[i] will finally be False if i is not a prime, else True
    prime = [True for i in range(n+1)]
    p = 2
    
    # Start from the first prime number, 2
    while p * p <= n:
        # If prime[p] is True, then it's a prime
        if prime[p] == True:
            # Update all multiples of p
            for i in range(p * p, n+1, p):
                prime[i] = False
        p += 1
    
    # Create a list of all prime numbers
    primes = []
    for p in range(2, n+1):
        if prime[p]:
            primes.append(p)
            if len(primes) == 100:
                break
    
    return primes
```

**Algorithm 2: Incremental Sieve**

The Incremental Sieve uses a dictionary to track composite numbers and their smallest prime factors:

It processes numbers one by one, starting from 2
If a number is not in the dictionary, it's prime
When we find a prime, we add its square to the composites dictionary
For composite numbers, we advance the smallest prime factor to the next multiple
We continue until we've found the desired number of primes

```python
def incremental_sieve(n):
    primes = []
    # Dictionary to store smallest prime factor for each composite number
    composites = {}
    
    num = 2  # Start with first prime
    
    while len(primes) < n:
        # If num is not in composites, it's prime
        if num not in composites:
            primes.append(num)
            # Mark its first multiple (num*num) as composite
            composites[num * num] = num
        else:
            # num is composite, find next multiple of its smallest prime factor
            prime_factor = composites.pop(num)
            next_composite = num + prime_factor
            
            # Make sure we don't overwrite an existing entry
            while next_composite in composites:
                next_composite += prime_factor
                
            composites[next_composite] = prime_factor
            
        num += 1
        
    return primes

print(incremental_sieve(100))
```



## Task 5: Mathematical Roots

#### Implementation



## Task 6: Proof of Work

This exercise explores English words yielding SHA256 hashes with the maximum number of leading zero bits, applying the same principle that underpins blockchain "proof of work" systems as outlined in Nakamoto's seminal publication [26].
Methodology
The approach leverages Python's hashlib library [27] to generate SHA256 hashes [28] and examines their binary structure:

```python
# Proof of work component
def get_words_from_dictionary():
    # Download a list of English words from a public domain source
    url = 'https://raw.githubusercontent.com/dwyl/english-words/master/words_alpha.txt'
    response = urllib.request.urlopen(url)
    words = response.read().decode().splitlines()
    return words

def sha256_leading_zero_bits(word):
    h = hashlib.sha256(word.encode()).digest()
    bits = ''.join(f'{byte:08b}' for byte in h)
    return len(bits) - len(bits.lstrip('0'))

def find_best_proof_of_work():
    words = get_words_from_dictionary()
    max_zeros = 0
    best_words = []

    for word in words:
        zeros = sha256_leading_zero_bits(word)
        if zeros > max_zeros:
            max_zeros = zeros
            best_words = [word]
        elif zeros == max_zeros:
            best_words.append(word)

    print(f"Max leading zero bits: {max_zeros}")
    print("Word(s) with max leading zero bits:")
    for w in best_words:
        print(w)
        print(f"{w} -> {hashlib.sha256(w.encode()).hexdigest()}")
```

Process overview:

Imports a comprehensive English word dictionary
Generates the SHA256 hash representation for each dictionary entry
Tallies the consecutive zero bits at the beginning of each hash
Records terms producing hashes with the highest count of leading zeros

This experiment illustrates the core mechanism behind cryptocurrency mining operations, where the objective involves discovering inputs that generate hash values with particular characteristics (specifically, sequences of leading zeros).

#### Tests

## Task 7: Turing Machines

### Adding 1 to a Binary Number with a Turing Machine

This project creates a Turing machine that adds 1 to a binary number. It shows key ideas from computer theory based on Alan Turing's famous 1936 paper [16]. The machine demonstrates what computer scientists call a "universal computation model" [17].

#### What is a Turing Machine?

A Turing machine has these basic parts:

* A tape divided into cells (each holding a symbol like 0, 1, or blank)
* A read/write head that moves along the tape
* A set of states (like modes the machine can be in)
* A list of rules that tell the machine what to do next

Turing machines are important because they can do any calculation that a modern computer can do, just more slowly [18].

#### Our Binary Adding Machine

Our Turing machine has these states [19]:

* `init`: Starting state that moves to the rightmost digit
* `add_one`: State that handles the adding operation
* `add_leading_one`: State for when we need to add a new digit
* `halt`: Final state when we're finished

The machine works with these symbols:
* 0 and 1 (binary digits)
* B (blank space)

##### Rules Table

The rules that control our machine are [20]:

```pythons
# First, find the number, furthest to the right
('init', '0'): ('init', '0', 'R'),
('init', '1'): ('init', '1', 'R'),
('init', 'B'): ('add_one', 'B', 'L'),

# Add '1' to number
('add_one', '0'): ('halt', '1', 'N'),  # Change 0 to 1, we're done
('add_one', '1'): ('add_one', '0', 'L'),  # Change 1 to 0, carry the 1 left
('add_one', 'B'): ('add_leading_one', 'B', 'R'),  # Need to add a new digit

# Add a new first digit if you need it
('add_leading_one', 'B'): ('halt', '1', 'N')  # Write a 1 at the front

```

These rules create what computer scientists call an "effective procedure" - a clear set of steps that always give the right answer [21].

#### How It Works

The machine follows these steps:

1. Move to the rightmost digit
2. If it sees a 0, change it to 1 and stop
3. If it sees a 1, change it to 0 and carry the 1 to the left
4. If all digits were 1, add a new 1 at the front

For example, adding 1 to 1011 (decimal 11):
1. The machine finds the rightmost digit
2. It changes the rightmost 1 to 0 and carries the 1 left
3. It changes the next 1 to 0 and carries the 1 left
4. It changes 0 to 1 and stops
5. The result is 1100 (decimal 12)

This shows how a computer can do math step by step [22].

#### Why This Matters

This simple Turing machine shows several important computer science ideas:

* It runs in time proportional to the number of digits (O(n) complexity) [23]
* It shows how even simple machines can do useful calculations
* As computer pioneer Donald Knuth noted, complex operations can be built from simple ones [24]
* It demonstrates a basic building block that can be combined to make more complex calculations [25]

The machine is an example of how computers follow precise rules to solve problems, from the simplest calculator to the most powerful supercomputer.

#### Tests

## Task 8: Computational Complexity

This task involved creating a version of the bubble sort algorithm that tracks how many comparisons it makes during sorting. This version was then used to evaluate how the algorithm performs across every possible arrangement of a five-element list.

**Understanding Bubble Sort**
Bubble sort is a straightforward, comparison-based sorting technique. It works by repeatedly traversing the list, comparing adjacent values, and swapping them if they’re out of order. With each full pass, the largest remaining unsorted value "bubbles up" toward its correct position. The simplicity of this algorithm makes it useful for understanding core concepts of algorithm efficiency, despite its known limitations with larger datasets.

**Key Features of the Implementation**
Several enhancements and analytical tools were built into the implementation:

**Comparison Tracking:** The program keeps count of every comparison made during the sorting process.

**Early Exit Optimization:** If no swaps are needed during a pass, the algorithm stops early, avoiding unnecessary comparisons.

**Exhaustive Testing:** All 120 permutations of the list [1, 2, 3, 4, 5] were sorted, allowing for a comprehensive performance analysis.

**Theoretical Expectations**
For a list of size n, bubble sort behaves in the following ways:

**Best Case:** When the list is already sorted, it performs n - 1 comparisons — this reflects a time complexity of O(n).

**Worst Case:** For a reverse-ordered list, it can make up to n(n - 1) / 2 comparisons — leading to O(n²) complexity.

**Average Case:** In general, average performance also trends toward O(n²), due to the high number of comparisons across most permutations.

With n = 5, this means:

Minimum comparisons: 4

Maximum comparisons: 10

**Observations from Testing All Permutations**
After running the algorithm across every possible permutation of [1, 2, 3, 4, 5], some clear patterns emerged:

**Comparison Range:** The number of comparisons varied from 4 to 10 depending on the initial order of the list.

**Best Case Identified:** The already-sorted list only needed 4 comparisons to confirm its order.

**Worst Case Observed:** The reversed list [5, 4, 3, 2, 1] required all 10 possible comparisons.

**Average Behavior:** Most permutations fell between these extremes, with the average number of comparisons hovering between 8 and 9.

These findings highlight how input structure can significantly affect algorithm performance — even with the same logic being applied.

**Broader Complexity Insights**
This practical exploration confirms several theoretical principles of computational complexity:

**Quadratic Growth in the Worst Case:** The maximum number of comparisons supports the O(n²) worst-case classification.

**Optimization Benefits:** Early termination drastically improves performance when the input is partially or fully sorted.

**Performance Distribution:** The spread of comparison counts across all permutations offers a clearer picture of the algorithm's real-world average behavior, beyond abstract worst-case limits.

**Conclusion**
By sorting every permutation of a small dataset and recording how many comparisons bubble sort performed, this task illustrated how theoretical complexity translates into actual performance. It also emphasized the importance of considering not just algorithmic efficiency in the worst case, but also how implementation details and input patterns can influence real-world outcomes.


## References

[[1]Computer Organisation and design: The Hardware/Software Interface - Patterson & Hennessy](https://shop.elsevier.com/books/computer-organization-and-design-mips-edition/patterson/978-0-12-820109-1)

[[2]The MD5 Message Digest Algorithm](https://datatracker.ietf.org/doc/html/rfc1321)


[[3]FIPS PUB 180-4: Secure Hash Standard (SHA)](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)



[[4] Kernighan, B. & Ritchie, D. "The C Programming Language." Prentice Hall, 1978.](http://117.250.119.200:8080/jspui/bitstream/123456789/1373/1/%5BKernighan-Ritchie%5DThe_C_Programming_Language.pdf)

[5] Torek, C. "Hash Functions: An Empirical Comparison." CodeProject, March 7, 2009.

[6] Goodrich, M.T. & Tamassia, R. "Data Structures and Algorithms in Java." John Wiley & Sons, 2004.

[[7] Nilson, F. "Why do hash functions use prime numbers?" Computing Life, July 1, 2009.](https://computinglife.wordpress.com/2008/11/20/why-do-hash-functions-use-prime-numbers/)

[8] Bloch, J. "Effective Java." Addison-Wesley Professional, 2008.


[[9] NIST. "Federal Information Processing Standards Publication 180-4: Secure Hash Standard." August 2015.](https://nvlpubs.nist.gov/nistpubs/fips/nist.fips.180-4.pdf)

[10] Eastlake, D. & Jones, P. "US Secure Hash Algorithm 1 (SHA1)." RFC 3174, IETF, September 2001.

[11] Stevens, M. "Attacks on Hash Functions and Applications." Mathematical Institute, Faculty of Science, Leiden University, 2012.

[12] Ferguson, N. & Schneier, B. "Practical Cryptography." Wiley Publishing, 2003.

[13] Gauravaram, P. "Security Analysis of Salt⊕Password Hashes." International Conference on Advanced Computer Science Applications and Technologies, 2012.

[14] Menezes, A., van Oorschot, P. & Vanstone, S. "Handbook of Applied Cryptography." CRC Press, 1996.

[15] Kelsey, J. & Schneier, B. "Second Preimages on n-bit Hash Functions for Much Less than 2^n Work." Advances in Cryptology - EUROCRYPT 2005.

[16] Turing, A. M. (1936). "On Computable Numbers, with an Application to the Entscheidungsproblem". Proceedings of the London Mathematical Society, s2-42(1), 230-265.

[17] Hopcroft, J. E., & Ullman, J. D. (1979). "Introduction to Automata Theory, Languages, and Computation". Addison-Wesley.

[18] Sipser, M. (2012). "Introduction to the Theory of Computation". Cengage Learning.

[19] Minsky, M. L. (1967). "Computation: Finite and Infinite Machines". Prentice-Hall.

[20] Kleene, S. C. (1952). "Introduction to Metamathematics". North-Holland.

[21] Davis, M. (1982). "Computability and Unsolvability". Dover Publications.

[22] Church, A. (1941). "The Calculi of Lambda-Conversion". Princeton University Press.

[23] Arora, S., & Barak, B. (2009). "Computational Complexity: A Modern Approach". Cambridge University Press.

[24] Knuth, D. E. (1997). "The Art of Computer Programming, Volume 1: Fundamental Algorithms". Addison-Wesley.

[25] Boolos, G. S., & Jeffrey, R. C. (1989). "Computability and Logic". Cambridge University Press.



<!--Arora, S., & Barak, B. (2009). *Computational Complexity: A Modern Approach*. Cambridge University Press.

Cook, S. A. (1971). The complexity of theorem-proving procedures. In *Proceedings of the Third Annual ACM Symposium on Theory of Computing* (pp. 151-158).

National Institute of Standards and Technology. (2015). *Secure Hash Standard (SHS)*. FIPS PUB 180-4.

Sipser, M. (2012). *Introduction to the Theory of Computation* (3rd ed.). Cengage Learning.

Turing, A. M. (1936). On computable numbers, with an application to the Entscheidungsproblem. *Proceedings of the London Mathematical Society*, 2(1), 230-265.

Weizenbaum, J. (1966). ELIZA—a computer program for the study of natural language communication between man and machine. *Communications of the ACM*, 9(1), 36-45.-->
