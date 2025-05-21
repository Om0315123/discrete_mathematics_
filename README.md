# discrete_mathematics_

## PRAC 1 
```python
class SET:
    def __init__(self, elements=None):
        if elements is None:
            self.elements = set()
        else:
            self.elements = set(elements)
    
    def ismember(self, element):
        return element in self.elements
    
    def powerset(self):
        elems = list(self.elements)
        power_set = []
        n = len(elems)
        for i in range(2**n):
            subset = set()
            for j in range(n):
                if i & (1 << j):
                    subset.add(elems[j])
            power_set.append(subset)
        return power_set
    
    def subset(self, other):
        return self.elements.issubset(other.elements)
    
    def union(self, other):
        return self.elements.union(other.elements)
    
    def intersection(self, other):
        return self.elements.intersection(other.elements)
    
    def complement(self, universal):
        return set(universal).difference(self.elements)
    
    def set_difference(self, other):
        return self.elements.difference(other.elements)
    
    def symmetric_difference(self, other):
        return self.elements.symmetric_difference(other.elements)
    
    def cartesian_product(self, other):
        return {(a, b) for a in self.elements for b in other.elements}


def menu():
    print("\n--- Set Operations Menu ---")
    print("1. Check if element is member of the set")
    print("2. Power set")
    print("3. Check subset")
    print("4. Union of two sets")
    print("5. Intersection of two sets")
    print("6. Complement of the set")
    print("7. Set difference")
    print("8. Symmetric difference")
    print("9. Cartesian product")
    print("0. Exit")

def get_set_input(prompt="Enter elements separated by space: "):
    elems = input(prompt).strip().split()
    return SET(elems)

def main():
    print("Create the first set:")
    set1 = get_set_input()

    while True:
        menu()
        choice = input("Enter your choice: ").strip()
        
        if choice == '0':
            print("Exiting...")
            break
        
        elif choice == '1':
            elem = input("Enter element to check membership: ")
            print(f"Is '{elem}' a member of the set? {set1.ismember(elem)}")

        elif choice == '2':
            ps = set1.powerset()
            print("Power set:")
            for subset in ps:
                print(subset)

        elif choice == '3':
            print("Create the second set to check subset:")
            set2 = get_set_input()
            print(f"Is first set a subset of second? {set1.subset(set2)}")

        elif choice == '4':
            print("Create the second set for union:")
            set2 = get_set_input()
            result = set1.union(set2)
            print("Union:", result)

        elif choice == '5':
            print("Create the second set for intersection:")
            set2 = get_set_input()
            result = set1.intersection(set2)
            print("Intersection:", result)

        elif choice == '6':
            universal = input("Enter Universal Set elements separated by space: ").strip().split()
            result = set1.complement(universal)
            print("Complement of set with respect to universal set:", result)

        elif choice == '7':
            print("Create the second set for set difference (set1 - set2):")
            set2 = get_set_input()
            result = set1.set_difference(set2)
            print("Set difference:", result)

        elif choice == '8':
            print("Create the second set for symmetric difference:")
            set2 = get_set_input()
            result = set1.symmetric_difference(set2)
            print("Symmetric difference:", result)

        elif choice == '9':
            print("Create the second set for cartesian product:")
            set2 = get_set_input()
            result = set1.cartesian_product(set2)
            print("Cartesian product:")
            for pair in result:
                print(pair)

        else:
            print("Invalid choice, please try again.")

if __name__ == "__main__":
    main()

```
## PRAC 2 

