# Discrete_Mathematical_Structures_Practicals
# Practical List — Solutions

---

## Practical 1: Class SET with SET Operations

```python
class SET:
    def __init__(self, elements=None):
        self.elements = list(set(elements)) if elements else []

    def display(self):
        print("Set:", self.elements)

    # a. is_member
    def is_member(self, elem):
        return elem in self.elements

    # b. powerset
    def powerset(self):
        result = [[]]
        for e in self.elements:
            result += [subset + [e] for subset in result]
        return result

    # c. subset
    def subset(self, other):
        return all(e in other.elements for e in self.elements)

    # d. union and intersection
    def union(self, other):
        return SET(self.elements + other.elements)

    def intersection(self, other):
        return SET([e for e in self.elements if e in other.elements])

    # e. complement
    def complement(self, universal):
        return SET([e for e in universal.elements if e not in self.elements])

    # f. set difference and symmetric difference
    def difference(self, other):
        return SET([e for e in self.elements if e not in other.elements])

    def symmetric_difference(self, other):
        return SET(
            [e for e in self.elements if e not in other.elements] +
            [e for e in other.elements if e not in self.elements]
        )

    # g. cartesian product
    def cartesian_product(self, other):
        return [(a, b) for a in self.elements for b in other.elements]


# ── Menu-driven program ──────────────────────────────────────────────────────
def main():
    a = SET(list(map(int, input("Enter elements of Set A (space-separated): ").split())))
    b = SET(list(map(int, input("Enter elements of Set B (space-separated): ").split())))
    u = SET(list(map(int, input("Enter elements of Universal Set (space-separated): ").split())))

    while True:
        print("""
--- SET OPERATIONS MENU ---
1. Is Member
2. Power Set
3. Subset Check (A ⊆ B)
4. Union
5. Intersection
6. Complement of A
7. Set Difference (A - B)
8. Symmetric Difference
9. Cartesian Product (A × B)
0. Exit
""")
        choice = input("Enter choice: ")

        if choice == '1':
            elem = int(input("Enter element to check: "))
            print(f"{elem} in A:", a.is_member(elem))
        elif choice == '2':
            print("Power Set of A:", a.powerset())
        elif choice == '3':
            print("A is subset of B:", a.subset(b))
        elif choice == '4':
            a.union(b).display()
        elif choice == '5':
            a.intersection(b).display()
        elif choice == '6':
            a.complement(u).display()
        elif choice == '7':
            a.difference(b).display()
        elif choice == '8':
            a.symmetric_difference(b).display()
        elif choice == '9':
            print("Cartesian Product:", a.cartesian_product(b))
        elif choice == '0':
            break
        else:
            print("Invalid choice!")

main()
```

---

## Practical 2: Class RELATION — Reflexive, Symmetric, Anti-symmetric, Transitive

```python
class RELATION:
    def __init__(self, n, matrix):
        self.n = n            # number of elements
        self.matrix = matrix  # n x n adjacency matrix

    def is_reflexive(self):
        return all(self.matrix[i][i] == 1 for i in range(self.n))

    def is_symmetric(self):
        for i in range(self.n):
            for j in range(self.n):
                if self.matrix[i][j] != self.matrix[j][i]:
                    return False
        return True

    def is_antisymmetric(self):
        for i in range(self.n):
            for j in range(self.n):
                if i != j and self.matrix[i][j] == 1 and self.matrix[j][i] == 1:
                    return False
        return True

    def is_transitive(self):
        for i in range(self.n):
            for j in range(self.n):
                if self.matrix[i][j] == 1:
                    for k in range(self.n):
                        if self.matrix[j][k] == 1 and self.matrix[i][k] == 0:
                            return False
        return True

    def classify(self):
        r = self.is_reflexive()
        s = self.is_symmetric()
        a = self.is_antisymmetric()
        t = self.is_transitive()

        print(f"Reflexive    : {r}")
        print(f"Symmetric    : {s}")
        print(f"Antisymmetric: {a}")
        print(f"Transitive   : {t}")

        if r and s and t:
            print("=> Equivalence Relation")
        elif r and a and t:
            print("=> Partial Order Relation")
        else:
            print("=> None")


# ── Driver ───────────────────────────────────────────────────────────────────
n = int(input("Enter number of elements: "))
print(f"Enter {n}x{n} matrix row by row (space-separated):")
matrix = [list(map(int, input().split())) for _ in range(n)]

rel = RELATION(n, matrix)
rel.classify()
```

---

## Practical 3: Permutations of a Set of Digits

```python
from itertools import permutations, product

def generate_permutations(digits, r, with_repetition):
    if with_repetition:
        result = list(product(digits, repeat=r))
    else:
        result = list(permutations(digits, r))
    return result


digits = list(map(int, input("Enter digits (space-separated): ").split()))
r = int(input("Enter permutation length r: "))
choice = input("Allow repetition? (y/n): ").strip().lower()

perms = generate_permutations(digits, r, choice == 'y')
print(f"\nTotal permutations: {len(perms)}")
for p in perms:
    print(p)
```

---

## Practical 4: List All Solutions of x₁ + x₂ + ... + xₙ = C (Brute Force)

```python
from itertools import product

def find_solutions(n, C):
    solutions = []
    # Each xi can range from 0 to C
    for combo in product(range(C + 1), repeat=n):
        if sum(combo) == C:
            solutions.append(combo)
    return solutions


n = int(input("Enter number of variables (n): "))
C = int(input("Enter constant C (<=10): "))

if C > 10:
    print("C must be <= 10")
else:
    solutions = find_solutions(n, C)
    print(f"\nTotal solutions: {len(solutions)}")
    for sol in solutions:
        print(" + ".join(map(str, sol)), "=", C)
```

---

## Practical 5: Evaluate a Polynomial Function

```python
def evaluate_polynomial(coeffs, n):
    """
    coeffs[i] is the coefficient of x^i
    e.g., f(x) = 4x^2 + 2x + 9  =>  coeffs = [9, 2, 4]
    """
    result = 0
    for i, c in enumerate(coeffs):
        result += c * (n ** i)
    return result


degree = int(input("Enter degree of polynomial: "))
coeffs = []
print("Enter coefficients from constant term to highest degree:")
for i in range(degree + 1):
    c = float(input(f"  Coefficient of x^{i}: "))
    coeffs.append(c)

x = float(input("Enter value of x: "))
print(f"\nf({x}) = {evaluate_polynomial(coeffs, x)}")

# Example: f(x) = 4x^2 + 2x + 9  at x=5
# coeffs = [9, 2, 4], x = 5  =>  f(5) = 9 + 10 + 100 = 119
```

---

## Practical 6: Check if a Graph is a Complete Graph (Adjacency Matrix)

```python
def is_complete_graph(matrix, n):
    """
    A complete graph Kn has edges between every pair of distinct vertices.
    In adjacency matrix: all off-diagonal entries must be 1,
    and diagonal entries must be 0 (no self-loops).
    """
    for i in range(n):
        for j in range(n):
            if i == j:
                if matrix[i][j] != 0:
                    return False
            else:
                if matrix[i][j] != 1:
                    return False
    return True


n = int(input("Enter number of vertices: "))
print(f"Enter {n}x{n} adjacency matrix row by row (space-separated):")
matrix = [list(map(int, input().split())) for _ in range(n)]

if is_complete_graph(matrix, n):
    print(f"\nThe graph is a Complete Graph K{n}.")
else:
    print(f"\nThe graph is NOT a Complete Graph.")
```

---

## Practical 7: In-Degree and Out-Degree of Each Vertex in a Directed Graph

```python
def compute_degrees(matrix, n):
    in_degree  = [0] * n
    out_degree = [0] * n

    for i in range(n):
        for j in range(n):
            if matrix[i][j] == 1:
                out_degree[i] += 1
                in_degree[j]  += 1

    return in_degree, out_degree


n = int(input("Enter number of vertices: "))
print(f"Enter {n}x{n} adjacency matrix of directed graph row by row:")
matrix = [list(map(int, input().split())) for _ in range(n)]

in_deg, out_deg = compute_degrees(matrix, n)

print("\nVertex | In-Degree | Out-Degree")
print("-------|-----------|----------")
for i in range(n):
    print(f"  v{i+1}   |     {in_deg[i]}     |     {out_deg[i]}")
```

---

> **Note:** All programs are written in Python 3. Each practical can be saved as a separate `.py` file and run directly from the terminal.