```PYTHON
class RELATION:
    def __init__(self, matrix):
        self.matrix = matrix
        self.size = len(matrix)

    def is_reflexive(self):
        for i in range(self.size):
            if self.matrix[i][i] != 1:
                return False
        return True

    def is_symmetric(self):
        for i in range(self.size):
            for j in range(self.size):
                if self.matrix[i][j] != self.matrix[j][i]:
                    return False
        return True

    def is_anti_symmetric(self):
        for i in range(self.size):
            for j in range(self.size):
                if i != j and self.matrix[i][j] == 1 and self.matrix[j][i] == 1:
                    return False
        return True

    def is_transitive(self):
        for i in range(self.size):
            for j in range(self.size):
                if self.matrix[i][j] == 1:
                    for k in range(self.size):
                        if self.matrix[j][k] == 1 and self.matrix[i][k] != 1:
                            return False
        return True

    def relation_type(self):
        reflexive = self.is_reflexive()
        symmetric = self.is_symmetric()
        anti_symmetric = self.is_anti_symmetric()
        transitive = self.is_transitive()

        if reflexive and symmetric and transitive:
            return "Equivalence Relation"
        elif reflexive and anti_symmetric and transitive:
            return "Partial Order Relation"
        else:
            return "None"


def menu():
    print("\n--- Relation Properties Menu ---")
    print("1. Check Reflexive")
    print("2. Check Symmetric")
    print("3. Check Anti-symmetric")
    print("4. Check Transitive")
    print("5. Check Relation Type")
    print("0. Exit")

def input_matrix(size):
    print(f"Enter the relation matrix row-wise (only 0 or 1) for size {size}x{size}:")
    matrix = []
    for i in range(size):
        while True:
            row = input(f"Row {i+1}: ").strip().split()
            if len(row) != size or any(x not in ['0', '1'] for x in row):
                print(f"Invalid input. Enter exactly {size} elements as 0 or 1.")
                continue
            matrix.append([int(x) for x in row])
            break
    return matrix

def main():
    size = int(input("Enter the number of elements in the set (size of relation matrix): "))
    matrix = input_matrix(size)

    relation = RELATION(matrix)

    while True:
        menu()
        choice = input("Enter your choice: ").strip()

        if choice == '0':
            print("Exiting...")
            break

        elif choice == '1':
            print("Reflexive:", relation.is_reflexive())

        elif choice == '2':
            print("Symmetric:", relation.is_symmetric())

        elif choice == '3':
            print("Anti-symmetric:", relation.is_anti_symmetric())

        elif choice == '4':
            print("Transitive:", relation.is_transitive())

        elif choice == '5':
            print("Relation Type:", relation.relation_type())

        else:
            print("Invalid choice, try again.")

if __name__ == "__main__":
    main()
```
## PRAC 3
```PYTHON
from itertools import permutations, product

def permutations_without_repetition(digits):
    # Generate all permutations without repetition
    return list(permutations(digits, len(digits)))

def permutations_with_repetition(digits, length):
    # Generate all permutations with repetition allowed of given length
    return list(product(digits, repeat=length))

def main():
    digits = input("Enter digits separated by space: ").strip().split()
    
    choice = input("Allow repetition? (y/n): ").strip().lower()
    
    if choice == 'y':
        length = int(input("Enter the length of each permutation: "))
        perms = permutations_with_repetition(digits, length)
    else:
        perms = permutations_without_repetition(digits)
    
    print("Generated permutations:")
    for p in perms:
        print(''.join(p))


```
## PRAC 4
```PYTHON
from itertools import product

def find_solutions(n, C):
    solutions = []
    # Generate all n-tuples where each element is in [0..C]
    for combo in product(range(C+1), repeat=n):
        if sum(combo) == C:
            solutions.append(combo)
    return solutions

def main():
    n = int(input("Enter the number of variables (n): "))
    C = int(input("Enter the constant C (<=10): "))
    
    if C > 10:
        print("C must be less than or equal to 10.")
        return
    
    solutions = find_solutions(n, C)
    print(f"All solutions to x1 + x2 + ... + x{n} = {C}:")
    for sol in solutions:
        print(sol)

if __name__ == "__main__":
    main()
```
## PRAC 5
```PYTHON
def evaluate_polynomial(coefficients, n):
    result = 0
    for power, coeff in enumerate(coefficients):
        result += coeff * (n ** power)
    return result

def main():
    degree = int(input("Enter the degree of the polynomial: "))
    
    coefficients = []
    print("Enter coefficients for each term starting from constant term (n^0) to n^degree:")
    for i in range(degree + 1):
        coeff = float(input(f"Coefficient for n^{i}: "))
        coefficients.append(coeff)
    
    n = float(input("Enter the value of n to evaluate the polynomial: "))
    
    value = evaluate_polynomial(coefficients, n)
    print(f"Value of the polynomial at n = {n} is: {value}")

if __name__ == "__main__":
    main()
```
## PRAC 6 
```PYTHON
def is_complete_graph(adj_matrix):
    n = len(adj_matrix)
    for i in range(n):
        for j in range(n):
            if i == j:
                # diagonal should be 0
                if adj_matrix[i][j] != 0:
                    return False
            else:
                # off-diagonal should be 1
                if adj_matrix[i][j] != 1:
                    return False
    return True

def main():
    n = int(input("Enter number of vertices: "))
    adj_matrix = []

    print("Enter adjacency matrix row by row (0 or 1):")
    for _ in range(n):
        row = input().strip().split()
        row = [int(x) for x in row]
        adj_matrix.append(row)

    if is_complete_graph(adj_matrix):
        print("Graph is complete.")
    else:
        print("Graph is NOT complete.")

if __name__ == "__main__":
    main()

```
## PRAC 7
```PYTHON
def is_complete_graph(adj_list):
    n = len(adj_list)
    for vertex in adj_list:
        neighbors = adj_list[vertex]
        # Check if vertex is connected to all other vertices except itself
        if len(neighbors) != n - 1:
            return False
    return True

def main():
    n = int(input("Enter number of vertices: "))
    adj_list = {}

    print("Enter neighbors of each vertex (space separated):")
    for i in range(n):
        neighbors = input(f"Neighbors of vertex {i}: ").strip().split()
        # Convert neighbors to integers
        neighbors = [int(x) for x in neighbors]
        adj_list[i] = neighbors

    if is_complete_graph(adj_list):
        print("Graph is complete.")
    else:
        print("Graph is NOT complete.")

if __name__ == "__main__":
    main()
```
## PRAC 8 
```PYTHON
def compute_in_out_degree(adj_list, n):
    in_degree = [0] * n
    out_degree = [0] * n
    
    for vertex in range(n):
        neighbors = adj_list.get(vertex, [])
        out_degree[vertex] = len(neighbors)
        for neighbor in neighbors:
            in_degree[neighbor] += 1
    return in_degree, out_degree

def main():
    n = int(input("Enter number of vertices: "))
    adj_list = {}
    
    print("Enter neighbors for each vertex (directed edges):")
    for i in range(n):
        neighbors = input(f"Outgoing edges from vertex {i} (space separated): ").strip().split()
        if neighbors == ['']:
            neighbors = []
        else:
            neighbors = [int(x) for x in neighbors]
        adj_list[i] = neighbors
    
    in_deg, out_deg = compute_in_out_degree(adj_list, n)
    
    print("\nVertex : In-degree | Out-degree")
    for i in range(n):
        print(f"{i} : {in_deg[i]} | {out_deg[i]}")

if __name__ == "__main__":
    main()
```
